# Security reviews of open-source Bitcoin software

By MrHodl ([@MrHodl](https://x.com/MrHodl) on X). Every finding named below
is public or fixed; reports still under embargo are counted, not named.
Paid reviews: [mrhodlx.github.io](https://mrhodlx.github.io/).

## What I do

I review open-source Bitcoin software: wallets, signers, PSBT tools and
servers. The work is done by AI models. Several read each codebase
independently and raise claims. Before anything is reported, every claim is
checked against the code, the relevant BIP or the project's own test vectors,
and traced through the code paths. The reviews read the code; nothing is
built or run, and the reports say so. Most of what the models raise does not
survive that checking, and that filtering is what maintainers get: fewer,
real findings. I look hardest at what other reviews skip: crafted inputs
(files, QR codes, PSBTs) that lie about themselves, and code that should fail
closed but does not. I report privately first, through each project's
security policy, and never test live systems.

## How findings are checked

As of November 2026:

- Several open-weight models review the code, each on its own. Every claim is
  then cross-checked by models that did not make it.
- Every claim that goes into a report is then traced through the source with a
  frontier model. Anything rated medium or above is read a second time,
  independently, by a model that is released only to vetted security
  researchers.
- Models do the reading at every step. Model output alone is never a finding:
  a claim is reported only after that tracing. Each report says how it was
  made, and every report goes out under my name.

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
  rest, one was already fixed before the reviewed commit (my miss), and
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

**p2pool-lab** (EthnTuttle/p2pool-lab, a testnet lab for rbitcoin and P2Pool with a miner dashboard), November 2026
- A private report on 18 November 2026 with three low-severity hardening
  notes: status polls followed HTTP redirects, one wrongly typed field from a
  miner stopped every miner's collection cycle, and the device MAC check
  passed when the MAC was missing. The maintainer reproduced all three and
  fixed them the same day in
  [74513cf](https://github.com/EthnTuttle/p2pool-lab/commit/74513cf4b1a06de3d815852aadce3ce0eb135107),
  with regression tests.

**ArkPool gateway** (arkpool/arkpool-gateway on GitLab, the open-source home gateway that connects Bitaxe miners to ArkPool over Tor), November 2026
- A private report on 18 November 2026 with six low-severity hardening
  notes: two LAN addresses could hold every mining slot, a slow client could
  hold the status server, one slow miner stalled the telemetry cycle, a Tor
  route that dropped streams kept being chosen, an optional pin of each miner
  to its payout username, and a documentation line on collector credentials.
  The team fixed all six within about an hour, among them
  [ffa31d85](https://gitlab.com/arkpool/arkpool-gateway/-/commit/ffa31d852faada613ae87a9ef67648d3a2d07db6) (slots),
  [8ee72f11](https://gitlab.com/arkpool/arkpool-gateway/-/commit/8ee72f11f2a478d97cb49a970088f073fa5891ed) (status server) and
  [e8984bd1](https://gitlab.com/arkpool/arkpool-gateway/-/commit/e8984bd14d71e3caafd83a5cd8c0d9835e5d94df) (username pins), recorded in
  [80b0abc0](https://gitlab.com/arkpool/arkpool-gateway/-/commit/80b0abc0087fdf92216ea46bbe9cac3ba2946a4e), and thanked me publicly.

**ckpool** (ckolivas/ckpool on Bitbucket, the pool software behind solo.ckpool.org), November 2026
- A private report on 17 November 2026 to Con Kolivas: five findings rated
  medium, plus lower-severity notes. Within two days he fixed four of the five
  mediums and two of the notes. I re-checked the fixes on 19 November: a solved
  block is now resubmitted to every configured bitcoind instead of being
  dropped when the node is unreachable, the connector keeps accepting miners
  through file-descriptor or memory errors, solo usernames that are not
  addresses no longer pile up, and an SV2 custom-job block solve is checked
  even with the upstream down (commits 227c156, 72aff21, bce5fc5, a78c5cc).
  The remaining item stays private until it is fixed.

**Bark** (ark-bitcoin/bark, Second's Ark wallet and server), November 2026
- A private report on 13 November 2026 to Second's security team: two
  findings rated high and four rated medium, plus lower-severity notes. They
  confirmed it, fixed both highs and two of the mediums within a week, and
  credited the report in their release notes. I re-checked the fixes on 19
  November: new round VTXOs must now be locked by the wallet's own unlock
  hash, the new VTXOs are stored before forfeits are sent, an unanswered
  Lightning payment attempt waits for xpay's own retry window, and repeated
  HTLC-receive VTXOs are rejected (commits c5798125, abf74aac, 241ac71f,
  a2259083). The remaining items stay private until they are fixed.

**BitBoxApp** (BitBoxSwiss/bitbox-wallet-app), November 2026
- A private report on 18 November 2026, sent PGP-encrypted to Shift Crypto's
  security team. They confirmed it, had already been tracking it internally,
  and classify it as hardening (no bounty). Listed here as "reported,
  acknowledged, tracked as hardening" with their permission; no technical
  details until they ship a change.

**electrs**: four ordinary bugs filed as public issues (not security issues under its policy).

## Private reports (counted, not named)

<!-- counts:start (generated by update-security-reviews.py; edit its data file, not this block) -->
- As of 19 November 2026, 41 other open-source Bitcoin projects had
  received private reports (October and November 2026), from critical signing and
  seed-loss paths to low-severity hardening.
<!-- counts:end -->
- One project declined two findings as outside its trust model.
- The rest are in triage or have not replied. They will be named here once
  each project publishes or the embargo ends.

## Accuracy

Of the 81 public Faraday audit findings, about 7 in 10 led to a fix and 1 in
10 did not hold. The maintainer also triaged 91 lower-confidence items I
flagged separately: 55 led to fixes and 30 did not hold. I publish these
numbers because a review is only useful if its findings are real.

Checking costs findings, and I accept that. In one November audit the
models' cross-checks agreed on 86 findings, one of them rated critical; read
against the source, the critical did not hold and went out as a low-severity
note.

## EntropyLab

[EntropyLab](https://github.com/OogaBoogaX/entropylab), the offline Bitcoin
key tool I lead, gets a multi-model review and a red-team pass on every
change. Changes that handle keys get further independent reviews. Unlike the
reviews of other projects, work on EntropyLab includes running it: the test
suite, mutation runs, and a harness that captures what secrets a browser
leaves in memory. The rounds and the fixes they produced are logged in
[entropylab-red-team.md](entropylab-red-team.md). EntropyLab's pull requests
are public and name the models that worked on each one.

## How to know a report is from me

My reports come from these accounts:

- email: [misterhodl21@proton.me](mailto:misterhodl21@proton.me), signed MrHodl (reports sent before 16 November 2026 came from entropylab-security@proton.me);
- GitHub: [MrHodlX](https://github.com/MrHodlX), through a project's private vulnerability reporting;
- X: [@MrHodl](https://x.com/MrHodl), which will confirm any report on request.

Every report I send names the commit reviewed and links each finding to a file and line. It never:

- includes exploit code, crafted files or anything to run;
- asks you to move funds, share a seed phrase or keys, install software, or open a link to run something;
- comes with a request for payment. A bounty, if a project offers one, is its own choice, made after the fix.

If a message claims to be from me and any of this doesn't match, don't act on it. Ask [@MrHodl](https://x.com/MrHodl) on X to confirm.

## Contact

To ask for a review, or about one of mine, email [misterhodl21@proton.me](mailto:misterhodl21@proton.me) or message mrhodl.1952 on [Signal](https://signal.me/#eu/TDoFVpNILKkCUvzndeku5ASssFN6LdaypDAljWSfMndT-S1Jm2BOeqnT5Z_FWred).
