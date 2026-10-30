# ai-safety.txt use cases

This page opens with who reads the specification and where each reader's question is
answered, then gives each use case with what you can do today and where it stops.

## Who reads this

Each row names a reader, the question that brings them here and the text that answers it.
The specification is the Internet-Draft; a `draft-fane-ai-safety-txt-01.txt:N` reference
is a line of [its text rendering](../draft-fane-ai-safety-txt-01.txt) in this repository.

| Reader | The question they bring | Where it is answered |
|---|---|---|
| Security leader or CISO | Can a site's declaration change how our agents treat its content, and what do we require of an agent vendor? | [Your security team asks whether a declaration can loosen an agent's defenses](#your-security-team-asks-whether-a-declaration-can-loosen-an-agents-defenses); Section 5.1 (`draft-fane-ai-safety-txt-01.txt:260`); Section 5.2 (`draft-fane-ai-safety-txt-01.txt:273`); Section 5.4 (`draft-fane-ai-safety-txt-01.txt:302`) |
| IAM professional or identity architect | Does the file carry identity or credentials, and does it touch the identity systems I run? | It carries no identity, credential, key or signature: its six fields are three booleans, two URIs and a date, Section 3 (`draft-fane-ai-safety-txt-01.txt:151`). A consumer confirms that an `Attestation` record corresponds to the domain and comes from a verifier it trusts, Section 5.2 (`draft-fane-ai-safety-txt-01.txt:273`) |
| Platform or security engineer | How do I publish the file, how is it read, and how do I check a claim in it? | [You run a site and want agents to know how to reach you](#you-run-a-site-and-want-agents-to-know-how-to-reach-you); [You want to test a site's no-cloaking claim yourself](#you-want-to-test-a-sites-no-cloaking-claim-yourself); the well-known URI, Section 4 (`draft-fane-ai-safety-txt-01.txt:237`); Section 5.3 (`draft-fane-ai-safety-txt-01.txt:290`) |
| Agent developer or implementer | What does my agent do with a declaration? | [Your agent is about to read a site it has never seen](#your-agent-is-about-to-read-a-site-it-has-never-seen); the parsing rules of Section 3 (`draft-fane-ai-safety-txt-01.txt:157`, `draft-fane-ai-safety-txt-01.txt:210`); the consumer rules of Section 5 (`draft-fane-ai-safety-txt-01.txt:252`) |
| GRC, audit or compliance reader | Which controls does it support, and what does an auditor read? | The draft maps to no control framework. The evidence it points to is the `Attestation` and `Last-Verified` fields, Section 3 (`draft-fane-ai-safety-txt-01.txt:200`, `draft-fane-ai-safety-txt-01.txt:205`), and the check a consumer makes on an attestation record, Section 5.2 (`draft-fane-ai-safety-txt-01.txt:273`) |
| Standards reviewer (IETF or AIW participant) | What problem does it solve, how does it relate to robots.txt and to AI usage preferences, and what does it ask of IANA? | Section 1 (`draft-fane-ai-safety-txt-01.txt:93`); related work, Appendix A (`draft-fane-ai-safety-txt-01.txt:356`); the well-known URI registration, Section 6 (`draft-fane-ai-safety-txt-01.txt:306`); [Relationship to AI usage preferences](../README.md#relationship-to-ai-usage-preferences) in the README |

### The readers

- **Security leader or CISO**
  - Who: accountable for the organization's security risk, and for which agents and
    vendors may act on its systems.
  - Why they open it: the organization's agents read outside sites, and some of those
    sites publish a declaration about themselves.
  - What they decide: what an agent may and may not do with a declaration, and what to
    require of agent vendors on that point.
  - How they verify: Sections 5.1, 5.2 and 5.4, against the vendor's description of how
    its agent treats a declaration.
- **IAM professional or identity architect**
  - Who: runs the identity systems the organization already has, such as OAuth 2.0 and
    OpenID Connect, X.509 certificates, workload identity such as SPIFFE, DIDs and
    privileged access management.
  - Why they open it: to know whether the file is one more place where identity or
    credentials live.
  - What they decide: whether the issuer of an attestation record is one the organization
    trusts; nothing in the identity systems they run changes.
  - How they verify: Section 3, which defines six fields and no key, token or signature.
- **Platform or security engineer**
  - Who: deploys, operates and monitors the systems that serve the organization's sites
    and run its agents, and is paged when they fail.
  - Why they open it: they publish the file for the organization's domains, or run agents
    that read other domains' files.
  - What they decide: the values to declare, where the file is served, and whether to test
    a claim before an agent acts on it.
  - How they verify: a live declaration, and the comparison Section 5.3 describes.
- **Agent developer or implementer**
  - Who: writes the agent or the library that fetches and parses the file.
  - Why they open it: they need parsing rules that other consumers apply the same way.
  - What they decide: how to treat comments, unknown and repeated fields and invalid
    values, and how much weight a declaration gets.
  - How they verify: the parsing rules of Section 3. This repository ships no validator or
    test vectors.
- **GRC, audit or compliance reader**
  - Who: maps controls to evidence and reads the records an auditor asks for.
  - Why they open it: an auditor asks how the organization's agents treat a site's
    self-declared posture, or what the organization's own domains declare.
  - What they decide: whether a declaration backed by an `Attestation` record is evidence
    they accept, and how recent `Last-Verified` must be.
  - How they verify: Section 5.2, and the attestation record itself where one exists.
- **Standards reviewer (IETF or AIW participant)**
  - Who: a participant in an IETF or AIW discussion who judges whether a problem needs new
    work.
  - Why they open it: to judge whether a robots.txt-style declaration of safety posture is
    useful beside the IETF AIPREF work and `ai.txt`, which express usage preferences.
  - What they decide: whether to review or support the draft, and what to ask its author
    to change.
  - How they verify: the draft on the datatracker, Appendix A and Section 6.

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

Who acts: the developer whose agent fetches the file, and the person who delegated the
task.

Spec text: the six fields of Section 3 (`draft-fane-ai-safety-txt-01.txt:151`); a
consuming agent SHOULD treat each declaration as a hint that informs risk decisions, not
as a guarantee (`draft-fane-ai-safety-txt-01.txt:125`); and it MUST NOT treat a
declaration as proof, or relax its own defenses solely because of it, Section 5.1
(`draft-fane-ai-safety-txt-01.txt:260`).

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

Who acts: the platform or security engineer who runs the site.

Spec text: the `Contact` and `Consistent-Rendering` fields of Section 3
(`draft-fane-ai-safety-txt-01.txt:197`, `draft-fane-ai-safety-txt-01.txt:192`); the
well-known location of Section 4 (`draft-fane-ai-safety-txt-01.txt:237`); and the rule
that every field is OPTIONAL and a consumer MUST NOT infer a value for an absent one
(`draft-fane-ai-safety-txt-01.txt:211`).

## Your security team asks whether a declaration can loosen an agent's defenses

Your agents read outside sites as part of their work. Some of those sites publish a file
saying they are safe. Your security team asks whether an agent may treat such a site with
less suspicion, and what to require of the vendors whose agents you run.

Who acts: the security leader, and the vendor or internal team that builds the agent.

Without the specification: there is no shared rule. One agent ignores such claims, another
trusts any page that says it is safe, and a hostile site benefits from the second.

What the specification does: a consumer MUST NOT treat a declaration as proof of the
property it asserts, and MUST NOT relax its own defenses solely because a domain claims a
favorable posture (Section 5.1, `draft-fane-ai-safety-txt-01.txt:260`). Where an
`Attestation` URI is present, a consumer SHOULD retrieve and evaluate it, and MUST confirm
that the record corresponds to the domain and comes from a verifier it trusts (Section
5.2, `draft-fane-ai-safety-txt-01.txt:270`, `draft-fane-ai-safety-txt-01.txt:273`). A
declaration retrieved over an unauthenticated channel MUST NOT be treated as more
trustworthy than one not retrieved at all (Section 5.4,
`draft-fane-ai-safety-txt-01.txt:302`).

What to require of an agent vendor, in the draft's terms: a favorable declaration never
switches off a defense on its own, an attestation record counts only from a verifier you
trust, and the file is fetched over HTTPS.

How to see it work: not runnable here. The draft defines what a consuming agent does, and
this repository ships no validator or test vectors to run an agent against. What you can
read is a live declaration whose author applies Section 5.1 to its own content. On
2026-10-30 its field lines were:

```sh
curl -s https://opena2a.org/.well-known/ai-safety.txt | grep -vE '^(#|$)'
```

```
AI-Safe: true
Injection-Protected: false
Consistent-Rendering: true
Contact: mailto:info@opena2a.org
Last-Verified: 2026-07-22
```

The comments this command leaves out say why `Injection-Protected` is false and why the
file carries no `Attestation` field.

Out of scope: the format has no expiry and no signature field, so a declaration's
freshness rests on `Last-Verified` and its integrity on the transport. The draft does not
say how an agent weighs a declaration against its other signals.

## You want to test a site's no-cloaking claim yourself

A site declares `Consistent-Rendering: true`, meaning it serves agents the same page it
serves people. A site that serves agents a different page can hide hostile text from human
review. You want to check the claim instead of taking it on trust.

Who acts: the platform or security engineer, or an agent that checks the claim before it
relies on the page.

Without the specification: a site has no machine-readable way to state that it serves
agents and people the same page, so an agent has no claim to test before it relies on the
page.

What the specification does: a consumer SHOULD treat a `Consistent-Rendering` declaration
as testable, by fetching a page as both a human-like and an agent-like user agent and
comparing the results, and a mismatch is a strong negative signal that SHOULD override the
declaration (Section 5.3, `draft-fane-ai-safety-txt-01.txt:290`,
`draft-fane-ai-safety-txt-01.txt:294`).

How to see it work: fetch the same page with two user agents and compare digests. On
2026-10-30 the home page of opena2a.org, whose declaration says `Consistent-Rendering:
true`, returned the same bytes to both:

```sh
curl -s -A "Mozilla/5.0 (Macintosh; Intel Mac OS X 14_0) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/17.0 Safari/605.1.15" https://opena2a.org/ | shasum -a 256
curl -s -A "example-agent/1.0" https://opena2a.org/ | shasum -a 256
```

```
2746dcf4808cf4ef5939b9fa6df9160a7f7afa2bc73f89aa17f371ea9cd9bd7b  -
2746dcf4808cf4ef5939b9fa6df9160a7f7afa2bc73f89aa17f371ea9cd9bd7b  -
```

Out of scope: one matching pair shows that these two requests received the same bytes, not
that the site never cloaks, since a site can also branch on other headers, the client
address or the time. A page whose bytes change on every request, for example one that
embeds a timestamp, needs a comparison of content rather than of digests, and the draft
does not define the comparison.

## Why you can check this yourself

Why you can check this yourself: the Internet-Draft source and text are in this
repository ([`draft-fane-ai-safety-txt-01.xml`](../draft-fane-ai-safety-txt-01.xml),
[`draft-fane-ai-safety-txt-01.txt`](../draft-fane-ai-safety-txt-01.txt)) and on the [IETF
datatracker](https://datatracker.ietf.org/doc/draft-fane-ai-safety-txt/); and a live
declaration is served at `https://opena2a.org/.well-known/ai-safety.txt`.
