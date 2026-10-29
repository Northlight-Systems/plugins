---
name: use-our-apps
description: "Call Northlight's providers from your app on the person's runtime — texting their phone, web search, their connected accounts, their Mac — with providers.tools and providers.call."
metadata:
  sdk-version: "0.3.553"
---

This skill covers one part of building a Northlight app. Account, login and registration are in the `northlight` skill; read it first if the project has no registered app yet.

# Let your app use our apps

Northlight's own apps are providers your app can call on the person's runtime: text their phone, place calls, search the web, open a cloud browser, read and act in the accounts they connected, and use their Mac. The person approves each provider's scope once, on your app's consent page.

```ts
import type {ConnectedCrouter} from '@north-light/sdk';

/** List the tools a provider offers this person, then call one. Scope strings come from the console's
 *  "Add permissions" list; tool names and arguments come from `providers.tools`. Never invent either. */
export async function textMe(runtime: ConnectedCrouter, body: string) {
  const sms = await runtime.providers.tools('sms');
  console.log(JSON.stringify(sms.groups, null, 2)); // each leaf: tool, description, params

  return runtime.providers.call({provider: 'sms', tool: 'send', arguments: {body}});
}
```

Every provider works the same way: ask for its scope, list its tools with `providers.tools('<provider>')`, then call one with `providers.call`. Scope strings, tool names and arguments come from those lists, never from memory.

## Providers

| Provider id | What it does | Page |
|---|---|---|
| `sms` | Texts the signed-in person's own verified phone | [Texting (sms)](https://sdk.docs.northlight.ai/docs/providers/sms.md) |
| `search` | Web search, sourced answers, page contents and paid data endpoints | [Search](https://sdk.docs.northlight.ai/docs/providers/search.md) |
| `integrations` | The person's own accounts at other services | [Integrations](https://sdk.docs.northlight.ai/docs/providers/integrations.md) |
| `context` | Ask about the person from their memory, and look up public profiles | [Context](https://sdk.docs.northlight.ai/docs/providers/context.md) |
| `device` | The person's Mac: browser, files, Messages and shell | [Device](https://sdk.docs.northlight.ai/docs/providers/device.md) |

These five have pages here. Northlight also runs `phone` (calls from Northlight's number, with an AI voice), `browser` (cloud browser sessions) and `meetings` (the person's meeting notes and transcripts), which have no page yet. For any of them, `providers.tools()` with no argument lists every provider installed on the person's runtime, `providers.tools('<provider>')` lists its tools and arguments, and the console's **Add permissions** list shows every scope your app can ask for.

## Guides

| Page | What it covers |
|---|---|
| [Providers](https://sdk.docs.northlight.ai/docs/concepts/providers.md) | Scopes, finding and calling tools, and providers your app requires |
| [Providers index](https://sdk.docs.northlight.ai/docs/providers.md) | Every first-party provider, its scopes and where names come from |
| [Grant changes](https://sdk.docs.northlight.ai/docs/guides/grant-changes.md) | When a person adds or removes a scope, and waiting on a provider's setup |
