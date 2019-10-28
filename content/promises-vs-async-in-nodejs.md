Title: Promises vs Async in Node
Date: 2019-10-28 20:20
Modified: 2019-10-28 20:20
Category: Node
Tags: programming
Authors: Jack

A couple years ago, promises save me from nested callbacks. Now, async takes over promises to make code looks cleaner. Although, they have their own pros and cons. Async is the way to go for now. Let's take a look both of them.

## Promise

```javascript
var fetchDaily = require("./fetchDaily.js")

var getDailyPromises = new Promise(function(resolve, reject) {
	fetch("http://www.twse.com.tw/exchangeReport/STOCK_DAY_ALL?response=json")
		.then(function(res) { return res.json() })
		.then(function(json) { resolve(json.data) })
})


fetchDaily.getDailyPromises
	.then(function(res) { console.log(res.length) })
```

## Async

```javascript
var fetchDaily = require("./fetchDaily.js")

async function getDaily() {
	let res = await fetch("http://www.twse.com.tw/exchangeReport/STOCK_DAY_ALL?response=json")
	let json = await res.json()

	return json.data
}

async function main() {
	let data = await fetchDaily.getDaily()

	console.log(data.length)	
}

main()
```

Which one do you like?

