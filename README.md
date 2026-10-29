# catalog — the frompts you register

**A foreign prompt — a frompt — is a prompt acquired from a non-local source, usually a URL, adopted on purpose by an agent that did not write it.** This is the catalog of them that people **register**: you trust this publisher's key once, and from then on your client resolves an id to verified bytes with nobody typing a digest. The digest does not go away; it moves from the phrase into the signed manifest, and the client checks every fetched document against it. That is how apt has worked for twenty years.

The frompts the documentation adopts as examples are published by [`protocol`](https://github.com/frompt-org/protocol) as a small signed catalog of their own. This is the one you use.

```
index.json                        the signed manifest; a client fetches this first
index.json.sig
INDEX.md                          the same thing, for people -- digests, never consent sentences
allowed_signers                   the key, identity frompt-catalog -- fetched once, checked against a fingerprint you hold
frompt-catalog.json               where this catalog is served from, its signing identity, its summaries
prompts/<id>/<version>.frompt.md  the documents, immutable
attestations/<digest>.*.json      what an assay observed about that exact document (see below)
```

## Registering

The tools live in the protocol repository; there is nothing to install beyond a clone:

```bash
git clone https://github.com/frompt-org/protocol ~/frompt-protocol
export PATH="$HOME/frompt-protocol/bin:$PATH"
export FPA_WORKSPACE="$HOME/.config/frompt"   # your trust record: keys, registrations, pins
```

`FPA_WORKSPACE` is where your consent lives -- one per set of agents that should share it. Without
it, the tools use their own checkout. Then, once:

```bash
fp-register https://raw.githubusercontent.com/frompt-org/catalog/main --key SHA256:x4Z+wsX1wVrBt5SV/NWWgPfapXlpcmSk3rqZ36Y/rKY --pin latest
# or through the GitHub API, with your own credential:
fp-register gh:frompt-org/catalog@main --key SHA256:x4Z+wsX1wVrBt5SV/NWWgPfapXlpcmSk3rqZ36Y/rKY --pin latest
```

`--key` fetches this catalog's `allowed_signers` and trusts the key in it only if its fingerprint
is the one you typed (FPA.md M7a). **Check that fingerprint somewhere other than this repository**
-- the protocol's README, a colleague, your MDM. A fingerprint read from the catalog it vouches for
proves only that the two agree with each other.

That writes one line to `fpa.registered` -- catalog, key fingerprint, pin mode, when, who -- and that
line is the consent. A manifest later signed by any other key, even one you also trust for another
catalog, is refused until you re-register on purpose. From then on:

```bash
fp-resolve pr-review@latest --from gh:frompt-org/catalog@main   # this catalog's manifest, bytes verified against it
fp-resolve pr-review@2.0.0  --from gh:frompt-org/catalog@main   # exactly that one
fp-verify  pr-review        --from gh:frompt-org/catalog@main   # the managed path: signature, expiry, serial, digest
```

`--from` is what makes it *this* catalog: without it a resolver reads the catalog it is standing in, or
the tools' own, which is a different publisher. The pin mode you registered with is enforced by the resolver —
`latest` floats; `version` and `digest` require a matching line in `fpa.lock`
(`fp-lock --update <id> --from <this catalog>`), and re-pinning is a change somebody reviews.

## What is in here

Seeded on 2026-09-09 from the protocol repo's examples. Versions are immutable: a change is a new version. Submissions are pull requests; a maintainer holding the key runs `fp-index && fp-sign` in a clone of this repository, with the protocol's tools on `PATH`. Nothing is admitted, nothing is ranked, and being listed here makes a prompt no more official than one served from your own repo -- a catalog is a shape, not a privilege.

## Assay

Every document here is scanned at publish time and the scanner's raw output is stored beside it, keyed by digest. That is an **attestation**: an observation about these exact bytes, as data. It is not a verdict and there is no score threshold. Read the observation, then read the document; the second is the one that catches things.

Protocol, tools, and the rest: [`frompt-org/frompt`](https://github.com/frompt-org/frompt).

## License

Apache License 2.0 — see [`LICENSE`](LICENSE) and [`NOTICE`](NOTICE). Copyright 2026 Ramazan Polat.
