---
name: huly-write-api
description: Build and test a local Node.js API for safe Huly document writes using environment-provided credentials and exact-match append semantics. Use for scaffolding, configuring, validating, or troubleshooting Huly write integrations; do not use for blind full-document overwrite or deletion.
---

# Huly Write API

Build a local write service that protects credentials and fails closed when the remote document has changed.

## Required user input

Before a live connection or write test, require the user to fill or securely configure:

- Huly base URL and Workspace identifier.
- One authentication method: email plus password, or a supported token.
- Target Teamspace name or ID and Document ID.
- Expected current document text and the exact test text to append.

Do not infer missing connection or target values from examples. Do not ask the user to paste passwords or tokens into chat. Prefer OS user environment variables, a credential store, or an interactive prompt. Report only `PRESENT` or `MISSING` for secret-bearing configuration.

## Build workflow

1. Inspect the target project runtime and its project instructions.
2. Read [references/setup-and-test.md](references/setup-and-test.md) for the service architecture and Windows configuration flow.
3. Implement a server-side/local-only API. Default to `127.0.0.1`; do not expose it to the LAN unless the user explicitly requests that scope.
4. Require a separate `HULY_WRITE_KEY` Bearer credential for callers.
5. Provide an exact-match append operation: accept `oldText` and `appendText`, then replace only that exact occurrence with `oldText + separator + appendText`. Never offer blind whole-document replacement as the default.
6. Pass structured CLI/API inputs through temporary files or typed client calls. Never interpolate user values into a shell command.
7. Remove temporary files in `finally`, close Huly clients, redact authentication errors, and return stable JSON errors.
8. Add unit tests for configuration presence, authentication, validation, safe append, concurrent-change rejection, and temporary-file cleanup.

## Live test gate

Run unit and build tests before a live mutation. For the live test:

1. Check the user-supplied connection fields for presence without displaying values.
2. Perform a read-only connection and target-document check.
3. Show the resolved document title, Teamspace, expected current text, and append text.
4. Obtain any required mutation authorization at the moment dictated by the active tool or browser policy.
5. Execute one exact-match append. If the current text differs, stop; do not retry with a full overwrite.
6. Re-read through an independent surface when practical and verify the appended text is visible.
7. Stop the temporary test server and report whether any remote write occurred.

For the request/response contract and expected status behavior, read [references/api-contract.md](references/api-contract.md).

## Result report

Report the configuration state, connection result, target identity, unit/build test results, live append result, verification result, local server status, and whether a Huly write occurred. Never include credentials, tokens, cookies, or complete authentication errors containing account identifiers.
