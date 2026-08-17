# ArgusAgent — Releases

**Deterministic, zero-token repository audit and ship-readiness verdicts.**

This repository distributes **release builds** of ArgusAgent. The source is not published here.

[**⬇ Download the latest beta**](https://github.com/XAgents-ai/argus-agent-releases/releases/latest)

---

## ⚠️ Beta — read this before you weigh any verdict

> Instrument status: Argus's own finding precision has not been independently validated. Its findings rest on the Argus dogfood corpus, a self-audit of this repository with no human true-positive/false-positive adjudication behind it. This notice is removed only when Epic 13's human adjudication clears the >=80% precision gate; nothing else removes it.

ArgusAgent prints that notice on every run.

Stated plainly: **we have not yet proven how often Argus is right.** On 2026-08-17 we ran the
first human adjudication of its blocking findings across a five-repository validation corpus. Of
31 blocking findings, **0 were judged true positives**, 26 were false positives and 5 were
unresolved. That did not clear our own ≥80% precision gate, so the notice stays on and the gate
stays shut.

We are publishing that result rather than hiding it, because a tool that tells you your tests
are bad had better be able to say how often it is wrong.

**Do not treat Argus output as assurance, certification, or a substitute for review.**

---

## What it does

ArgusAgent audits a repository and answers two questions together: *are there blocking
problems*, and *was enough of the code actually examined to say so*.

- **Deterministic verdict gate** — no language model reads your source on the default path. The
  same input gives the same verdict.
- **Three outcomes, not two** — `RELEASE_READY`, `NOT_READY_FOR_RELEASE`, and
  `INSUFFICIENT_COVERAGE`. The third is the point: a tool that cannot examine your code should
  say so rather than award a pass.
- **Ten languages ground out of the box** — Python, JavaScript, TypeScript, Go, Rust, Java, C,
  C++, Ruby, PHP.
- **Local only** — your source is never uploaded.

| Verdict | Exit code |
|---|---|
| `RELEASE_READY` | `0` |
| `NOT_READY_FOR_RELEASE` | `1` |
| `INSUFFICIENT_COVERAGE` | `3` |

---

## Install

Download the archive from [Releases](https://github.com/XAgents-ai/argus-agent-releases/releases/latest),
extract it, and run the executable. **No Python, pip, git or GitHub account required.**

```powershell
.\argus.exe audit C:\path\to\my-project
```

Verify your download against the published checksum:

```powershell
Get-FileHash .\argus.exe -Algorithm SHA256
```

The executable is unsigned, so Windows SmartScreen may warn on first run.

**Platform:** the current beta is **Windows x64 only**. macOS and Linux builds are not yet
published.

Full instructions are in `QUICKSTART.md` inside the archive.

---

## Known limits, stated up front

- **Accuracy is unproven** — see the notice above. This is the headline limitation.
- **`--coverage-scope repository`** enables a vacuous-test detector whose measured precision was
  poor. Expect false positives if you use it. It does **not** run on the default scope.
- **C, C++, Ruby and Rust** parse and are graded but currently yield no function/class
  definitions, so those files cannot reach the deep grade. They still count toward coverage.

---

## Reporting a wrong finding

**This is the most useful thing you can do with the beta.** If Argus flags something you believe
is fine, tell us — that report is the evidence that improves the gate.

Open an issue on this repository with the rule id, the file and line Argus printed, your call
(true positive / false positive / borderline) and a sentence on why. `FEEDBACK.md` in the archive
has the full template.

Never send code you are not free to share. Argus itself transmits nothing.

---

## Licence

**Proprietary. This is not open-source software.** Released under a Beta Evaluation Licence:
free to use for evaluation, no redistribution, no reverse engineering, no production use. The
full terms are in `LICENSE.txt` inside the archive.

Bundled third-party open-source components remain under their own licences — see
`THIRD-PARTY-NOTICES.txt`.

© 2026 XAgents. All rights reserved.
