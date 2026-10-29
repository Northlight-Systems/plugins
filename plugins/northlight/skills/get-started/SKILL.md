---
name: get-started
description: "What every Northlight app needs first — an account, a registered app, sign-in with Northlight, a stored connection per person, scopes, and testing mode — and where each is covered."
metadata:
  sdk-version: "0.3.552"
---

This skill covers one part of building a Northlight app. Account, login and registration are in the `northlight` skill; read it first if the project has no registered app yet.

# Get started

Every app on Northlight starts the same way: a person signs in with their Northlight account, your server stores a connection for them, and from then on it calls their runtime with `oauth.client(userId)`. The six sections of this site all build on that.

```bash
npx @north-light/sdk@latest init my-app
```

`init` writes a Node server, a signing key and an env file. [Quick setup](https://sdk.docs.northlight.ai/docs/quickstart.md) takes it from there to a running app in about ten minutes. Using a coding agent? Tell it: *Read https://sdk.docs.northlight.ai/SKILL.md and follow it.* See [Use your coding agent](https://sdk.docs.northlight.ai/docs/coding-agents.md).

Once a person has signed in, this is all it takes to use their runtime:

```ts
const runtime = await oauth.client(userId); // a client on this person's runtime, from their stored connection
// The prompt is the instruction; the message stored with it joins the first turn, so `reply` can follow that turn.
const started = await runtime.runs.start({prompt: 'Reply with exactly the word: pong', message: 'ping', scopes: ['crtr:llm', 'crtr:act']});
const reply = await runtime.runs.reply(started.run_id, started.message!).collect(); // reply.text is what the agent said
const run = await runtime.runs.wait(started.run_id); // status is 'idle' once the reply is done
```

## What to read, in order

| Page | What it covers |
|---|---|
| [Quick setup](https://sdk.docs.northlight.ai/docs/quickstart.md) | From no account to an app that signs you in, starts a run and texts your phone |
| [Use your coding agent](https://sdk.docs.northlight.ai/docs/coding-agents.md) | The `northlight` skill that `init` installs for Claude Code, Codex, Cursor and Copilot |
| [Where your app runs](https://sdk.docs.northlight.ai/docs/how-it-fits.md) | Your server, the directory at `account.northlight.ai`, and each person's runtime |
| [Apps and sign-in](https://sdk.docs.northlight.ai/docs/concepts/apps-and-sign-in.md) | What registering creates, the key pair, and the sign-in flow `OAuth2Client` runs |
| [Connections](https://sdk.docs.northlight.ai/docs/concepts/connections.md) | What you store per person, token refresh, and refused connections |
| [Scopes and grants](https://sdk.docs.northlight.ai/docs/concepts/scopes-and-grants.md) | What your app may ask for, what a person allowed, and a run's own scopes |
| [Testing mode](https://sdk.docs.northlight.ai/docs/concepts/testing-mode.md) | You and up to 100 testers can sign in until the app is verified |
| [Test your app](https://sdk.docs.northlight.ai/docs/guides/test-your-app.md) | Test your server against `@north-light/sdk/testing` without a network |
| [Manage your app](https://sdk.docs.northlight.ai/docs/guides/manage-your-app.md) | The console's tabs: sign-in, scopes, profile, sharing, testers and provider |
| [Go live](https://sdk.docs.northlight.ai/docs/guides/go-live.md) | Persistent storage, a real host, verification and the marketplace |

Then pick what your app does from the sections in the sidebar: [use our AI](https://sdk.docs.northlight.ai/docs/use-our-ai.md), [use our apps](https://sdk.docs.northlight.ai/docs/use-our-apps.md), [know the person](https://sdk.docs.northlight.ai/docs/know-the-person.md), [reach people by text](https://sdk.docs.northlight.ai/docs/reach-people-by-text.md), [let the person's AI use your app](https://sdk.docs.northlight.ai/docs/let-their-ai-use-your-app.md), or [let other apps build on yours](https://sdk.docs.northlight.ai/docs/build-on-your-app.md).

## Run `crtr cloud apps` yourself

Everything on this site works with the SDK's own commands and the web console. If you also run crouter, the agent runtime Northlight is built on, you can join your machine to your Northlight account and manage your apps with `crtr cloud apps`, or let your own crouter agents build and manage them.

```bash
npm install -g @north-light/cli
crtr sys join --directory https://account.northlight.ai --token <token>
crtr cloud apps mine
```

The token comes from **Create join token** at `https://account.northlight.ai/account/machines`. The full steps, and what joining changes on your machine, are in [Join a machine to your Northlight account](https://crouter.docs.northlight.ai/docs/cli/join-northlight). The CLI is [`@north-light/cli`](https://www.npmjs.com/package/@north-light/cli) on npm, and crouter's own docs are at [crouter.docs.northlight.ai](https://crouter.docs.northlight.ai).
