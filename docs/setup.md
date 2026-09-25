---
title: My Development Setup
nav_order: 5
---

# My Development Setup

I do all of my development on a phone. This page is a snapshot of the whole
setup — the hardware, the Linux layer, the AI coding agent, and the model behind
it — plus the steps to reproduce it.

*Recorded 2026-09-25. Versions move; treat everything below as a point-in-time snapshot.*

## TL;DR

| Layer | What I run |
|---|---|
| Hardware | Samsung Galaxy A52s 5G (Snapdragon 778G, 7.2 GiB RAM) |
| Host OS | Android 14 |
| Linux layer | Termux + **PRoot-Distro** (Debian 13, no root) |
| Agent | **OpenCode** v2.0.16 |
| Provider | **OpenRouter** |
| Model | **DeepSeek Flash Latest** |

A mid-range 2021 phone, a userspace Linux distro that needs no root, and a
terminal-based AI agent. No laptop, no Docker, no cloud dev box.

## Hardware

The machine is a **Samsung Galaxy A52s 5G**, model `SM-A528B` (device codename
`a52sxq`).

| Component | Detail |
|---|---|
| SoC | Qualcomm **Snapdragon 778G** (`SM7325`, platform "lahaina", QTI) |
| CPU | 8 cores — Cortex-A78 + Cortex-A55, up to ~1.8 GHz |
| Architecture | `arm64-v8a` |
| RAM | 7.2 GiB |
| Swap | 4 GiB |
| Storage | 226 GB total, ~16 GB used |

It is not a fast machine. It is, however, always in my pocket, always on, and
entirely sufficient for editing text, running Git, and driving an agent that
does the heavy lifting elsewhere.

## OS and container

There are three layers here, and none of them is Docker.

### 1. Android (host)

- **Android 14** (SDK 34), build `UP1A.231005.007.A528BZTSBGYI3`.

### 2. Termux (Linux userspace)

[Termux](https://termux.dev) is a terminal emulator and Linux environment for
Android. It provides the shell and the package manager that everything else is
built on. It does **not** require root.

### 3. PRoot-Distro (the "container")

The Linux distribution itself runs under **[PRoot-Distro](https://github.com/termux/proot-distro)**,
which uses [PRoot](https://proot-me.github.io/) — a `ptrace`-based userspace
implementation of `chroot`, `mount --bind`, and friends.

- Guest distribution: **Debian GNU/Linux 13 (trixie)**, `aarch64`
- Kernel string: `6.17.0-PRoot-Distro`
- **No root, no unlocked bootloader, no custom ROM.**

**Why not Docker?** Docker (and containers generally) rely on kernel namespaces
and cgroup privileges that a stock, unrooted Android ROM does not hand to a
normal app. PRoot sidesteps that entirely by intercepting syscalls in userspace,
so it works as an ordinary app. The trade-off is overhead: PRoot adds latency to
syscall-heavy work, which is fine for a terminal workflow but not for anything
CPU-bound.

## The agent

The AI coding agent is **[OpenCode](https://opencode.ai)**, currently **v2.0.16**,
running inside the PRoot Debian rootfs as a terminal application.

I also wire in a **remote GitHub MCP server** so the agent can read and write
repositories directly. The relevant part of `~/.config/opencode/opencode.json`:

```json
{
  "mcp": {
    "servers": {
      "github": {
        "type": "remote",
        "url": "https://api.githubcopilot.com/mcp/",
        "headers": {
          "Authorization": "Bearer {env:GITHUB_PAT}"
        }
      }
    }
  }
}
```

The token is pulled from an environment variable (`{env:GITHUB_PAT}`) rather than
committed to the file — a habit worth keeping, since this config lives in the
same tree the agent edits.

## Provider and model

- **Provider:** [OpenRouter](https://openrouter.ai) — a single endpoint that
  fronts many models, which keeps provider switching to a config change.
- **Model:** **DeepSeek Flash Latest** — fast and cheap, and good enough for the
  edit-and-verify loop that most of my work is.

Because the agent runs on the phone but inference happens in the cloud, the phone
only has to be good at being a terminal. The heavy compute is somebody else's
problem.

## Replicating it

Roughly 15 minutes, assuming a recent Android phone and a network connection.

**1. Install Termux.** Get it from [F-Droid](https://f-droid.org/packages/com.termux/),
not the Play Store — the Play Store build is stale and its packages are broken.

**2. Update and install the proot tooling.**

```bash
pkg update && pkg upgrade
pkg install proot-distro
```

**3. Install and enter Debian.**

```bash
proot-distro install debian
proot-distro login debian
```

**4. Install the basics inside Debian.**

```bash
apt update && apt upgrade -y
apt install -y git curl python3 python3-pip
```

**5. Install Node.js via nvm.** Debian's packaged Node tends to lag; nvm keeps it current.

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.8/install.sh | bash
source ~/.bashrc
nvm install --lts
```

**6. Install OpenCode.**

```bash
curl -fsSL https://opencode.ai/install | bash
```

**7. Configure a provider.** Launch `opencode`, run `/connect`, pick OpenRouter,
and paste an API key. Then choose the model (DeepSeek Flash) in the model picker.

**8. (Optional) Add the GitHub MCP server** using the JSON snippet above, and put
a personal access token in `GITHUB_PAT`.

**9. Verify.**

```bash
opencode --version
```

### Toolchain snapshot

| Tool | Version |
|---|---|
| git | 2.47.3 |
| Node.js | v24.21.0 |
| npm | 11.19.0 |
| Python | 3.14.6 |
| pip | 26.2.1 |

Notably absent: **Ruby, Jekyll, `gh`, Docker, and VS Code**. None of them are
needed here. This very site is built by **GitHub Actions** in the cloud, so the
phone never has to run Jekyll at all — it only has to push Markdown.
