# Security reviews of open-source Bitcoin software

By MrHodl ([@MrHodl](https://x.com/MrHodl) on X). Every finding named below
is public or fixed; reports still under embargo are counted, not named.

## What we do

We review open-source Bitcoin software: wallets, signers, PSBT tools and
servers. The work is AI-assisted. Several AI models read each codebase
independently and raise claims. Before anything is reported, every claim is
checked against the code, the relevant BIP or the project's own test vectors,
and traced through the code paths. We read the code; we don't build or run
it, and our reports say so. Most of what the models raise does not survive
that checking, and that filtering is what maintainers get from us: fewer,
real findings. We look hardest at what other reviews skip: crafted inputs
(files, QR codes, PSBTs) that lie about themselves, and code that should fail
closed but does not. We report privately first, through each project's
security policy, and never test live systems.

## How findings are checked

As of November 2026:

- Several open-weight models review the code, each on its own. Every claim is
  then cross-checked by models that did not make it.
- Every claim that goes into a report is then traced through the source with a
  frontier model that is released only to vetted security researchers
  (Mythos). Anything rated medium or above is read a second time,
  independently.
- Models do the reading at every step. Model output alone is never a finding:
  a claim is reported only after that tracing. Each report says how it was
  made.

## Public results

**Faraday** (bitcoinpasada/faraday), October 2026
- Three published security advisories, all fixed in v0.1.8:
  - [GHSA-7h4m-p8qx-pxrf](https://github.com/bitcoinpasada/faraday/security/advisories/GHSA-7h4m-p8qx-pxrf) (critical): five critical/high findings, including FROST nonce handling and the multisig change check.
  - [GHSA-w52w-c7x4-8pw5](https://github.com/bitcoinpasada/faraday/security/advisories/GHSA-w52w-c7x4-8pw5) (high): secret QR frames not wiped.
  - [GHSA-vxpw-8g7c-8qmr](https://github.com/bitcoinpasada/faraday/security/advisories/GHSA-vxpw-8g7c-8qmr) (high): finalized inputs bypassed the unverified-amount guard.
- Two public audits of later commits (issues #4 and #5), 81 medium/low findings (some overlap between the two). The maintainer's own verdicts: 56 fixed, 9 by design, 8 did not hold, 8 moot (code removed).

**VoidForge** ([random21m/voidforge](https://gitlab.com/random21m/voidforge) on GitLab; on GitHub until November 2026), October and November 2026
- GHSA-8xcw-qx5p-8rmr: the release workflow could attest a tampered build. Confirmed and fixed the same day (1.8.1); the maintainer rated it medium. The advisory page went offline when the project left GitHub; the project's [SECURITY.md](https://gitlab.com/random21m/voidforge/-/blob/main/SECURITY.md) lists the report under its security acknowledgements.
- A second report (November 2026, medium): after the inputs were edited, DOWNLOAD could save an old seed with the new rolls, and the old fingerprint and address could stay on screen. Fixed in v1.26.1 and v1.29.1 ("Stale results" in the project's [README](https://gitlab.com/random21m/voidforge/-/blob/main/README.md)). The GitHub release notes that credited the report went offline with the move.

**Sparrow** (sparrowwallet/sparrow), October 2026
- A private report with one medium and four low findings. Craig Raw confirmed
  the medium (adding an account to a multisig wallet with mixed seed and
  watch-only keystores could leave the wallet decrypted for the session) and
  fixed it within two days; the fix shipped in
  [2.5.6](https://github.com/sparrowwallet/sparrow/releases/tag/2.5.6). Of the
  rest, one was already fixed before the commit we reviewed (our miss), and
  three were left as informational or by design.

**Foxy** (getfoxy), November 2026
- Reviewed at the Foxy team's request. Both advisories were published on 3 November 2026 with patched versions:
  - [GHSA-38xx-q9p3-2f57](https://github.com/getfoxy/iOS/security/advisories/GHSA-38xx-q9p3-2f57) (critical, iOS wallet): a failed install-marker write could make the next launch treat it as a reinstall and delete the seed. Fixed in Build 23.
  - [GHSA-vppf-jq63-hwx9](https://github.com/getfoxy/card/security/advisories/GHSA-vppf-jq63-hwx9) (medium, ecash card applet): the Schnorr signing nonce stayed in persistent key storage after signing. Fixed in card 1.25.

**ZEUS** (Lightning wallet), October 2026
- A private code and security review. ZEUS credited it publicly in its
  [v13.2.4 release thread](https://x.com/ZeusLN/status/2115791922774348183) on 29 October 2026 ("code and security scans",
  @MrHodl).

**BIPs** (bitcoin/bips), November 2026
- [bitcoin/bips#2345](https://github.com/bitcoin/bips/pull/2345): one of
  BIP-373's test vectors repeated another case, so the x-only output
  participant keys case was never really tested. The fix replaced it with the
  vector Bitcoin Core already uses. Merged by jonatack; Ava Chow (achow101),
  the author of BIP-373, gave a post-merge ACK: "This matches the vector for
  this case in Bitcoin Core."

**rbitcoin** (rbitcoin-org/rbitcoin, a Bitcoin full node in Rust), November 2026
- A private report on 10 November 2026: one connected peer could grow a
  node's outbound queue without limit, because compact-filter replies and
  pongs were not counted against the per-peer send budget. The maintainer
  recorded five findings from it, each rated medium and fixed, in the
  project's [external findings](https://github.com/rbitcoin-org/rbitcoin/tree/master/docs/external_findings)
  (109 to 113), and credited the review in the changelog.
- A public review of [pull request #1501](https://github.com/rbitcoin-org/rbitcoin/pull/1501)
  on 13 November 2026, a change to how the store records spent outputs. Of
  four points, three were fixed within about an hour, among them a schema
  version bump so that an older build refuses the new format instead of
  misreading it. The fourth was declined with reasons.
- A code contribution, [pull request #1629](https://github.com/rbitcoin-org/rbitcoin/pull/1629),
  merged by the maintainer on 14 November 2026: the RPC server's "at max
  connections" log line is now written at most once a second, so a flood of
  connections past the cap no longer adds one line each.

**electrs**: four ordinary bugs filed as public issues (not security issues under its policy).

## Private reports (counted, not named)

<!-- counts:start (generated by update-security-reviews.py; edit its data file, not this block) -->
- As of 14 November 2026, 32 other open-source Bitcoin projects had
  received private reports (October and November 2026), from critical signing and
  seed-loss paths to low-severity hardening.
<!-- counts:end -->
- One project declined two findings as outside its trust model.
- The rest are in triage or have not replied. They will be named here once
  each project publishes or the embargo ends.

## Accuracy

Of the 81 public Faraday audit findings, about 7 in 10 led to a fix and 1 in
10 did not hold. The maintainer also triaged 91 lower-confidence items we
flagged separately: 55 led to fixes and 30 did not hold. We publish these
numbers because a review is only useful if its findings are real.

Checking costs us findings, and we accept that. In one November audit the
models' cross-checks agreed on 86 findings, one of them rated critical; read
against the source, the critical did not hold and went out as a low-severity
note.

## Our own project

[EntropyLab](https://github.com/OogaBoogaX/entropylab), our offline Bitcoin
key tool, gets a multi-model review and a red-team pass on every change.
Changes that handle keys get further independent reviews. Unlike our reviews
of other projects, work on our own code includes running it: the test suite,
mutation runs, and a harness that captures what secrets a browser leaves in
memory. The rounds and the fixes they produced are logged in
[entropylab-red-team.md](entropylab-red-team.md). EntropyLab's pull requests
are public and name the models that worked on each one.

## How to know a report is from us

Our reports come from these accounts:

- email: [entropylab-security@proton.me](mailto:entropylab-security@proton.me), signed MrHodl;
- GitHub: [MrHodlX](https://github.com/MrHodlX), through a project's private vulnerability reporting;
- X: [@MrHodl](https://x.com/MrHodl), which will confirm any report on request.

Every report we send names the commit we reviewed and links each finding to a file and line. It never:

- includes exploit code, crafted files or anything to run;
- asks you to move funds, share a seed phrase or keys, install software, or open a link to run something;
- comes with a request for payment. A bounty, if a project offers one, is its own choice, made after the fix.

If a message claims to be from us and any of this doesn't match, don't act on it. Ask [@MrHodl](https://x.com/MrHodl) on X to confirm.

## Contact

To ask for a review, or about one of ours, email [entropylab-security@proton.me](mailto:entropylab-security@proton.me).
