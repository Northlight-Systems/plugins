---
description: "Give the app its own iMessage number with Comms, so people text it and it answers."
argument-hint: "[what the texts should do]"
---

Let the app text people, using Comms and the `reach-people-by-text` skill. Comms is the only way an app texts people; the `sms` provider texts only the signed-in person's own phone.

Asked for: $ARGUMENTS

1. Read https://comms.northlight.ai/llms.txt and follow it wherever it differs from the skill.
2. Build in fake mode: `npx @north-light/comms fake --webhook http://localhost:3000/hook`, and play a person with `npx @north-light/comms fake send --from +15550001 --text "hi"`.
3. Register the app on Northlight (the `northlight` skill), then ask the person to connect it to Comms at https://comms.northlight.ai. That step is done in a browser only.
4. Turn Texting on for the app (`crtr comms turn-on`, or on the site): display name, square avatar, HELP text, test webhook URL.
5. In the webhook handler, verify the raw body with `verifyWebhook`, answer with `continueInRuntime`, and on `not_connected` send `sendSignInLink`.
6. Test from the person's phone in test mode: add them as a tester and have them text `JOIN <code>` to the test number.
7. Production is paid. Tell the person so; do not promise a free production number. Never text a person first.
