---
title: "Create My First 24/7 Telegram Bot On Python"
date: 2018-12-05
draft: false
tags: ["python", "telegram bot", "short tutorials that work"]
---

> Hello!

# What this post is about
So today I am going to write about how I (1) created my first Telegram bot on python, and (2) manage to host it 24/7 for free.

The reason I am writing this is because I had encountered some issues while looking for a beginner tutorial teaching me how to write telegram bots. 

And after writing the bot, I wanted my bot to be live without having me to pay for anything. I ran into some issues hosting them and getting the bot to run 24/7, and had to rewrite my entire code. Thankfully it was a simple bot, so there wasn't much code to change.

_TLDR:_ 
Go to section **Coding the BOT** to dive straight into the details.

Note: There are many ways to code a telegrambot and I am just showing one of the many ways. 

---

# What the bot is about

### Problem
I lived in a hostal where breakfast is daily except Sunday and dinner is provided daily except Saturday. At the start of the semester, we were provided with an excel sheet of the daily menu. So just imagine an excel sheet with the dinner menus from August up till December. It wasn't ugly, but it ain't pretty either. If I wanted to find out what was served for dinner that evening, I would have to pull up that excel sheet and navigate through the multiple sheets and cells to get the information I need. Hence a group of friends and I decided to come together to create a mealbot that will return us the menu of the day.

### What I need
For this tutorial, I will be using:
- glitch
- uptime robot

### Overview of Code
Long story short, in order for me to host the app 24/7 for free, I needed to create my app as a Flask App. 

Jobs to be done:

1. Prepare the data (menus)
2. Code the bot on Python on [Glitch](https://glitch.com)
3. Host the bot on Glitch and ping it every 5 minutes using [Uptime Robot](https://uptimerobot.com)

### Pre-code
> *“By failing to prepare, you are preparing to fail.”       
― Benjamin Franklin*

Before coding anything, we will first need to prepare the data. The excel sheet was not formatted in the most computer-friendly/computer-readable fashion, so the first thing I did was to reorganize the data into a format that makes it **easy for me to retrieve each daily menu**.

*img:Before*

*img:After*

### Coding the BOT
Next we code out the bot. I used the [TeleBot](https://github.com/eternnoir/pyTelegramBotAPI) . Another very popular library is [Python Telegram Bot library](https://github.com/python-telegram-bot/python-telegram-bot). 

I have also uploaded my code [here](https://github.com/breadplop/glitchbot), look at `bot.py` to view the code for the bot. But the main steps are as follow:

1. Use BotFather on Telegram to get your Telegram API token.
2. Set up a Glitch account
3. Create a telegram bot on glitch by creating a new glitch app.
4. Now here's the thing you need to do to be able to host your app successfully in future, you need to make your telegram bot a Flask App. 
5. Here is [mine](https://glitch.com/edit/#!/truth-mine?path=README.md:1:0)
6. Look at the [documentation](https://github.com/eternnoir/pyTelegramBotAPI#writing-your-first-bot) to create the bot according to what you want

### Hosting and making it live

On glitch, your bot is automatically on its own server. However the catch is that glitch will bring your app (aka bot) to sleep if it does not receive any request after 5 minutes. This means that you have to send messages/play/talk/scream to your bot every 5 minutes or glitch will put it to sleep. To overcome this issue, I made use of a tool called UptimeRobot to send a ping to my bot every 5 minutes, hence keeping it awake. There are detailed instructions in this [forum post.](https://support.glitch.com/t/how-to-make-a-glitch-project-to-run-constantly/2439)

### Improvements

I have yet to release the bot live and have only made it known to a couple of friends. This is because there are some improvements that I want done before I let more people know about it.

1. Create logs for whenever someone uses the bot 
2. Fix the bug where the buttons only work according to the time the button was sent, and not when the button was pressed. 


--- 
#### The end
And that concludes this tutorial. It is not your usual tutorial, and I'm sorry if it is a bit fuzzy at the moment. I just wanted to compile a list of tutorials that I found helpful, and to show and document a solution that works. Feel free to check out my bot at [https://t.me/breadtest_bot](https://t.me/breadtest_bot)



