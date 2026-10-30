---
name: reach-people-by-text
description: "Give your app its own iMessage number with Comms. People text it, your server answers with an agent run on their runtime, and you can text them again later."
metadata:
  sdk-version: "0.3.557"
---

This skill covers one part of building a Northlight app. Account, login and registration are in the `northlight` skill; read it first if the project has no registered app yet.

# Reach people by text

Comms gives your app its own iMessage number, under your app's name and avatar. People text that number, Comms sends each text to your server as a signed webhook, and your server answers with an agent run on the person's own runtime. Once a person has texted you, your app can text them again later, for example from a timer.

```ts
import {continueInRuntime, formatReply, verifyWebhook} from '@north-light/comms';

const event = verifyWebhook(req.headers, rawBody, [process.env.COMMS_WEBHOOK_SECRET!]);
if (event.type === 'message.received' && event.data.content.type === 'text') {
  const {contact, content} = event.data;
  if (!contact.user) return; // only a sender texting from an Apple ID email has no user
  const client = await oauth.client(contact.user.id); // their Northlight user id, the same as Connection.userId
  const {runId, reply} = await continueInRuntime({client, message: content.text, runId: runs.get(contact.id), idempotencyKey: event.id});
  runs.set(contact.id, runId); // later texts continue the same run
  const {text} = await reply.collect();
  for (const part of formatReply(text)) {
    await comms.sendMessage({environment: event.environment, to: {contact_id: contact.id}, text: part, idempotency_key: `reply:${event.id}:${part.length}`});
  }
}
```

`oauth.client` throws `CrouterError` with code `not_connected` when the person has not signed in to your app yet. Answer that with `sendSignInLink(comms, contact, {environment: event.environment})`: Comms texts them a link that signs them in to your app, which lands on your **Sign-in start URL** (console → **Profile** → **Marketplace**).

## The steps

The [Comms guide for coding agents](https://comms.northlight.ai/llms.txt) is the whole reference: the webhook events, the send API, its errors, STOP and HELP handling, and the `crtr comms` commands. In short:

1. **Build in fake mode.** `npx @north-light/comms fake --webhook http://localhost:3000/hook` runs a local Comms with no account. `npx @north-light/comms fake send --from +15550001 --text "hi"` plays a person texting you.
2. **Register your app on Northlight.** [Quick setup](https://sdk.docs.northlight.ai/docs/quickstart.md) steps 1 to 4: sign up, `npx @north-light/sdk login`, `npx @north-light/sdk register`. Comms signs in with the same app key.
3. **Connect the app to Comms.** At `https://comms.northlight.ai`, sign in and choose **Connect an app**. This is done in a browser only.
4. **Turn Texting on.** On the app's page there, or with `crtr comms turn-on`: a display name, a square avatar, the HELP text and your test webhook URL. The page shows the test webhook signing secret.
5. **Text it from your own phone, in test mode.** Add yourself as a tester (`crtr comms testers add --app-id <id> --phone <number> --label <name>`, or the site), then text `JOIN <code>` to the test number. Test mode is free; the [Comms guide](https://comms.northlight.ai/llms.txt) has the tester limit.
6. **Go to production** when you want your app's own number and anyone to text it. Production is paid: add a card on the billing account, and Comms gets your app its own number.

Comms is priced per person your app texts with in production each month; fake mode and testing are free. The SDK is [`@north-light/comms`](https://www.npmjs.com/package/@north-light/comms) on npm. Install it with `npm i @north-light/comms`.

## Rules

- **People text first.** Your app can text a person only after they texted your number in that environment, and until they send STOP. A send to anyone else fails `403 not_texted_first`.
- **Address people by `contact_id` or `user_id`**, never by phone number.
- **Derive `idempotency_key` from the event you are answering**, so a retried webhook sends nothing twice. Deduplicate deliveries on `event.id`.
- **Verify the raw body** with `verifyWebhook` before you parse it, and answer `2xx` within 15 seconds.
