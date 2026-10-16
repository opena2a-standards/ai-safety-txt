# ai-safety.txt

The domain AI-safety declaration format. A file published at `/.well-known/ai-safety.txt`
where a domain declares its AI-safety posture to the agents that read its content.

Specified as an IETF Internet-Draft, current revision
[draft-fane-ai-safety-txt-01](https://datatracker.ietf.org/doc/draft-fane-ai-safety-txt/) (submitted 2026-07-23).
Spec page: [specs.opena2a.org/specs/ai-safety](https://specs.opena2a.org/specs/ai-safety).

## What it is

A robots.txt-equivalent for AI safety. A domain publishes a short text file; an agent
fetches it before acting on a page. A declaration is self-asserted: it is a hint, not
proof. A consuming agent should check the claim against the `Attestation` record where present.

Read the current draft: [`draft-fane-ai-safety-txt-01.txt`](draft-fane-ai-safety-txt-01.txt).
To publish a declaration, start from the [example](#example) with your own values.

## Use cases

### Your agent is about to read a site it has never seen

An agent fetches pages as part of its task. Some sites carry injection-shaped text, by
design in the case of a security research site that quotes payloads, and the agent has
no signal about a site's posture before it reads. The person who delegated the task gets
whatever the page says.

A site publishes a short text file at `/.well-known/ai-safety.txt` with six fields:
whether the domain vouches for its content's origin and intent, whether it is hardened
against embedded injection, whether humans and agents see the same page, a contact, an
attestation record and the date last verified. The declaration is self-asserted: a hint
for the agent's risk decision, not proof.

What you can do today: read a live declaration, including one that deliberately declares
`Injection-Protected: false` and says why.

```
curl https://opena2a.org/.well-known/ai-safety.txt
```

Where it stops today: the format has no expiry and no signature field, and this
repository ships the drafts and no validator.

### You run a site and want agents to know how to reach you

Agents visit your site. When one misbehaves, its operator has no contact meant for this,
and a site that serves agents a different page from the one humans see is itself an
attack path.

The `Contact` field gives agent operators a security or abuse address, and
`Consistent-Rendering` states that identical content is served to human and agent user
agents.

What you can do today: publish a declaration in the format of the example below at
`/.well-known/ai-safety.txt` on your domain, with your own values. The example describes
a hypothetical `example.com`; copied unchanged, it makes claims about your domain that
nobody has verified.

All six fields are optional. Declare only what is true for your
domain (the draft makes `AI-Safe: false` the correct value for a domain that is unsure),
and omit `Attestation` until an external verification record for your domain exists.

Where it stops today: the same limit applies.

Why you can check this yourself: the Internet-Draft source and text are in this
repository ([`draft-fane-ai-safety-txt-01.xml`](draft-fane-ai-safety-txt-01.xml),
[`draft-fane-ai-safety-txt-01.txt`](draft-fane-ai-safety-txt-01.txt)) and on the [IETF
datatracker](https://datatracker.ietf.org/doc/draft-fane-ai-safety-txt/); and a live
declaration is served at `https://opena2a.org/.well-known/ai-safety.txt`.

## The format

`Field: value` lines, one per line. A line whose first non-whitespace character is `#` is a
comment. Six fields are defined.

| Field | Type | Meaning |
|---|---|---|
| `AI-Safe` | boolean | Content is authored or reviewed by the domain, not user-generated, and not published with intent to harm a consuming agent: provenance and intent, not a claim that it is harmless to process. |
| `Injection-Protected` | boolean | Content is hardened against embedded prompt injection. |
| `Consistent-Rendering` | boolean | Identical content served to human and agent user agents (no cloaking). |
| `Contact` | URI | Security or abuse contact. |
| `Attestation` | URI | External verification record for the declaration. |
| `Last-Verified` | ISO 8601 date | When the declaration was last verified. |

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

## License

Apache-2.0.
