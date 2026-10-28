---
name: use-our-ai
description: "Start agent runs on each person's own Northlight runtime, with their memory, files and tools, paid by their AI subscription. Typed results, streaming, background runs and documents."
metadata:
  sdk-version: "0.3.543"
---

This skill covers one part of building a Northlight app. Account, login and registration are in the `northlight` skill; read it first if the project has no registered app yet.

# Let your app use our AI

Your app starts agents on the person's own Northlight runtime. Each run has their memory, files and tools, works under the scopes they approved, and its model calls are paid by their AI subscription, not by you.

```ts
import {z} from 'zod';
import type {ConnectedCrouter} from '@north-light/sdk';

const Profile = z.object({
  display_name: z.string(),
  bio: z.string(),
  facts: z.array(z.string()),
});

/** Start a run on the person's runtime and get a value back that was checked against the schema. */
export async function draftProfile(runtime: ConnectedCrouter) {
  const outcome = await runtime.runs.parse({
    prompt: 'Draft a public profile for me from my notes.',
    scopes: ['crtr:llm'], // submitting a result needs no other scope
    output_schema: Profile,
  });

  if (outcome.kind === 'result') return outcome.output_parsed; // {display_name, bio, facts}, typed from Profile
  if (outcome.reason === 'declined') {
    // The agent could not honestly satisfy the schema; outcome.details says why.
    throw new Error(`Declined: ${outcome.details?.reason ?? 'no reason given'}`);
  }
  throw new Error(`The run ended without a result: ${outcome.reason}`); // canceled, deadline_exceeded or runtime_error
}
```

`runs.parse` starts a run and returns a value checked against your schema. `runs.start` returns at once and lets you follow the run, reply to it or leave it running.

## Guides

| Page | What it covers |
|---|---|
| [Runs](https://sdk.docs.northlight.ai/docs/concepts/runs.md) | Start a run, its status and outcome, a run's own scopes, and how to bound one |
| [Typed results](https://sdk.docs.northlight.ai/docs/guides/typed-results.md) | Get a value back with `runs.parse` and a schema; never parse the trace |
| [Stream a run](https://sdk.docs.northlight.ai/docs/guides/stream-a-run.md) | Show the agent's text and tool calls as they happen |
| [Background runs](https://sdk.docs.northlight.ai/docs/guides/background-runs.md) | Learn when a run you did not wait on has ended, with `RunWatcher` or run notices |
| [Files and documents](https://sdk.docs.northlight.ai/docs/guides/files-and-documents.md) | Download a file from a run, and read the person's public documents |
| [Many users](https://sdk.docs.northlight.ai/docs/guides/many-users.md) | Keep each person's runtime separate when people meet in your app |

To schedule work, call `runs.start` from your own timer. A run's agent may also schedule itself with `crtr cron add` when the run holds `crtr:schedule` ([Scopes and grants](https://sdk.docs.northlight.ai/docs/concepts/scopes-and-grants.md)).

An app that pays for its own model calls instead can serve them from its own service with `inferenceEndpoint` from `@north-light/sdk`; see the [release notes](https://sdk.docs.northlight.ai/docs/release-notes.md) and the [reference](https://sdk.docs.northlight.ai/docs/reference.md).
