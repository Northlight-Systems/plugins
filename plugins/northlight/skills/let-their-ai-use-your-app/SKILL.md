---
name: let-their-ai-use-your-app
description: "Make your app a provider so its tools show up in the person's chat and terminal agents. Define commands with @north-light/plugin, serve them over HTTP, and add the provider role in the console."
metadata:
  sdk-version: "0.3.558"
---

This skill covers one part of building a Northlight app. Account, login and registration are in the `northlight` skill; read it first if the project has no registered app yet.

# Let the person's AI use your app

Your app can be a provider: it offers tools, and once a person connects it, every agent they run (in Northlight chat, in their terminal, and in other apps that ask for it) can call them. You define the tools once with [`@north-light/plugin`](https://www.npmjs.com/package/@north-light/plugin) and serve them from your own server.

```ts
import {createFetchHandler, defineLeaf, definePlugin, field, param} from '@north-light/plugin';

export const plugin = definePlugin({
  name: 'acme',
  provider: true,
  version: 1,
  description: 'Manage Acme orders.',
  whenToUse: 'you need to look up an Acme order.',
  summary: 'Acme orders',
  rootEntry: {concept: 'an Acme order', description: 'Look up orders.', whenToUse: 'the task is about an Acme order.'},
  groups: {read: {consentLine: 'Read your Acme orders.'}},
  commands: {
    order: defineLeaf({
      group: 'read',
      description: 'Show one order.',
      whenToUse: 'you have an order id.',
      summary: 'show an order',
      params: {id: param.positional('The order id.', {required: true})},
      output: {status: field.string('The order status.')},
      effects: ['Reads one order.'],
      handler: (input, ctx) => ({status: 'shipped'}), // ctx.caller.sub is the person who connected
    }),
  },
});

export default createFetchHandler(plugin, {
  auth: {issuer: 'https://account.northlight.ai', audience: 'https://acme.example.com/crtr', providerId: process.env.CROUTER_CLIENT_ID!},
});
```

Each group in `groups` is a scope named `<your client id>:<group>`, and its `consentLine` is what the consent page shows. Every request carries a token the directory minted for one person; the handler checks it and the group's scope before your code runs.

## The steps

1. **Define the tools** with `definePlugin({provider: true, version, groups, commands})`. Every leaf names its `group`.
2. **Serve them** at an HTTPS URL. `createFetchHandler` is a standard Fetch handler; `createNodeHandler` serves the same plugin from Node's `http` server.
3. **Add the provider role.** In the console, open your app's **Provider** tab, fill in **Offer tools to other apps** with your **Endpoint URL** (and an optional **Manage URL**, where people manage their connection), and choose **Add provider role**. The directory fetches your manifest and saves the groups it declares. After a change, choose **Refresh manifest**.
4. **Set a price**, if you charge, on the same tab: free, a price per unit, or the cost your provider reports, plus a free monthly allowance per person.

## Who may use a group

A group's `who` says who may be granted it: `own` (only your own app), `user` (the default), `allowlist` (the apps listed in `allowlist`), `any`, or `team:<id>`. The directory applies `own` and `team:<id>`; `user`, `allowlist` and `any` are stored but not yet enforced at consent.

## Guides

These live on the crouter docs site, because the same package also adds commands to a self-run crouter:

| Page | What it covers |
|---|---|
| [Plugin docs](https://crouter.docs.northlight.ai/docs/plugin) | What `@north-light/plugin` does and how a plugin is installed |
| [Commands](https://crouter.docs.northlight.ai/docs/plugin/commands) | Branches, leaves, parameters, outputs, groups and `who` |
| [Deploying](https://crouter.docs.northlight.ai/docs/plugin/deploying) | `auth: {issuer, audience, providerId}`, `ctx.caller`, versions and breaking changes |
| [Manage your app](https://sdk.docs.northlight.ai/docs/guides/manage-your-app.md) | The console's **Provider** tab beside the others |
