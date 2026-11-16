<!-- The README of github.com/mlreps/cli — the public, releases-only repository.
     ⚠ THIS FILE IS THE ORIGINAL; the one on GitHub is a copy of it. It lives here because the claims it
     makes about verification are the same claims `public/install.sh` and the release notes make, and
     `channels-check` holds all three against `docs/cli-distribution.md` §6 in both directions. A README
     edited in GitHub's web editor is a fourth wording of one fact that no gate can see (rule 5).
     Publish it with: gh api -X PUT repos/mlreps/cli/contents/README.md -f sha=<current> -f content=<base64>. -->

# MLReps CLI — releases

**There is no source code in this repository, and there never will be.** This is the public place the
*released builds* of the MLReps command-line tool are published to: one archive per platform, the
checksums that prove you got what we built, and a bill of materials. The CLI itself is built from a
private repository.

MLReps is a codebase auditor and a merge guard — it reads a repository and tells you how that software
actually works, what a change will touch before it is merged, and **how much of the repository it could
not read**. On the command-line path your source never leaves your machine.

→ **https://www.mlreps.com**

## Installing

Three ways. Only the first works today; the other two become available with the first release.

**With Node.js 20 or newer** — nothing in this repository is needed:

```sh
npm install -g mlreps
```

**A single binary, no Node at all:**

```sh
curl -fsSL https://www.mlreps.com/install.sh | sh
```

**Homebrew:**

```sh
brew install mlreps/tap/mlreps
```

macOS and Linux, x64 and arm64. There is **no Windows build** — the merge guard is only measured on
macOS and Linux — and no musl build, so Alpine is not covered; the npm package runs wherever Node 20
runs, including both.

## What a release contains

| asset | what it is |
| --- | --- |
| `mlreps-<version>-darwin-arm64.tar.gz` | the standalone build for Apple silicon Macs |
| `mlreps-<version>-darwin-x64.tar.gz` | …for Intel Macs |
| `mlreps-<version>-linux-x64.tar.gz` | …for Linux on x86-64 |
| `mlreps-<version>-linux-arm64.tar.gz` | …for Linux on arm64 |
| `SHA256SUMS` | one SHA-256 line per archive in that release |
| `mlreps-sbom.cdx.json` | a CycloneDX bill of materials for what went into the build |

Each archive unpacks to a directory holding the binary and the two things it reads from beside itself:
the TypeScript standard library and the graph-layout engine. Nothing is installed anywhere else, and
nothing is run at install time.

The same version number is published on every channel. If npm has `1.0.1`, the binaries here are the
same `1.0.1` code and answer identically — that equivalence is held by a gate on every build, which runs
all three builds of the CLI against one fixture and fails if any of them answers differently.

## Verifying a download

Every release publishes `SHA256SUMS` as its own asset, covering every archive in that release. Download
both, then:

```sh
shasum -a 256 -c SHA256SUMS --ignore-missing    # macOS
sha256sum --check SHA256SUMS --ignore-missing   # Linux
```

The install script above does exactly this and **refuses to install anything that does not match**, so a
corrupted or tampered download stops there rather than landing on your PATH.

**What that proves, and what it does not.** It proves the bytes you received are the bytes this release
published. It does not prove who built them: anybody who can write to this repository's releases could
replace an archive and its `SHA256SUMS` together. Closing that needs a build attestation a stranger can
check, and there is **no public build attestation to check** here yet — the build is attested, but the
workflow that does it runs in the private repository, so the record is not readable to you and no
command we could print would make it so.
Rather than print a verification step that fails for everybody who tries it, this page says plainly that
checksums are what verification rests on today. If the build source becomes public, the attestation
becomes checkable and this section will say so.

**macOS builds are ad-hoc signed and not notarized.** That is a deliberate, stated choice. Ad-hoc
signing is what lets the binary run at all on Apple silicon; it is not an Apple Developer ID and carries
no identity, so it tells you nothing about who built the file. A practical consequence: if you download
an archive through a browser, macOS marks it quarantined and Gatekeeper will refuse the binary until you
clear that attribute. Installing with the script or with Homebrew avoids it, because `curl` and `brew`
set no quarantine flag.

## Issues are off here

Issues, the wiki and projects are disabled on this repository on purpose: a releases-only repo cannot
answer a question about code nobody can see. Write to **connect@mlreps.com**.

## Where the code is

Private. What is public is the product: the npm package, these binaries, their checksums and the
measurements on the website — including the ones that say what the reader could not read.
