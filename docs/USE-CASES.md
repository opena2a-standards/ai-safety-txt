# ai-safety.txt use cases

## Your agent is about to read a site it has never seen

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

## You run a site and want agents to know how to reach you

Agents visit your site. When one misbehaves, its operator has no contact meant for this,
and a site that serves agents a different page from the one humans see is itself an
attack path.

The `Contact` field gives agent operators a security or abuse address, and
`Consistent-Rendering` states that identical content is served to human and agent user
agents.

What you can do today: publish a declaration in the format of the
[example](../README.md#example) at `/.well-known/ai-safety.txt` on your domain, with your
own values. The example describes a hypothetical `example.com`; copied unchanged, it makes
claims about your domain that nobody has verified.

All six fields are optional. Declare only what is true for your
domain (the draft makes `AI-Safe: false` the correct value for a domain that is unsure),
and omit `Attestation` until an external verification record for your domain exists.

Where it stops today: the same limit applies.

Why you can check this yourself: the Internet-Draft source and text are in this
repository ([`draft-fane-ai-safety-txt-01.xml`](../draft-fane-ai-safety-txt-01.xml),
[`draft-fane-ai-safety-txt-01.txt`](../draft-fane-ai-safety-txt-01.txt)) and on the [IETF
datatracker](https://datatracker.ietf.org/doc/draft-fane-ai-safety-txt/); and a live
declaration is served at `https://opena2a.org/.well-known/ai-safety.txt`.
