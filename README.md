# ArgusAgent — Releases

**Deterministic, zero-token repository audit and ship-readiness verdicts.**

This repository distributes **release builds** of ArgusAgent. The source is not published here.

[**⬇ Download the latest beta**](https://github.com/XAgents-ai/argus-agent-releases/releases/latest)

---

## Beta

This is a pre-release. Accuracy tuning is ongoing, and **reporting a wrong finding is the most
useful thing you can do with it** — see [Reporting a wrong finding](#reporting-a-wrong-finding).

Not for production use, and not a substitute for review. See `LICENSE.txt`.

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

Download the package for your platform from
[Releases](https://github.com/XAgents-ai/argus-agent-releases/releases/latest), extract it, and
run the executable. **No Python, pip, git or GitHub account required.**

| Platform | Package |
|---|---|
| Windows x64 | `argus-agent-0.1.0-beta-windows-x64.zip` |
| macOS (Apple silicon) | `argus-agent-0.1.0-beta-macos-arm64.zip` |
| Linux x64 | `argus-agent-0.1.0-beta-linux-x64.zip` |

```powershell
# Windows
.\argus.exe audit C:\path\to\my-project
```

```bash
# macOS / Linux
chmod +x argus
./argus audit /path/to/my-project
```

Each package carries its own `SHA256SUMS.txt`. The executables are unsigned, so Windows
SmartScreen and macOS Gatekeeper will warn on first run.

All three are built and smoke-tested by a CI matrix — each binary must start and complete a real
audit to a verdict before it is packaged.

Full instructions are in `QUICKSTART.md` inside the archive.

---

## Known limits, stated up front

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

**Proprietary — for now.** Released under a Beta Evaluation Licence: free to use and free to
**share on** (pass the complete unmodified package to colleagues or classmates; don't charge for
it, don't break it up, don't reverse engineer it, don't rely on it in production). Full terms in
`LICENSE.txt`.

**We intend to release ArgusAgent as open source under the MIT Licence once the precision
validation is complete.** That is a statement of intent rather than a commitment, and it grants
no MIT rights today.

Bundled third-party open-source components remain under their own licences — see
`THIRD-PARTY-NOTICES.txt`.

© 2026 XAgents. All rights reserved.
