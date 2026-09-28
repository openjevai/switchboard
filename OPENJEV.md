# OpenJEV support

This fork of [ruban-24/switchboard](https://github.com/ruban-24/switchboard) adds
**optional** support for [OpenJEV](https://openjev.sh), a free community gateway
to the same Jev model that TypeSafe provides. TypeSafe remains the default; anyone
with a TypeSafe key sees zero behaviour change.

Jev is built by [TypeSafe](https://typesafe.ai). OpenJEV is a community gateway to
it, not a replacement.

## What was added

- `src/jev.ts` — `OPENJEV_BASE_URL` constant (`https://api.openjev.sh`) and
  `openjevModel()` helper (default model id `openjev`); the `connection.provider`
  type now accepts `'openjev'`.
- `src/classifier.ts` — new `openjev` provider case in `settings()`: uses the
  TypeSafe System One adapter, base URL `https://api.openjev.sh`, model `openjev`,
  key env `OPENJEV_API_KEY` (or `SWITCHBOARD_API_KEY`). Updated the invalid-provider
  error message.
- `src/settings.ts` — `'openjev'` added to the `Connection.provider` type, the
  `parseConnection` allow-list, `credentialKeys()`, and `connectionEnvironment`'s
  optional-key list.
- `src/core/types.ts` — `ClassificationDiagnostics.provider` accepts `'openjev'`.
- `src/jev-questions.ts` — `parseJevAnswers` source `provider` type accepts
  `'openjev'`.
- `src/core/classification-diagnostics.ts` — `parseDiagnostics` accepts
  `'openjev'`; the requested-model regex now also matches `openjev`.
- `src/cli.ts` — `switchboard init` offers OpenJEV as provider option 5.
- `.env.example` — `SWITCHBOARD_PROVIDER=openjev` / `OPENJEV_API_KEY` preset.
- `docs/classifiers.md` — OpenJEV row in the supported-contracts table and the
  environment-overrides table.
- `README.md` — short note after the project intro, TypeSafe credited first.

## Provider selection rule

1. Explicit choice wins: `SWITCHBOARD_PROVIDER=openjev` (or picking option 5 in
   `switchboard init`).
2. Otherwise, if a TypeSafe key is set → TypeSafe (unchanged default).
3. Otherwise, if only `OPENJEV_API_KEY` is set → set `SWITCHBOARD_PROVIDER=openjev`
   to use OpenJEV.

This mirrors the existing provider logic (`typesafe` / `openrouter` / `vercel`).

## How to configure

Set environment variables (or run `switchboard init` and choose option 5):

```sh
SWITCHBOARD_PROVIDER=openjev
OPENJEV_API_KEY=<key from https://openjev.sh/dashboard>
```

`SWITCHBOARD_BASE_URL` and `SWITCHBOARD_MODEL` overrides work the same as for the
other providers.

## Verification

A live `POST https://api.openjev.sh/v1/systemone` request with model `openjev`,
state `ping`, and one noul question returned HTTP 200 with a valid answer.
No repository code was executed during this port.

## Upstream

Original project: https://github.com/ruban-24/switchboard by @ruban-24.
