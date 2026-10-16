# ai-safety.txt

The domain AI-safety declaration format. A file published at `/.well-known/ai-safety.txt`
where a domain declares its AI-safety posture to the agents that read its content.

Specified as an IETF Internet-Draft, current revision
[draft-fane-ai-safety-txt-01](https://datatracker.ietf.org/doc/draft-fane-ai-safety-txt/).
Spec page: [specs.opena2a.org/specs/ai-safety](https://specs.opena2a.org/specs/ai-safety).

## What it is

A robots.txt-equivalent for AI safety. A domain publishes a short text file; an agent
fetches it before acting on a page. A declaration is self-asserted: it is a hint, not
proof. A consuming agent should check the claim against the `Attestation` record where present.

Read the current draft: [`draft-fane-ai-safety-txt-01.txt`](draft-fane-ai-safety-txt-01.txt).
To publish a declaration, start from the [example](#example) with your own values.

## The format

`Field: value` lines, one per line. A line whose first non-whitespace character is `#` is a
comment. Six fields are defined.

| Field | Type | Meaning |
|---|---|---|
| `AI-Safe` | boolean | Content is authored or reviewed by the domain, not user-generated, and not published with intent to harm a consuming agent. |
| `Injection-Protected` | boolean | Content is hardened against embedded prompt injection. |
| `Consistent-Rendering` | boolean | Identical content served to human and agent user agents (no cloaking). |
| `Contact` | URI | Security or abuse contact. |
| `Attestation` | URI | External verification record for the declaration. |
| `Last-Verified` | ISO 8601 date | When the declaration was last verified. |

`AI-Safe` asserts provenance and intent, not a claim that the content is harmless to
process. Section 3 of the [draft](draft-fane-ai-safety-txt-01.txt) defines each field in full.

## Example

This is the example from Section 3 of the draft. Its values describe a hypothetical
`example.com`. Replace them with your own before publishing, and leave out `Attestation`
until a verification record for your domain exists.

```
# ai-safety.txt for example.com
AI-Safe: true
Injection-Protected: true
Consistent-Rendering: true
Contact: mailto:security@example.com
Attestation: https://registry.example.org/verify/example.com
Last-Verified: 2026-07-01
```

## The well-known URI

The file lives at `/.well-known/ai-safety.txt`, following RFC 8615.

## Relationship to AI usage preferences

ai-safety.txt declares a domain's safety posture toward consuming agents. It is distinct
from the IETF AIPREF working group and `ai.txt`, which express whether and how content may
be used (training, indexing, licensing). A domain may publish both; they answer different
questions.

## Documents

- [`draft-fane-ai-safety-txt-01.xml`](draft-fane-ai-safety-txt-01.xml): the Internet-Draft, revision 01 (RFCXML source).
- [`draft-fane-ai-safety-txt-01.txt`](draft-fane-ai-safety-txt-01.txt): revision 01 rendered as text.
- [`draft-fane-ai-safety-txt-00.xml`](draft-fane-ai-safety-txt-00.xml), [`draft-fane-ai-safety-txt-00.txt`](draft-fane-ai-safety-txt-00.txt):
  revision 00, superseded by -01; its document date is on its first page.
- [`docs/USE-CASES.md`](docs/USE-CASES.md): use cases, each with what you can do today and where it stops.

## License

Apache-2.0.
