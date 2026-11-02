# Security reviews of open-source Bitcoin software

By MrHodl ([@MrHodl](https://x.com/MrHodl) on X). Every finding named below
is public or fixed; reports still under embargo are counted, not named.

## What we do

We review open-source Bitcoin software: wallets, signers, PSBT tools and
servers. Several AI models attack each codebase independently, then every
claim is checked against the code, the relevant BIP or the project's own test
vectors, and reproduced, before anything is reported. Most of what models
produce does not survive that step, and that filtering is what maintainers get
from us: fewer, real findings. We look hardest at what other reviews skip:
crafted inputs (files, QR codes, PSBTs) that lie about themselves, and code
that should fail closed but does not. We report privately first, through each
project's security policy, and never test live systems.

## Public results

**Faraday** (bitcoinpasada/faraday), October 2026
- Three published security advisories, all fixed in v0.1.8:
  - [GHSA-7h4m-p8qx-pxrf](https://github.com/bitcoinpasada/faraday/security/advisories/GHSA-7h4m-p8qx-pxrf) (critical): five critical/high findings, including FROST nonce handling and the multisig change check.
  - [GHSA-w52w-c7x4-8pw5](https://github.com/bitcoinpasada/faraday/security/advisories/GHSA-w52w-c7x4-8pw5) (high): secret QR frames not wiped.
  - [GHSA-vxpw-8g7c-8qmr](https://github.com/bitcoinpasada/faraday/security/advisories/GHSA-vxpw-8g7c-8qmr) (high): finalized inputs bypassed the unverified-amount guard.
- Two public audits of later commits (issues #4 and #5), 81 medium/low findings (some overlap between the two). The maintainer's own verdicts: 56 fixed, 9 by design, 8 did not hold, 8 moot (code removed).

**VoidForge** (Random21m/voidforge), October 2026
- [GHSA-8xcw-qx5p-8rmr](https://github.com/Random21m/voidforge/security/advisories/GHSA-8xcw-qx5p-8rmr): the release workflow could attest a tampered build. Confirmed and fixed the same day (1.8.1); the maintainer rated it medium.
- A second, private report (November 2026) is in triage.

**ZEUS** (Lightning wallet), October 2026
- A private code and security review. ZEUS credited it publicly in its
  v13.2.4 release post on 29 October 2026 ("code and security scans",
  @MrHodl).

**electrs**: four ordinary bugs filed as public issues (not security issues under its policy).

## Private reports (counted, not named)

- Fourteen other open-source Bitcoin projects received private reports in
  October and November 2026, from critical signing and seed-loss paths to
  low-severity hardening.
- One project confirmed a medium finding and fixed it within two days; one
  finding there was already fixed before the commit we reviewed (our miss),
  and three were left unchanged as informational or by design.
- One project declined two findings as outside its trust model.
- The rest are in triage or have not replied. They will be named here once
  each project publishes or the embargo ends.

## Accuracy, honestly

Of the 81 public Faraday audit findings, about 7 in 10 led to a fix and 1 in
10 did not hold. The maintainer also triaged 91 lower-confidence items we
flagged separately: 55 led to fixes and 30 did not hold. We publish these
numbers because a review is only useful if its findings are real.

Checking costs us findings, and we accept that. In one November audit the
models' cross-checks agreed on 86 findings, one of them rated critical. Read
against the source, the critical did not hold (the coins it said could be
signed away are never handed to the signer), so it went out as a
low-severity hardening note; the report carried four medium findings and two
low notes.

## Our own project

EntropyLab, an offline Bitcoin key tool, gets the same process on every
change: six or more models review it, and a fix only counts when deliberately
breaking it makes a test fail.

## Contact

To ask for a review, or about one of ours, email [entropylab-security@proton.me](mailto:entropylab-security@proton.me).
