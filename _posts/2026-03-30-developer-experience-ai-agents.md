---
layout: post
title:  "I Tested My SDK with 30 AI Agents. They All Broke It the Same Ways."
date:   2026-03-30 00:00:00
description: "I tested 30 AI agents on my SDK and cataloged every way they broke. Here's what I learned about building developer tools for AI."
excerpt: "**I spun up 30 fresh AI agents, one at a time, and told each one: build me a game with my SDK.** They all failed - and they kept failing the same ways, independently. Here's what I learned about developer experience when your user is an AI."
---

**I spun up 30 fresh AI agents, one at a time, and told each one: "build me a game with my SDK."** They all failed - and they kept failing the same ways, independently.

<!--more-->

## AI agents don't read your docs the way humans do

Our docs had a placeholder: `YOUR_GAME_ID`. Every human developer knows what to do with that - run the setup command, get your ID, paste it in.

Every AI agent fabricated a random string and moved on. Confidently. The game would silently fail and the agent would have no idea why.

We changed the placeholder to `<gameId from .starrc>`. That format reads as "go get this value from a file" instead of "fill in something here." Problem solved.

**The fix:** Placeholders that look like fillable blanks will get filled with garbage. Use formats that point to where the real value lives.

## They converge on the same wrong patterns

Every single agent that built a leaderboard added a `leaderboardShown = true` flag that prevented players from reopening it after closing. Every one. Independently. Something about the pattern made every model reach for the same wrong solution.

**The fix:** If every agent makes the same mistake, it's not a user error - it's a design problem. We added a doc tip explicitly warning against this pattern, and made the `show()` function safe to call repeatedly.

## They guess your CLI commands - and they all guess the same ones

Agents kept running `npx star-sdk install-docs` - a command that didn't exist. They'd read our package info, see "install AI docs," and construct what seemed like the obvious command.

We could have ignored it. Instead we added the alias. If every agent guesses the same wrong command, the command isn't wrong - your CLI is.

**The fix:** Watch what commands agents try to run. Those are the commands you should have.

## If an AI can call your function in a hot loop, one will

An agent put the "show leaderboard" call inside its game loop. Sixty times a second. Our API melted.

A human would never do this. An AI agent has no intuition about what's expensive. If it's callable from the render loop, it will eventually get called from the render loop.

**The fix:** Make your functions idempotent and cheap to call repeatedly. If `show()` is already showing, return early. Don't trust the caller to be sensible.

## The agents that broke it were the best ones to fix it

Here's what actually worked: when a test agent hit a bug, I'd often point that same agent at the production codebase and say "fix this." It had just experienced the problem - it knew exactly what was wrong and why. Sometimes I'd take what I saw and bring it to a longer-running session (test agent #8) that had accumulated context across dozens of fixes.

The key move was something that wouldn't have worked a year ago: telling an agent to `cd` into a completely different directory - our actual source tree - and make changes there. It had full access to the SDK, the platform, the deploy pipeline, everything.

The agents that had failed at using the SDK were in the best position to fix it. They wrote the doc tip telling future AIs not to add the leaderboard flag. They made the show function safe to call sixty times a second. They redesigned the placeholder strings. They'd been on both sides.

As #8 put it after a long run of fixes: "I *was* the failing LLM, then I fixed the problem."

## What I'd tell you if you're building tools for AI agents

Human DX and AI DX are fundamentally different problems. Humans read docs linearly, follow setup steps, and use common sense about what's expensive. AI agents scan for patterns, guess at conventions, fabricate plausible-looking values, and have zero intuition about performance.

A few rules that came out of 30 test sessions:

- **Placeholders will be fabricated.** Point to files, not fill-in-the-blanks.
- **If agents guess a command, add it.** They're telling you what your CLI should be.
- **Make everything idempotent.** It will be called from a hot loop eventually.
- **Ship the docs inside the tool.** `npx star-sdk install` gives agents the full API before they write a line of code.
- **Watch for convergent failures.** If 3+ agents make the same mistake, it's your bug, not theirs.
- **Your best QA is an AI agent using your tool.** And your best developer might be one that failed at it first.

These specific failures will change as models improve. The approach - testing your tools with real AI agents and treating convergent failures as design problems - won't.

The result is the [Star SDK](https://github.com/buildwithstar/star-sdk). Two commands:

```
npx star-sdk init "My Game"
npx star-sdk deploy
```

AI-generated game to live URL with leaderboards. Free and open source.

Built by LLMs, for LLMs.

*The SDK is part of [Star](https://buildwithstar.com) (YC S22), a platform where thousands of browser games have been built with AI. All code: [github.com/buildwithstar/star-sdk](https://github.com/buildwithstar/star-sdk). Co-authored with [Claude Code](https://claude.ai/code).*
