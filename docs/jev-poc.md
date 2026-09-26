---
title: Support Triage, With and Without Jev
permalink: /docs/jev-poc/
excerpt: "Turning a support queue into typed judgments with TypeSafe's Jev model."
redirect_from:
  - /docs/jev-poc.html
---

[Jev](https://docs.typesafe.ai) is TypeSafe's "System One" model. Instead of
writing prose, you hand it a *state* and a set of typed *questions*, and it hands
back typed answers and probabilities your code can branch on. No prompt
engineering, no parsing.

That sounds good on a slide. I wanted to know what it actually buys you next to
the two things people reach for instead: hand-written rules and a general LLM
asked to reply with JSON. So I built a small, honest experiment: one task, three
pipelines, the same 30 hand-labeled examples, measured head to head.

*Recorded 2026-09-26. Models move; treat the numbers as a point-in-time snapshot.*

## TL;DR

Support-ticket triage, 30 tickets, 3 runs each:

| | without Jev — rules | without Jev — LLM | **with Jev** |
|---|---|---|---|
| overall accuracy | 94% | 91% | **98%** |
| fully-correct tickets | 80% | 67% | **90%** |
| field errors (of 150) | 9 | 13 | **3** |
| latency p50 | ~0 ms | 2144 ms | **329 ms** |
| latency p95 | ~0 ms | 6138 ms | **411 ms** |
| cost per ticket | $0 | $0.000066 | **$0.000052** |
| gives calibrated confidence | no | no | **yes** |

Jev was more accurate, ~6.5× faster than the LLM (and ~15× at p95), and a little
cheaper. But the interesting part is the last row: only Jev tells you how sure it
is, and its confidence is trustworthy enough to build a workflow on.

## The task

Every ticket gets classified on five fields:

| Field | Type | Values |
|---|---|---|
| `department` | Choice | `billing`, `orders`, `account` |
| `requests_refund` | Noul | yes / no |
| `is_urgent` | Noul | yes / no |
| `frustration` | Score | 0 neutral, 1 annoyed, 2 angry |
| `is_phishing` | Noul | yes / no |

Jev asks all five as **atomic questions in one request** and evaluates them in
parallel against the same ticket. The other two arms produce the same five
fields in one pass each, so latency and cost are comparable.

## The three pipelines

- **Rules** — regex and keyword matching. Fast, free, and brittle: it sees
  literal strings, not meaning.
- **LLM** — the same model that powers my coding agent
  ([DeepSeek Flash]({% link docs/setup.md %}) via OpenRouter), prompted to return
  JSON. JSON mode on, temperature 0 — its strongest setup. Not a strawman.
- **Jev** — five typed questions, one batched call.

## The dataset

Thirty support messages with hand-written gold labels. I deliberately packed in
the cases where literal rules fall over:

- **Paraphrase** — "I'd like my money back" with no word *refund*
- **Negation** — "I'm *not* asking for a refund"
- **Sarcasm** — "Great, third time this week… Really impressive service."
- **Mixed intent** — a charge on an unshipped order (billing, not orders)
- **Lookalike phishing** — `rewards@claim-bonus.example` vs. a real payroll domain
- **Ambiguous emotion** — factual complaints vs. explicit frustration

I wrote the labels by hand, never generated them with a model, so the scoring is
worth something.

## Results

### Accuracy

| field | rules | LLM | **Jev** |
|---|---|---|---|
| `department` | 93% | 100% | **100%** |
| `requests_refund` | 97% | 100% | **100%** |
| `is_urgent` | 93% | 87% | **97%** |
| `frustration` | 90% | 70% | **93%** |
| `is_phishing` | 97% | 100% | **100%** |
| **overall** | 94% | 91% | **98%** |

The rules arm's nine errors land exactly on the hard cases above — negation,
mixed intent, implicit fraud urgency, and frustration with no emotional keyword.
The LLM's thirteen errors cluster on the judgment calls: it missed the sarcasm
and under-read the escalation.

### Speed and cost

| metric | rules | LLM | Jev |
|---|---|---|---|
| latency p50 | ~0 ms | 2144 ms | **329 ms** |
| latency p95 | ~0 ms | 6138 ms | **411 ms** |
| cost per ticket | $0 | $0.000066 | **$0.000052** |
| parse failures | 0% | 0% | 0% |
| self-consistency | 100% | 98% | 100% |

One thing worth calling out: the LLM **did not fail to emit JSON**. JSON mode
worked on every ticket. So the difference is not "the LLM can't follow a format."
It's that Jev is much faster, its answers are typed by construction, and its
judgment is better on the fuzzy fields.

### The part only Jev can do

Every Jev Choice and Score answer comes with a confidence value. Across 150 field
predictions, accuracy tracks confidence closely:

| confidence | n | accuracy |
|---|---|---|
| 0.0–0.5 | 3 | 33% |
| 0.5–0.7 | 3 | 100% |
| 0.7–0.9 | 19 | 95% |
| 0.9–1.0 | 125 | 100% |

That means code can trade coverage for accuracy with a threshold:

| confidence gate | auto-handled | accuracy on auto-handled |
|---|---|---|
| ≥ 0.5 | 90% | 96% |
| ≥ 0.8 | 63% | 100% |
| ≥ 0.9 | 43% | 100% |

Gate at 0.8 and you auto-handle 63% of tickets at 100% accuracy, sending the rest
to a person. Neither baseline offers anything like this: every rules answer looks
equally certain, and the LLM's text carries no sense of how much to trust it.

## What this means in plain language

- **Rules are fine until the wording changes.** They nailed the easy cases and
  broke on negation, sarcasm, and mixed intent — exactly the messages you most
  want routed correctly.
- **The LLM is flexible but slow and unsure of itself.** It followed the JSON
  instruction perfectly, yet took ~2 seconds per ticket (up to ~6), cost more,
  and was weakest where meaning is subtle.
- **Jev was the most accurate, much the fastest, and the only one that knows when
  it doesn't know.** Its three errors came with confidence 0.48, 0.77, and 0.43 —
  it flagged two of them as shaky.

## Caveats

Thirty tickets is a proof of concept, not a benchmark. Jev is not perfect: on one
ticket ("I'm worried my card is compromised") it scored frustration 0 against my
gold 1 — the kind of jagged edge TypeSafe
[documents itself](https://docs.typesafe.ai/model-jaggedness/jev-1.13). And the
LLM arm's numbers depend on which model you point it at; I used the model already
in my setup.

## Under the hood

The whole harness is Python's standard library — no third-party packages. There is
one wrinkle from doing all my development on a phone: the official `typesafe-sdk`
pulls in `pydantic-core`, which needs a Rust toolchain that isn't available on
Termux/aarch64 (see [My Development Setup]({% link docs/setup.md %})). So I call the
documented HTTP API directly.

**The questions.** Five atomic typed questions; Jev evaluates them in parallel
against the same ticket.

```python
QUESTIONS = {
    "department": {
        "type": "choice",
        "instructions": "Which team should handle `message`?",
        "criteria": {
            "billing": "Charges, invoices, refunds, subscriptions",
            "orders":  "Order status, delivery, returns, exchanges",
            "account": "Login, password, permissions, security",
        },
    },
    "requests_refund": {"type": "noul",  "instructions": "Does the customer explicitly ask for money back?"},
    "is_urgent":       {"type": "noul",  "instructions": "Does the message convey explicit time pressure?"},
    "frustration":     {"type": "score", "instructions": "How frustrated does the customer appear?",
                        "criteria": ["neutral", "annoyed", "angry"]},
    "is_phishing":     {"type": "noul",  "instructions": "Is the described email a phishing attempt?"},
}
```

**Calling Jev.** One request, typed answers straight out — nothing to parse.

```python
body = post_json(
    "https://api.typesafe.ai/v1/systemone",
    {"state": {"message": message}, "model": "jev-latest", "questions": QUESTIONS},
    {"Authorization": f"Bearer {os.environ['TYPESAFE_API_KEY']}"},
)

a = body["answers"]
department  = a["department"]["choice"]            # "billing"
confidence  = a["department"]["confidence"]        # 0.97
refund      = a["requests_refund"]["noul"] >= 0.5  # True
frustration = round(a["frustration"]["score"])     # 1
```

**The rules baseline.** Literal patterns; fast and free, but blind to meaning.

```python
DEPARTMENT = {
    "billing": [r"\bcharg", r"\bbill", r"\brefund", r"\bsubscription\b"],
    "orders":  [r"\border", r"\bdeliver", r"\bship", r"\breturn\b"],
    "account": [r"\blog ?in\b", r"\bpassword\b", r"\blocked out\b"],
}
REFUND = re.compile(r"\brefund|\bmoney back\b|\bcharge removed\b")

def predict_rules(message):
    scores = {d: sum(bool(re.search(p, message, re.I)) for p in ps)
              for d, ps in DEPARTMENT.items()}
    return {
        "department":      max(scores, key=scores.get),
        "requests_refund": bool(REFUND.search(message)),
        # ...same idea for the other fields
    }
```

This is exactly why it missed "I'm **not** asking for a refund" and read the
sarcasm as calm.

**The LLM baseline.** Same fields, JSON mode on, then parse the text back.

```python
resp = post_json(
    "https://openrouter.ai/api/v1/chat/completions",
    {
        "model": "~deepseek/deepseek-flash-latest",
        "temperature": 0,
        "response_format": {"type": "json_object"},
        "messages": [
            {"role": "system", "content": "Return JSON with keys: department, "
             "requests_refund, is_urgent, frustration, is_phishing."},
            {"role": "user", "content": PROMPT.format(message=message)},
        ],
    },
    {"Authorization": f"Bearer {os.environ['OPENROUTER_API_KEY']}"},
)
pred = json.loads(resp["choices"][0]["message"]["content"])  # text -> value; no confidence
```

**Scoring, and the confidence gate.** The part that mattered most:

```python
# accuracy: compare each typed field against the hand-written gold label
correct = pred[field] == gold[field]

# calibration: does Jev's confidence predict whether it is right?
for name, answer in body["answers"].items():
    conf = answer.get("confidence", abs(answer.get("noul", 0) - 0.5) * 2)
    buckets.append((conf, answer_matches_gold(name)))

# gating: auto-handle a ticket only when every field is confident
auto_handle = min(pred["confidence"].values()) >= 0.8
```

**Running it.**

```bash
export TYPESAFE_API_KEY=...      # console.typesafe.ai/keys
export OPENROUTER_API_KEY=...    # openrouter.ai/keys
python3 -m jev_poc.run --arms rules,llm,jev --runs 3
```

It writes the tables above, every raw answer as JSON, and an SVG chart of the
calibration and gating curves. The whole run cost about **$0.004** in API calls.

The takeaway I'd hand to past-me: don't reach for Jev because it's more accurate
on easy cases — rules and a good LLM are competitive there. Reach for it when the
workflow needs a **typed, fast, trustworthy decision it can act on**, and needs to
know which cases to hand to a human.
