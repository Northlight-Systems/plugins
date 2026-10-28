---
description: "Build a new app people text, from an idea, on Northlight."
argument-hint: "[your idea]"
---

Build an app people text, using the `northlight` skill.

The idea: $ARGUMENTS

If the idea above is empty, ask what the app should do before you start.

1. Read the `northlight` skill, section "If the person gave you an idea: build it as an app people text", and follow it.
2. Scaffold with `npx @north-light/sdk@latest init <dir>`. Build and test in Comms fake mode first; no account is needed yet.
3. Have the person sign up, then run `npx @north-light/sdk login` and `npx @north-light/sdk register` as the skill describes. Show them the login link and code exactly as printed.
4. Texting is Comms only. Follow https://comms.northlight.ai/llms.txt and the `reach-people-by-text` skill for the webhook handler.
5. Answer each text with an agent run on the texter's own runtime (the `use-our-ai` skill).
6. Check each step worked before the next, and tell the person exactly what they must do in a browser.
