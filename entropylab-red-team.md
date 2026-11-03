# EntropyLab red-team log

[EntropyLab](https://github.com/OogaBoogaX/entropylab) is our own offline
Bitcoin key tool. It gets the same model review as the projects we review,
plus tests we can only run on our own code: mutation runs, and a harness that
captures what secrets a browser leaves in memory. This page lists the rounds
and the fixes they produced. Only merged, public fixes are listed; findings
still being fixed stay private until they merge.

## On every change

Each pull request is reviewed by five models (DeepSeek V4.1, Kimi K3, GLM 5.3,
MiniMax M3.1 Flash and Nemotron 3.5 Lightning) plus a red-team pass that tries
to break it. A merge needs a clean approval from at least one of DeepSeek, GLM
or Kimi, and every finding has to be fixed or refuted against the code first.
Changes that handle keys, signing or secrets also get a second red team by
Kimi K3.1, plus a Codex or Grok review.

## Rounds

**7 September 2026: first red-team pass**
- Five findings, all closed in [#390](https://github.com/OogaBoogaX/entropylab/pull/390).

**4 to 6 October 2026: memory residue**
- Measured what secrets a browser leaves in memory after use, with a capture
  harness. Fixes: [#711](https://github.com/OogaBoogaX/entropylab/pull/711)
  (secrets kept away from translation, text services and the screen),
  [#734](https://github.com/OogaBoogaX/entropylab/pull/734) (passphrases kept
  out of their fields); harness:
  [#805](https://github.com/OogaBoogaX/entropylab/pull/805).

**11 October 2026: adversarial review (GPT-6)**
- Recorded in the repository as
  [docs/Adversarial_Audit.md](https://github.com/OogaBoogaX/entropylab/blob/677ac87144838e57869d8686496bc7cae25c33e9/docs/Adversarial_Audit.md).

**14 and 21 October 2026: mutation testing (Stryker)**
- Mutants the tests missed; each PR adds the test that catches it:
  [#864](https://github.com/OogaBoogaX/entropylab/pull/864),
  [#868](https://github.com/OogaBoogaX/entropylab/pull/868),
  [#1032](https://github.com/OogaBoogaX/entropylab/pull/1032),
  [#1033](https://github.com/OogaBoogaX/entropylab/pull/1033).

**18 October 2026: adversarial audit (Kimi K3), eight passes**
- Fixes merged on 19 and 20 October:
  [#976](https://github.com/OogaBoogaX/entropylab/pull/976),
  [#980](https://github.com/OogaBoogaX/entropylab/pull/980),
  [#981](https://github.com/OogaBoogaX/entropylab/pull/981),
  [#982](https://github.com/OogaBoogaX/entropylab/pull/982),
  [#984](https://github.com/OogaBoogaX/entropylab/pull/984),
  [#985](https://github.com/OogaBoogaX/entropylab/pull/985),
  [#986](https://github.com/OogaBoogaX/entropylab/pull/986),
  [#987](https://github.com/OogaBoogaX/entropylab/pull/987),
  [#991](https://github.com/OogaBoogaX/entropylab/pull/991),
  [#992](https://github.com/OogaBoogaX/entropylab/pull/992),
  [#993](https://github.com/OogaBoogaX/entropylab/pull/993),
  [#994](https://github.com/OogaBoogaX/entropylab/pull/994),
  and the remediation PR
  [#1001](https://github.com/OogaBoogaX/entropylab/pull/1001).

**21 and 22 October 2026: deep sweep, every source file**
- Each file audited on its own by ten model variants (DeepSeek V4.1, Kimi K3,
  GLM 5.3, Nemotron 3 and Muse Spark families), then cross-checked by other
  models. Fixes merged on 22 and 23 October:
  [#1057](https://github.com/OogaBoogaX/entropylab/pull/1057),
  [#1058](https://github.com/OogaBoogaX/entropylab/pull/1058),
  [#1062](https://github.com/OogaBoogaX/entropylab/pull/1062),
  [#1063](https://github.com/OogaBoogaX/entropylab/pull/1063),
  [#1064](https://github.com/OogaBoogaX/entropylab/pull/1064).
- A two-stage model audit of the translations corrected 39 mistranslations:
  [#1066](https://github.com/OogaBoogaX/entropylab/pull/1066).

**24, 25 and 27 October 2026: delta sweeps**
- The same per-file sweep over only the files changed since the previous one,
  by Kimi K3, GLM 5.3, DeepSeek V4.1 and Nemotron 3 Super; on 25 October a
  second full pass added MiniMax M3.1 Flash. Findings triaged; not tied to
  specific PRs in our logs.

**3 November 2026: delta sweep (running)**
- By Kimi K3, GLM 5.3, DeepSeek V4.1 and Nemotron 3 Super. Fixes will be
  listed here once they merge.
