# Deprecation Policy

How an API retires things, read off the provider's own deprecation policy: the minimum notice, the support window for old versions, the channels deprecations are announced on, whether the RFC 9745 `Deprecation` and RFC 8594 `Sunset` headers are sent, the versioning scheme, and what the provider counts as a breaking change.

This is the schema behind the [Deprecation Policy property](https://apicommons.org/common/deprecation-policy/)
in [API Commons](https://apicommons.org).

## Why it matters

API Commons was started to make the business side of API onboarding machine-readable. Finding an API is
the easy part; deciding whether you are allowed to use it — for this purpose, in this country, with this
data, by this kind of client — still means a person reading legal pages. When the answer is a document a
crawler, a catalog or an agent can read, discovery and onboarding can be automated end to end, and a
consumer can compare providers on the terms rather than on the marketing.

## Artifacts

- **[deprecation-policy-json-schema.yml](deprecation-policy-json-schema.yml)** — the JSON Schema (2020-12).
- **[deprecation-policy-example-1.yml](deprecation-policy-example-1.yml)** — a standalone document, as written to `all/<slug>/deprecation/<slug>-deprecation-policy.yml`.
- **[deprecation-policy-example-2.yml](deprecation-policy-example-2.yml)** — the same document inside the `DeprecationSchedule` apis.yml property envelope.
- **[validate.py](validate.py)** — validates any document against the schema.

Both examples describe **Example Weather API**, a fictional provider. The clauses are illustrative and are
not the terms of any real company.

## Fields

Every document carries the common envelope:

```yaml
deprecation_policy: 0.1.0
provider: Example Weather API
aid: example-weather-api
url: <the page it was read from>
source_date: YYYY-MM-DD
x-apis-io-derivation:
  synthetic: false
  method: harvested
  read: YYYY-MM-DD
  tool: engine/business/harvest-business.py
  model: <model id>
```

and then:

| Field | Shape | Meaning |
|---|---|---|
| `minimum_notice` | `{period, text}` | Minimum notice before removal |
| `support_window` | `{period, text}` | How long old versions stay supported |
| `announcement_channels[]` | `{channel, text}` | email / changelog / blog / dashboard / header / status / other |
| `headers` | `{deprecation, sunset, text}` | RFC 9745 Deprecation and RFC 8594 Sunset headers |
| `versioning_scheme` | `{value, text}` | The versioning scheme as named |
| `breaking_change_definition` | `{text}` | What counts as breaking |

## The stance enum

Wherever a permission is described: `allowed | prohibited | conditional | unstated`. Every stance other
than `unstated` must carry a verbatim `text` — the schema enforces it. `unstated` means the page says
nothing, which is a valid, recorded result and is never filled in with a guess. Periods ("30 days",
"12 months") are copied as written and never normalized.

## Provenance: verbatim text, never synthetic

Only documents read from the provider's own page are published. There is no template fill. The page is
fetched by code; a model only transcribes rows from it; every row's `text` is checked word for word
against the fetched page and dropped if it does not match. `x-apis-io-derivation.synthetic` is always
`false` and `method` is always `harvested` — the schema rejects anything else.

Sources (apis.yml property types): Deprecation, DeprecationPolicy, DeprecationNotice, Sunset, Deprecations.

## Standard it profiles

[RFC 8594 — The Sunset HTTP Header Field](https://www.rfc-editor.org/rfc/rfc8594) and [RFC 9745 — The Deprecation HTTP Header Field](https://www.rfc-editor.org/rfc/rfc9745). `headers` records whether the provider says it sends either one; the policy text itself stays verbatim.

## Using it

```
pip install jsonschema pyyaml
python3 validate.py deprecation-policy-example-1.yml deprecation-policy-example-2.yml
```

Exits non-zero on any failure, so it can gate a pipeline.

## License

Two licenses, by kind of thing:

- **Artifacts** — the schemas and examples — are **[CC BY-NC-SA 4.0](LICENSE)** (Attribution–NonCommercial–ShareAlike).
- **Code** — the validator — is **[Apache-2.0](LICENSE-CODE)**.

## Part of API Commons

A machine-readable building block from **[API Commons](https://apicommons.org)**. Related business-layer blocks:
[terms-of-service](https://github.com/api-commons/terms-of-service) ·
[privacy-policy](https://github.com/api-commons/privacy-policy) ·
[deprecation-policy](https://github.com/api-commons/deprecation-policy) ·
[compliance](https://github.com/api-commons/compliance) ·
[eligibility](https://github.com/api-commons/eligibility) ·
[ai-preferences](https://github.com/api-commons/ai-preferences) ·
[data-license](https://github.com/api-commons/data-license) ·
[service-level-agreement](https://github.com/api-commons/service-level-agreement) ·
[plans](https://github.com/api-commons/plans) ·
[rate-limits](https://github.com/api-commons/rate-limits)
