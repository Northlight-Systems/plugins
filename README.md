# Northlight plugins for Claude Code

The Northlight plugin for [Claude Code](https://code.claude.com): skills that teach Claude how to build apps on Northlight with [`@north-light/sdk`](https://www.npmjs.com/package/@north-light/sdk), and commands for the common jobs. Docs: https://sdk.docs.northlight.ai.

This repository is generated from the [crouter](https://github.com/Northlight-Systems/crouter) repository's docs and skill each time a release goes out. Do not edit it by hand: changes are overwritten. To change something, change the docs in crouter. The current content is for SDK version 0.3.551.

## Install

In Claude Code:

```
/plugin marketplace add Northlight-Systems/plugins
/plugin install northlight
```

## What it adds

Commands:

- `/northlight:new-app [your idea]`: Build a new app people text, from an idea, on Northlight.
- `/northlight:add-permission [scope, or what the app should be able to do]`: Add a permission (a scope) to the app so it can call a Northlight provider.
- `/northlight:text-people [what the texts should do]`: Give the app its own iMessage number with Comms, so people text it and it answers.
- `/northlight:become-a-provider [what the tools do]`: Offer the app's tools to the person's own AI, as a Northlight provider.
- `/northlight:go-live`: Move the app from the laptop to a real host and ask Northlight to verify it.

Skills:

- `northlight`: the front door. Account, login, registering the app, sign-in, runs, providers and the rules for each.
- `get-started`: https://sdk.docs.northlight.ai/docs/get-started
- `use-our-ai`: https://sdk.docs.northlight.ai/docs/use-our-ai
- `use-our-apps`: https://sdk.docs.northlight.ai/docs/use-our-apps
- `know-the-person`: https://sdk.docs.northlight.ai/docs/know-the-person
- `reach-people-by-text`: https://sdk.docs.northlight.ai/docs/reach-people-by-text
- `let-their-ai-use-your-app`: https://sdk.docs.northlight.ai/docs/let-their-ai-use-your-app
- `build-on-your-app`: https://sdk.docs.northlight.ai/docs/build-on-your-app

Without the plugin, tell your agent: Read https://sdk.docs.northlight.ai/SKILL.md and build my idea as an app people text.
