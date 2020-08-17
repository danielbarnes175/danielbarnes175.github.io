---
layout: project
permalink: /portfolio/DiscordBot
---
# Discord Bot

![An example of using the bot](https://i.imgur.com/LPslyRj.png)

A Discord bot used for managing servers and other various functions. It's created with Node.js and Discord.js.

<svg class="svg-icon"><use xlink:href="{{ '/assets/minima-social-icons.svg#github' | relative_url }}"></use></svg> [View Source](https://github.com/danielbarnes175/DanielBot) 

### Strategy

When I first started this Bot, I wasn't sure exactly what I wanted it to do. I wanted to use it primarily as a way to learn javascript. To do this, I would need to create the bot in a way such that it is easily extendable and modifiable.

#### Bot.js

Bot.js is a file that is the primary driver for the bot. It puts the bot online, and grabs the commands that can be called. It also handles the listening for calling commands.

Let's take a look at bot.js

```
const botSettings = require("./botSettings.json");
const Discord = require("discord.js");
const fs = require("fs");

const prefix = botSettings.prefix;

const bot = new Discord.Client({disableEveryone: true});
bot.commands = new Discord.Collection();

fs.readdir("./cmds/", (err, files) => {
	if (err) console.error(err);

	let jsfiles = files.filter(f => f.split(".").pop() === "js");
	if (jsfiles.length <= 0) {
		console.log("No commands to load.")
		return;
	}

	console.log(`Loading ${jsfiles.length} commands!`);

	jsfiles.forEach((f, i) => {
		let props = require(`./cmds/${f}`);
		console.log(`${i+1}: ${f} loaded!`);
		bot.commands.set(props.help.name, props);
	});
});

bot.on('error', err => {
	console.error(err);
});

bot.on("ready", async () => {
	console.log(` `);
	console.log(`Servers bot is connected to [${bot.guilds.size}]:`);

	bot.guilds.forEach((f, i) => {
	    console.log(`${f} connected.`);
	});

	console.log(` `);
	console.log('Bot is ready! ' + bot.user.username);

	//Creates the invite link and puts it in the console
	try {
		let link = await bot.generateInvite(["ADMINISTRATOR"]);
		console.log(link);
	} catch(e) {
		console.log(e.stack);
	}
});

bot.on("message", async message => {
	//If it's another bot or if its in a dm, don't do anything.
	if (message.author.bot) return;
	if (message.channel.type === "dm") return;

	let messageArray = message.content.split(" ");
	let command = messageArray[0];
	let args = messageArray.slice(1);

	//Ignore if they don't start with '~'
	if (!command.startsWith(prefix)) return;

	let cmd = bot.commands.get(command.slice(prefix.length));
	if (cmd) cmd.run(bot, message, args);

});

bot.login(botSettings.token);
```

#### /cmds/

/cmds/ is the directory where I hold all of my commands. Each command is contained within its own file. This allows me to easily know where to modify code for a specific command rather than being forced to go through multiple files. I kept most commands simple so they rarely needed to call external files. If they did, I would still know where to go to find the root. Since my code is open source, it also allows others to look through my code and easily navigate it.

Here are the current commands:

![The /cmds/ directory](https://i.imgur.com/SqWvFX6.png)

Let's take moron.js as an example to show exactly what the format for creating a new command looks like.

```
/*
 This command logs in the console that the person running the bot is a moron.
 There will be no noticeable output for regular users using the command.

 Usage is defined as ~moron
*/
module.exports.run = async (bot, message, args) => {
	console.log("You're a moron");
}

module.exports.help = {
	name: "moron",
	description: "Call the person running the bot a moron. You won't see the message, but they will.",
	usage: "~moron"
}
```

moron.js is a command used for logging important information. You can see that every command has a run function, which is what is called when someone calls the command, for example, `~moron` and a help function. This is called when someone asks for help about a specific command, for example `~help moron`.

### Want DanielBot in your server?

Just for full disclosure, I've stopped development on DanielBot, so things could break (For example, 3rd party APIs that are used), but if you really want to add DanielBot to your server, there's a link on the [GitHub](https://github.com/danielbarnes175/DanielBot)
