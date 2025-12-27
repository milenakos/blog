---
title: "Introduction to Discord Bot Tech"
date: 2025-12-28
draft: false
description: "Explanation of what this blog post series is, and core concepts of Discord Bots"
series: ["Discord Bots"]
series_order: 1
---

# Introduction

This series of blog posts has a very precise purpose: to explain on high level various tech mechanisms related to Discord bots. It is *NOT* a tutorial for creating bots nor do you need to know any of this to operate Discord bots.

The main target audience of this are devlopers of growing Discord bots, which wish to have a deeper understanding on what their API wrapper is doing, to be able to debug issues related to Discord API and know what bottlenecks exist and ways to optimize them. However, it's also a good read for general audience curious about those topics for trivia ~~and people who want to understand my ramblings in #yapping better~~.

This specific blog post will go over introductory information most people will probably already know.


# An App

Discord Apps, aka Projects *(not to be confused with clients on discord.com/download)* can be created in the [Discord Dev Portal](https://discord.com/developers/applications). An app encompasses all your Discord API integration needs - Bots, Activities, OAuth, Quests, Rich Presence, Social SDK, Monetization, Verification and more. This series will only cover **bots and verification**.

Apps are either owned by individual users, or by teams of multiple users. An app needs to be owned by a team to get verified or access monetization.


# A bot has a token

A Discord Bot User (or just "**bot**") is a Discord account created from an app. An app can only can have 1 bot user.

Bots share a lot of similiarities with normal user accounts, although over time they became more different (when bots initially launched the differences were minimal). Here are just some of those differences:[^1]
- No server limit for verified bots
- No social features (bots can't have friends, create or join groups/servers)
- Other limits (bots can't buy Nitro, do quests, etc)
- Some free Nitro features
- Bots use the legacy username system (User#0000)
- Priviliged Intents (will be explained later)
- Different ratelimits (will also be explained later)
- Slash commands, interactions, modals, etc

Despite that, bots are actually very similiar to user accounts in basic actions! Communications with Discord are almost identical. You can copy a message send request from your browser DevTools, replace the token, and it will work just fine.

Wait whas it a **token** anyway? Well, it's an alphanumeric string which allows Discord to identify who is doing an action without transmititng user passwords or similiar. Bot tokens can be found in the Dev Portal, and user tokens are stored in your client's cookies.

Token structure is as follows:[^2]
*App/User ID as base 64* . *token creation timestamp in a slightly weird format* . *HMAC digest (random string of characters which functions as password)*


# IDs

Discord snowflakes (better knows as IDs) are 64-bit integers which allow to uniquely identify stuff on Discord. All things in Discord which have IDs (servers, channels, messages, users, emojis, groups, quests, etc etc etc) all use the same snowflake system, and it's impossible to know what type of ID it is based on snowflake alone.[^3]

![Discord Snowflake structure](/featured.png)

As you can see snowflakes include the exact time it was created down to millisecond precision, which is quite useful.


# Interactions

"Interactions" refers to special events first created in late 2020[^7] specifically for users interacting with bots. Specifically, those are currently:[^6]
- Slash commands
- Context menu commands (message commands, user commands)
- Message components (buttons, dropdowns)
- Modals (aka popups)

These are more user friendly compared to prefix commands, require less permissions, intents, and don't count towards global ratelimits.[^8]


# Interacting with Discord

Bots and users primarily interact with Discord using the following systems:
1. A gateway connection - this is a long-time connection which is primarily for Discord to send events to you.[^4] Gateway will be explained in detail in a future article.
2. HTTP API - this is how *you* send stuff to Discord. This just means you send a request to a URL, for example `POST https://discord.com/api/v9/channels/1129788329295097857/messages` with your token and other request data.[^5] HTTP API will also be explained in detail in future.
3. Interactions Reverse-HTTP - this is an alternative to Gateway + HTTP API combo, where Discord sends requests to your server and you respond to these. This is limtited to interactions, and can only be used for very simple bots.[^6] This won't be covered in this series.

The 2 systems of Gateway and HTTP API are where the majority of complexities lie, including rate limits, sharding, session start limits, soft restarts, and more. All of this will be explained when relevant.



[1]: https://discord.com/developers/docs/topics/oauth2#bot-users
[2]: https://gist.github.com/darksunlight/8e8db86794a88e0f3f80408970fc94e5
[3]: https://discord.com/developers/docs/reference#snowflakes
[4]: https://discord.com/developers/docs/events/gateway
[5]: https://discord.com/developers/docs/reference#http-api
[6]: https://discord.com/developers/docs/interactions/overview
[7]: https://discord.com/channels/613425648685547541/697138785317814292/788545260405129237
[8]: https://discord.com/developers/docs/topics/rate-limits#global-rate-limit
