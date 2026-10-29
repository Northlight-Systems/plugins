---
name: know-the-person
description: "Learn who the person is and what they care about from their first sign-in, with the context provider and their public documents."
metadata:
  sdk-version: "0.3.548"
---

This skill covers one part of building a Northlight app. Account, login and registration are in the `northlight` skill; read it first if the project has no registered app yet.

# Let your app know the person

A person who signs in to your app already has a Northlight agent that knows them. Your app can ask it about them from the first sign-in, so onboarding needs fewer questions and the product fits them sooner.

```ts
const runtime = await oauth.client(userId);
const advice = await runtime.providers.call({
  provider: 'context',
  tool: 'ask informed', // pass the name exactly as providers.tools('context') lists it
  arguments: {request: 'Suggest the first three topics for this person\'s reading list.'},
});
```

`ask informed` (scope `context:ask_informed`) answers from everything their agent knows, private memories included, without handing you private details. `ask public` (scope `context:ask_public`) answers from their public memories only.

## Guides

| Page | What it covers |
|---|---|
| [Context](https://sdk.docs.northlight.ai/docs/providers/context.md) | The context provider's scopes, setup and tools, including public profile lookups |
| [Files and documents](https://sdk.docs.northlight.ai/docs/guides/files-and-documents.md) | Read the documents the person made public with `memory.list` and `memory.get`, with no scope |
| [Providers](https://sdk.docs.northlight.ai/docs/concepts/providers.md#providers-your-app-requires) | Require `context` so the person sets it up at first sign-in |
