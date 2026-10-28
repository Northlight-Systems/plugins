---
description: "Move the app from the laptop to a real host and ask Northlight to verify it."
---

Take the app live, using the `get-started` skill and https://sdk.docs.northlight.ai/docs/guides/go-live.md.

1. Read the Go live page and go through its production checklist against the project: a persistent `ConnectionStore`, the key file in a secret manager, a production redirect URI and origin, an exact SDK pin, one place for dead connections (`onRefusal`), and sign-in secrets redacted from logs.
2. Fix what the project does not meet, and say which items you changed.
3. A new app is in testing mode: only its owner and up to 100 testers can sign in. To open it to everyone, the person emails support@northlight.ai with the app's name and client ID, where it runs, and what it asks for and why. That email is theirs to send.
4. Listing in the marketplace is the person's choice, in the console under **Profile → Marketplace**.
