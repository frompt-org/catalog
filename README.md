# catalog — the frompts you register

**A foreign prompt — a frompt — is a prompt acquired from a non-local source, usually a URL, adopted on purpose by an agent that did not write it.** This is the catalog of them that people **register**: you trust this publisher's key once, and from then on your client resolves an id to verified bytes with nobody typing a digest. The digest does not go away; it moves from the phrase into the signed manifest, and the client checks every fetched document against it. That is how apt has worked for twenty years.

The frompts the documentation adopts as examples are published by [`protocol`](https://github.com/frompt-org/protocol) as a small signed catalog of their own. This is the one you use.

```
index.json                        the signed manifest; a client fetches this first
index.json.sig
INDEX.md                          the same thing, for people -- digests, never consent sentences
allowed_signers                   the key, identity frompt-catalog -- read it here once, pin it elsewhere
prompts/<id>/<version>.frompt.md  the documents, immutable
attestations/<digest>.*.json      what an assay observed about that exact document (see below)
```

## Registering

The tools live in the protocol repository; there is nothing to install beyond a clone:

```bash
git clone https://github.com/frompt-org/protocol ~/frompt-protocol
export PATH="$HOME/frompt-protocol/bin:$PATH"
```

Registration is recorded in that checkout's `fpa.registered`, so the checkout is the workspace:
one clone per set of agents that should share a consent record. Then, once:

```bash
# 1. bring the key in yourself -- copy allowed_signers from this page, a colleague, your MDM;
#    never let a tool fetch it from the catalog it will be used to check
fp-register https://raw.githubusercontent.com/frompt-org/catalog/main --pin latest --trust ./frompt-catalog.allowed_signers
# or through the GitHub API, with your own credential:
fp-register gh:frompt-org/catalog@main --pin latest --trust ./frompt-catalog.allowed_signers
```

The key's fingerprint is `SHA256:x4Z+wsX1wVrBt5SV/NWWgPfapXlpcmSk3rqZ36Y/rKY`. The registration line records it, and a manifest later signed by any other key -- even one you also trust for another catalog -- is refused until you re-register on purpose.

That writes one line to `fpa.registered` -- catalog, key fingerprint, pin mode, when, who -- and that line is the consent. From then on:

```bash
fp-resolve pr-review@latest --from gh:frompt-org/catalog@main   # this catalog's manifest, bytes verified against it
fp-resolve pr-review@2.0.0  --from gh:frompt-org/catalog@main   # exactly that one
fp-verify  pr-review        --from gh:frompt-org/catalog@main   # the managed path: signature, expiry, serial, digest
```

`--from` is what makes it *this* catalog: without it a resolver reads whatever index sits in its own checkout, which is a different publisher. The pin mode you registered with is enforced by the resolver — `latest` floats; `version` and `digest` require a matching line in `fpa.lock`, and re-pinning is a change somebody reviews.

**Do not take `allowed_signers` from this repository at verification time.** A key fetched from the same host as the signature it validates proves only that the two agree with each other. Copy it once, out of band, and pin it.

## What is in here

Seeded on 2026-09-09 from the protocol repo's examples. Versions are immutable: a change is a new version. Submissions are pull requests. Nothing is admitted, nothing is ranked, and being listed here makes a prompt no more official than one served from your own repo -- a catalog is a shape, not a privilege.

## Assay

Every document here is scanned at publish time and the scanner's raw output is stored beside it, keyed by digest. That is an **attestation**: an observation about these exact bytes, as data. It is not a verdict and there is no score threshold. Read the observation, then read the document; the second is the one that catches things.

Protocol, tools, and the rest: [`frompt-org/frompt`](https://github.com/frompt-org/frompt).

## License

Apache License 2.0 — see [`LICENSE`](LICENSE) and [`NOTICE`](NOTICE). Copyright 2026 Ramazan Polat.
