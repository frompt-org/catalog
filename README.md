# stable — the catalog you register

**A foreign prompt — an f-prompt — is a prompt acquired from a non-local source, usually a URL, adopted on purpose by an agent that did not write it.** This is the catalog of them that people **register**: you trust this publisher's key once, and from then on your client resolves an id to verified bytes with nobody typing a digest. The digest does not go away; it moves from the phrase into the signed manifest, and the client checks every fetched document against it. That is how apt has worked for twenty years.

The other catalog in this org, [`reference`](https://github.com/f-prompts/reference), is the exemplar: small, what the documentation and the conformance suite point at. This one is what you use.

```
index.json                        the signed manifest; a client fetches this first
index.json.sig
INDEX.md                          the same thing, for people -- digests, never consent sentences
allowed_signers                   the key, identity f-prompts-stable -- read it here once, pin it elsewhere
prompts/<id>/<version>.prompt.md  the documents, immutable
attestations/<digest>.*.json      what an assay observed about that exact document (see below)
```

## Registering

Once, in a workspace:

```bash
# 1. bring the key in yourself -- copy allowed_signers from this page, a colleague, your MDM;
#    never let a tool fetch it from the catalog it will be used to check
fp-register gh:f-prompts/stable@main --pin latest --trust ./f-prompts-stable.allowed_signers
# once public:
fp-register https://raw.githubusercontent.com/f-prompts/stable/main --pin latest --trust ./f-prompts-stable.allowed_signers
```

The key's fingerprint is `SHA256:x4Z+wsX1wVrBt5SV/NWWgPfapXlpcmSk3rqZ36Y/rKY`. The registration line records it, and a manifest later signed by any other key -- even one you also trust for another catalog -- is refused until you re-register on purpose.

That writes one line to `fpa.registered` -- catalog, key fingerprint, pin mode, when, who -- and that line is the consent. From then on:

```bash
fp-resolve pr-review@latest              # newest version, bytes verified against the manifest
fp-resolve pr-review@2.0.0               # exactly that one
fp-verify  pr-review --from gh:f-prompts/stable@main   # the managed path: signature, expiry, serial, digest
```

**Do not take `allowed_signers` from this repository at verification time.** A key fetched from the same host as the signature it validates proves only that the two agree with each other. Copy it once, out of band, and pin it.

## What is in here

Seeded from `reference` on 2026-09-09; the two will diverge. Versions are immutable: a change is a new version. Submissions are pull requests. Nothing is admitted, nothing is ranked, and being listed here makes a prompt no more official than one served from your own repo -- a catalog is a shape, not a privilege.

## Assay

Every document here is scanned at publish time and the scanner's raw output is stored beside it, keyed by digest. That is an **attestation**: an observation about these exact bytes, as data. It is not a verdict and there is no score threshold. Read the observation, then read the document; the second is the one that catches things.

Protocol, tools, and the rest: [`f-prompts/f-prompts`](https://github.com/f-prompts/f-prompts).
