---
description: "Add a permission (a scope) to the app so it can call a Northlight provider."
argument-hint: "[scope, or what the app should be able to do]"
---

Add a permission to the app, using the `use-our-apps` skill.

Asked for: $ARGUMENTS

1. If the app is not registered yet (no `CROUTER_CLIENT_ID` in `local/app.env`), register it first with `npx @north-light/sdk register`.
2. Find the exact scope string with `npx @north-light/sdk apps asks`. Never invent a scope string. If the person named a provider, list its tools with `providers.tools('<provider>')` once the scope is granted.
3. Add it with `npx @north-light/sdk apps set-scopes --add <scope>`.
4. Call the tool from the app with `providers.call`, taking the tool name and arguments from `providers.tools`.
5. Tell the person that people already connected approve the new list the next time they sign in, and that a new app is in testing mode until Northlight verifies it.
