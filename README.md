# ai-safety.txt

The domain AI-safety declaration format. A file published at `/.well-known/ai-safety.txt`
where a domain declares its AI-safety posture to the agents that read its content.

Specified as an IETF Internet-Draft:
[draft-fane-ai-safety-txt-00](https://datatracker.ietf.org/doc/draft-fane-ai-safety-txt/).
Spec page: [specs.opena2a.org/specs/ai-safety](https://specs.opena2a.org/specs/ai-safety).

## What it is

A robots.txt-equivalent for AI safety. A domain publishes a short text file; an agent
fetches it before acting on a page. A declaration is self-asserted: it is a hint, not
proof. A consuming agent verifies the claim against the `Attestation` record where present.

## The format

`Field: value` lines, one per line. Six fields are defined.

| Field | Type | Meaning |
|---|---|---|
| `AI-Safe` | boolean | Content is asserted safe for autonomous agent consumption. |
| `Injection-Protected` | boolean | Content is hardened against embedded prompt injection. |
| `Consistent-Rendering` | boolean | Identical content served to human and agent user agents (no cloaking). |
| `Contact` | URI | Security or abuse contact. |
| `Attestation` | URI | External verification record for the declaration. |
| `Last-Verified` | ISO 8601 date | When the declaration was last verified. |

## Example

```
AI-Safe: true
Injection-Protected: true
Consistent-Rendering: true
Contact: https://example.com/security
Attestation: https://registry.example.org/verify/example.com
Last-Verified: 2026-07-06
```

## The well-known URI

The file lives at `/.well-known/ai-safety.txt`, following RFC 8615.

## Relationship to AI usage preferences

ai-safety.txt declares a domain's safety posture toward consuming agents. It is distinct
from the IETF AIPREF working group and `ai.txt`, which express whether and how content may
be used (training, indexing, licensing). A domain may publish both; they answer different
questions.

## Documents

- `draft-fane-ai-safety-txt-00.xml` — the Internet-Draft (RFCXML source).
- `draft-fane-ai-safety-txt-00.txt` — the rendered text.

## License

Apache-2.0.
