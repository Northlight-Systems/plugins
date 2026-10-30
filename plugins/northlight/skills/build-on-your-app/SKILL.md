---
name: build-on-your-app
description: "Offer your app as a service other Northlight apps build on — as a provider their runs call, or as your own API — and list it with a docs link so their coding agents find it."
metadata:
  sdk-version: "0.3.557"
---

This skill covers one part of building a Northlight app. Account, login and registration are in the `northlight` skill; read it first if the project has no registered app yet.

# Let other apps build on yours

Other developers' apps can build on yours the way they build on Northlight's: by calling your provider tools from a run, or by calling your own API. Set a docs link on your app and it is listed on [Services you can build on](https://sdk.docs.northlight.ai/docs/services.md), where their coding agents look.

Comms is built this way. Another app calls it with its own Northlight app key, and reads [comms.northlight.ai/llms.txt](https://comms.northlight.ai/llms.txt) to learn how:

```ts
import {readFile} from 'node:fs/promises';
import {CommsClient} from '@north-light/comms';

const comms = new CommsClient({
  baseUrl: 'https://comms.northlight.ai',
  issuer: 'https://account.northlight.ai',
  clientId: process.env.CROUTER_CLIENT_ID!, // the calling app's own client id and key
  privateKeyJwk: JSON.parse(await readFile(process.env.CROUTER_KEY_FILE!, 'utf8')),
});
```

## Two ways to offer it

- **As a provider.** Your tools run on the person's runtime for any app the person has granted them, and you can charge per call. This is the same work as [letting the person's AI use your app](https://sdk.docs.northlight.ai/docs/let-their-ai-use-your-app.md); other apps ask for your groups as scopes named `<your client id>:<group>`.
- **As your own API.** Serve HTTP from your own host, as Comms does. A calling app identifies itself with its Northlight client id and key, so you know which app is asking.

## Get listed

1. In the console, open your app's **Profile** tab.
2. Under **Marketplace**, set **Docs link** to the https URL of your guide for agents and developers, for example `https://yourapp.com/llms.txt`, and choose **Save docs link**.
3. Write that guide for a coding agent: what your service does, how to install the client, one working snippet, the errors and their fixes. [comms.northlight.ai/llms.txt](https://comms.northlight.ai/llms.txt) is a working example.

[Services you can build on](https://sdk.docs.northlight.ai/docs/services.md) is rebuilt from the directory's public marketplace list each time this site is built, so a new docs link appears with no change to these docs.

## Guides

| Page | What it covers |
|---|---|
| [Let the person's AI use your app](https://sdk.docs.northlight.ai/docs/let-their-ai-use-your-app.md) | Define tools with `@north-light/plugin`, serve them, add the provider role, set a price |
| [Services you can build on](https://sdk.docs.northlight.ai/docs/services.md) | Every listed app with a docs link |
| [Manage your app](https://sdk.docs.northlight.ai/docs/guides/manage-your-app.md) | The console's **Profile** and **Provider** tabs |
| [Go live](https://sdk.docs.northlight.ai/docs/guides/go-live.md) | Verification and the marketplace listing |
