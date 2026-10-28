---
description: "Offer the app's tools to the person's own AI, as a Northlight provider."
argument-hint: "[what the tools do]"
---

Make the app a provider, using the `let-their-ai-use-your-app` skill.

Asked for: $ARGUMENTS

1. Define the tools with `definePlugin({provider: true, version, groups, commands})` from `@north-light/plugin`. Every command names its `group`; each group is a scope named `<client id>:<group>`, and its `consentLine` is what the consent page shows.
2. Serve the plugin at an HTTPS URL with `createFetchHandler` (or `createNodeHandler` for Node's `http` server), with `auth: {issuer, audience, providerId}`.
3. Ask the person to open the app's **Provider** tab in the console (https://account.northlight.ai/console), enter the **Endpoint URL**, and choose **Add provider role**. The console is a browser-only step.
4. If the app charges, set the price on the same tab.
5. To let other developers find it, set a **Docs link** on the **Profile** tab (the `build-on-your-app` skill).
