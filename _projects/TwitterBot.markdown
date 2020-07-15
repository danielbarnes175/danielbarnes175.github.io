---
layout: project
permalink: /portfolio/TwitterBot
---

![](https://media.discordapp.net/attachments/494527046345555969/733022435640868905/unknown.png)

<svg class="svg-icon"><use xlink:href="{{ '/assets/minima-social-icons.svg#github' | relative_url }}"></use></svg> [View Source](https://github.com/danielbarnes175/happydaytwitterbot)

[https://twitter.com/happydaybot2](https://twitter.com/happydaybot2)

# Happy Day Twitter Bot

HappyDayBot is a Twitter bot created to randomly generate what makes today special. To randomly generate the "happy day" it uses the formula "Happy" + adjective + noun + abstract noun + "Day!"

It is a relatively simple bot that utilizes the `twit` npm package. The bot is very straightforward, and uses this one file to drive the code.

```
const botSettings = require("./botSettings.json");
const twit = require("twit");
const fs = require("fs");

//text1 is the list of adjectives
//text2 is the list of nouns
//text3 is the list of abstract nouns
const text1 = fs.readFileSync("./words/adjectivelist.txt").toString('utf-8');
const text2 = fs.readFileSync("./words/nounlist.txt").toString('utf-8');
const text3 = fs.readFileSync("./words/abstractnounlist.txt").toString('utf-8');

let timeBetweenTweets = 1000 * 60 * 60;

let Twitter = new twit(botSettings);
let currentTime = new Date();
console.log("Bot online! Current time is " + currentTime);


let tweet_day = function() {
	//Find the adjective to use
	var adjByLine = text1.split("\n");
	var whichAdj = Math.floor(Math.random() * adjByLine.length);
	var adj = adjByLine[whichAdj];
	adj = (adj.charAt(0).toUpperCase() + adj.slice(1)).trim();

	//Find the noun to use
	var textByLine = text2.split("\n")
	var whichNoun = Math.floor(Math.random() * textByLine.length);
	var noun = textByLine[whichNoun];
	noun = (noun.charAt(0).toUpperCase() + noun.slice(1)).trim();

	//Find the abstract noun to use
	var abstractByLine = text3.split("\n");
	var whichAbs = Math.floor(Math.random() * abstractByLine.length);
	var abstract = abstractByLine[whichAbs];
	abstract = abstract.trim();
	abstract = (abstract.charAt(0).toUpperCase() + abstract.slice(1)).trim();

	let tweet = `Happy ${adj} ${noun} ${abstract} Day!`;

	let now = new Date();
	Twitter.post('statuses/update', { status: tweet}, function(err, data, response) {
		if (err) {
			console.error(err.message);
			console.log("Error at " + now);
		} else {
			//console.log(data);
			console.log("Successfully tweeted at " + now + "!");
		}
	});
}

tweetLoop();

function tweetLoop() {
	(function loop() {
	    var now = new Date();
	    if (/*now.getDate() === 6 && */now.getHours() === 7 /*&& now.getMinutes() === 46*/) {
	        tweet_day();
		}
	    now = new Date();
	    var delay = timeBetweenTweets - (now % timeBetweenTweets);
	    setTimeout(loop, delay);
	})();
}
```

# Why?

Why did I create this? I wanted something that could make me laugh every day, and give me something to be happy about the day. This bot does its job. Some of the randomly generated tweets are great, and really seem like something that could actually be a holiday. I strongly recommend you give the twitter account a follow.