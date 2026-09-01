# Setup and test procedure

## Architecture

Use a small local Node.js HTTP service when the main application runtime cannot safely launch a local Huly CLI or client. The service should:

- Bind to `127.0.0.1` by default.
- Read Huly credentials from the process environment.
- Require `Authorization: Bearer <HULY_WRITE_KEY>` for mutation endpoints.
- Invoke an installed compatible Huly CLI through its JavaScript entrypoint or use a compatible official API client.
- Write structured operation input to a uniquely created OS temporary directory with restricted permissions.
- Delete the temporary directory in `finally`.

Avoid `shell: true`, command-string concatenation, committed `.env` files, credential logging, and blind full-document updates.

## Windows user configuration

The user must fill these values through Windows user environment variables or another secure local mechanism:

| Variable | Required | Purpose |
|---|---:|---|
| `HULY_URL` | Yes | Self-hosted Huly base URL |
| `HULY_WORKSPACE` | Yes | Workspace identifier from `/workbench/<workspace>/` |
| `HULY_EMAIL` + `HULY_PASSWORD` | Auth | Password authentication |
| `HULY_TOKEN` | Auth | Alternative token authentication when supported |
| `HULY_WRITE_KEY` | Yes | Local API caller authentication |
| `HULY_WRITE_HOST` | No | Defaults to `127.0.0.1` |
| `HULY_WRITE_PORT` | No | Defaults to a documented local port |

Use exactly one Huly authentication method. Never display the values during verification.

## Safe append algorithm

Given `oldText` and `appendText`:

1. Reject empty values and enforce size limits.
2. Build `newText = oldText + separator + appendText`.
3. Request replacement of one exact `oldText` occurrence only.
4. Fail if the old text is absent or ambiguous rather than falling back to full replacement.
5. Return the Huly object ID and `updated` status without returning credentials.

This acts as an optimistic concurrency guard. For richer concurrency control, prefer a Huly revision/version token when the chosen client exposes one.

## Test sequence

Before live testing, the user must supply or securely fill the connection settings, target Teamspace, Document ID, expected current text, and append test text. Do not use placeholder examples as real targets.

Run:

1. Configuration presence test.
2. Unit tests with a mocked Huly executor.
3. Project typecheck/build.
4. Local health endpoint test.
5. Read-only Huly connection and document resolution.
6. One authorized exact-match append.
7. Independent re-read verification.
8. Local server shutdown.

Warnings about model transactions are not proof of success or failure. Use the structured operation result and independent readback.
