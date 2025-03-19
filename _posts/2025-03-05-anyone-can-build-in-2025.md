---
layout: post
title:  "Anyone Can Build in 2025"
date:   2025-03-05 16:20:00
excerpt: "*I was previously a senior software engineer at Google X (Loon), and now I'm a YC founder at [Stella](http://chatwithstella.com) who builds software faster than ever, thanks to AI.*


**100% of my code is written with AI. I accomplish 10x more, way faster.** AI prototypes entire products for me, fixes bugs while I'm meeting live with customers, and heck, I even used it to build a [multiplayer fish simulation](http://swim.endacott.me) in *one single instruction*\\! At this point, I don't think I could go back to the old way of building.


**My goal with this post is simple:**


**To inspire you and show that anyone can build with AI - no matter your background.**"
---

*I was previously a senior software engineer at Google X (Loon), and now I'm a YC founder at [Stella](http://chatwithstella.com) who builds software faster than ever, thanks to AI.*

**100% of my code is written with AI. I accomplish 10x more, way faster.** AI prototypes entire products for me, fixes bugs while I'm meeting live with customers, and heck, I even used it to build a [multiplayer fish simulation](http://swim.endacott.me) in *one single instruction*\! At this point, I don't think I could go back to the old way of building.

**My goal with this post is simple:**

**To inspire you and show that anyone can build with AI - no matter your background.**

<!--more-->

It's a crazy, exciting new world. Here are some tips I've learned:

- **The learning curve is real at any experience level.** Whether you're brand new to coding or you're a seasoned developer, **you will definitely be less productive at first.** Many coders run into a wall and assume AI can't help with their tasks. Don't stop there - stick with it! You'll run into many painful and frustrating situations where the AI doesn't *quite* get what you mean, or introduces subtle bugs. Refine your instructions ("prompts"), refine the info you give the AI ("context"), and try again. Try a different model - they all have varying strengths and weaknesses. My current thoughts below [[0]](#0).

- **Context is key. You have access to a literal genius in *any* domain** - you just have to give it the right data to base its decisions (and code) on. Tools like ***Cursor*** and ***RepoPrompt*** shine here [[1]](#1). They make it easy to pass *exactly* the right files and data to the AI. Just enough so it understands everything it needs in order to build your request. But not too much, so it doesn't get overwhelmed.

![Using Cursor to pass context](/images/cursor-context-example.png)
*Using Cursor to pass specific files and context to the AI. You can see the banner it created [here](http://chatwithstella.com).*

- **Prompting is a critical new skill.** AI is a genius, but it's a genius intern. You need to be clear about what you want. Refine your ask for the AI. Be detailed. Choose a role. Tell it to "Act as an expert software engineer that prioritizes maintainable, simple, clear code." Even better - ask an AI model for a prompt to use. Ask what's clear and unclear about your prompt, and where you can improve. **If AI can't accomplish a task, break it down into smaller steps.** Instead of asking AI to create a fully-working "like button" all at once, first have it set up your **database** (to store which users liked each post), then build the **backend** (the code that saves likes and keeps count), and finally design the **frontend** (the clickable heart ❤️ button users actually tap).

- **Prompts compound over time.** Store your prompts in your repo, so you can refer to them whenever you need. For example, to add logs to a file, I just run `@addlogs.md` in Cursor, which contains instructions like "Act as an expert software engineer and add logs to this code." **Prompts aren't limited to tasks: Document your design guidelines and product philosophy for AI** to use with every new line of code.

- **You have to manage technical debt.** It's easy for the AI to go overboard when coding, ending up with an unmaintainable mess. With good prompting and tools like Cursor, you can avoid this altogether. The technical eye of an engineer helps a lot here, but you can also go surprisingly far by asking the AI to review and improve its own designs. Ask it to simplify or consolidate files while keeping the same features intact or ask it what approaches could simplify the architecture and their trade-offs.

- **Experimenting pays big dividends!** I just discovered a new workflow that nails ***much*** more complex code changes in one prompt [[2]](#2). You can use it to prototype new products or build entire features in less than 5 minutes. I only realized it was possible when I tried a task with **four different AI models** before I found this workflow. Keep experimenting to find low-hanging fruit like this.

**I've led workshops on "vibe coding" with AI for engineers, founders, and non-technical folks alike. If this sounds interesting to you or your community, reach out on [X](https://x.com/ryanendacott) and let me know! I'd love to chat.**

Hope this was useful! I got carried away and wrote more than I intended, lol. Can you tell I like coding with AI? 😂

Seriously, anyone can do this stuff now. It's amazing.

![Fish simulator screenshot](/images/swim.png)
*A multiplayer fish simulation I built in just a couple hours using AI. Aren't Claude's fish adorable? [Play live here!](http://swim.endacott.me)*

**Appendix**

<a name="0"></a>[0]: **Current Model Strengths**, in my opinion:

- **Claude 3.7 Sonnet** is great for UI/UX changes, especially if you give it design guidelines and a screenshot. It's good at backend changes confined to one file. Be *extra careful* about technical debt with this model - it gets excited and does way more than you ask. I often tell it to "Please limit your changes only to <task>."
- **GPT-4.5** is great for strategy, product ideas, and copy. I haven't heard that it's particularly great for code, but I haven't experimented much.
- **Grok 3 Think Mode** is a powerhouse for full stack changes. I've been surprised at how well it can "one-shot" handle complex changes in a single prompt, even with my entire repository in context (via RepoPrompt).
- **OpenAI O1-Pro** is another powerhouse for full stack changes. It's about on par with Grok 3 Think Mode. It nails some tasks Grok 3 struggles with, and vice versa.

<a name="1"></a>[1]: **Current Tool Strengths**, in my opinion:

- **Cursor** is a code editor that lets you manage what context you pass to the AI, then applies changes directly to your codebase. Its paid mode has an "agent" that will autonomously read your codebase and make changes itself. It's pretty strong for medium complexity changes, and great for UI changes.
- **RepoPrompt** is an app to copy your entire repository or specific files for pasting into AI chats. It's great for pasting your code into powerful reasoning models like O1-Pro and Grok 3 Think Mode.
- **Claude Code** is an AI wrapper for your terminal. It's great for applying large changes from Grok 3 or O1-Pro to your codebase, but a bit pricey. I've heard it's great for making real code changes on its own, but haven't tried myself.

<a name="2"></a>[2]: [https://x.com/RyanEndacott/status/1896687550112403941](https://x.com/RyanEndacott/status/1896687550112403941)

**Huge thank you** to Arti Villa and Jaireh Tecarro for reading drafts of this!