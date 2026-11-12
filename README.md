![Source](https://img.shields.io/badge/Source-PlayStation_2-003791)
![Target](https://img.shields.io/badge/Target-Windows-0078D4)
![Development](https://img.shields.io/badge/Development-Native_port_research-orange)
![Gameplay](https://img.shields.io/badge/Gameplay-Not_playable_yet-lightgrey)
![Controller](https://img.shields.io/badge/Controller_support-Planned-lightgrey)
![License](https://img.shields.io/badge/License-GPL--3.0-blue)
![Credit](https://img.shields.io/badge/Credit-Saber-brightgreen)

# Manhunt 2 Native PC

A research project working toward a native Windows build from an owner-provided PlayStation 2 copy of **Manhunt 2**, with controller support as an eventual goal. **Credit to Saber.**

**Early development. There is no playable PC build or downloadable game here.** Current work translates and tests bounded parts of the original startup code. The goal is native Windows execution; the current result is a CPU startup diagnostic.

---

## Contents

- [Current development](#current-development)
- [Progress chart](#progress-chart)
- [Project scope](#project-scope)
- [Roadmap](#roadmap)
- [Downloads](#downloads)
- [Repository layout](#repository-layout)
- [Credits and license](#credits-and-license)

## Current development

## November 12, 2026 — checkpoint 305 and bounded sound research

**Proven:** The latest native startup run is 305: exit 2 in 0.406 seconds at PC `0x3CC5AC`, stopping on `unsupported-sndscrm-call`. The diagnostic has 788 generated translation files, 4,535 original dispatch entries and 73 relocated addresses. These counts are a bounded subset, not percent completion. ELF/ISO hashes remained unchanged and all three copied-code spans matched. Five native regressions (kernel, kernel2, thread, leaf, EntryHi-after) passed with zero mismatches.

**What this means:** Startup reaches Rockstar's SNDSCRM sound service, sid `0xBCF7`. The first unsupported record is type 1, ID `0x100A`, three zero payload words; the complete CALL sends 108 bytes and requests a 1,472-byte reply. Dispatch uses the record ID, not RPC fno. The corrected parser recognizes six records. No new command implementation or successful sound response was added.

**Corrections to historical notes:** CDVD SCMD sid `0x80000593` is a control service, not the sector-read service. NCMD sid `0x80000595`, fno 1, is the sector-read path. Earlier run-198 READ/file-loading wording below is superseded; that checkpoint did not establish an actual file read. For this USA executable, `0x100A` maps through table target `0x1F80` to helper `0x1614`, not `0x1660` (used by `0x100C`). Its zero input scales to **1024**, not the 109 mistakenly stated in an early local note.

**Bounded evidence:** The original gain slice matched an independent signed-integer reference in 65,545 cases, with zero mismatches. Twenty-nine original-setter fixture cases and six missing-effect refusals passed. A voice initializer executed in 2,081 diagnostic-interpreter steps; nine setter replays passed. Ten deterministic Scream worker/timer wakeups preserved inactive voice state. Eight selected follow-on Scream calls and ten SNDSCRM libsd-helper trace cases completed in fixtures. These are not hardware-validated IOP results: imports, clocks and scheduling use explicit test doubles; module relocation, general load-delay timing and real audio effects remain unverified.

**Harness correction:** Two SNDSCRM checks initially copied trailing ELF section/relocation data into BSS. Successor checks load only PT_LOAD (file size 14,600; memory size 30,536) and zero-fill BSS. All ten helper cases and three worker-branch results remained unchanged. Prior receipts are preserved as superseded evidence, not silently erased.

**Unknown / next:** The full live state across both sound workers and libsd reset/transfer-handler effects. The queued worker path reaches sysmem ordinal 7, `QueryMaxFreeMemSize`; it must use represented allocator state. Independently validate interpreter semantics and required IOP effects before enabling `0x100A`. Next probe run is 306. No complete boot, rendered gameplay, audible sound or controller result is claimed.

Evidence remains local under `startup-probe-001/run-305/` and `resume-305/` (gain, execution, initializer, scheduler, follow-on, corrected libsd-init and worker receipts). Only this curated summary is published; game binaries, generated game C++, copied module bytes, commercial SDKs and local compiler installations remain excluded. No gameplay screenshot exists to publish.

Read [status and preserved history](docs/STATUS.md).

## Progress chart

| Workstream | State | Evidence / limit |
| --- | --- | --- |
| Input preservation | Locally checked | ISO and extracted executable hashes are checked before/after startup tests |
| Function mapping and translation | In progress | 788 C++ files in the current native subset; other generated material stays local |
| Initializer linkage | Locally checked | 128/128 callback addresses linked; walker return observed; individual callback correctness unverified |
| Strict startup diagnostics | Locally checked | Missing code stops explicitly instead of returning fabricated success |
| SIF RPC and sound startup | Partial local coverage | Run 305 stops explicitly on SNDSCRM command 0x100A; actual sector reads and complete sound compatibility remain unverified |
| Hand decompilation (parallel track) | In progress | All 102 distinct handlers of the main entity dispatcher lifted to C (141 types), about 4,100 lines across 40 files in a separate local project; that code is not published here |
| ISO file access | Narrow native test passed | Boot-file lookup and all 3,703,880 bytes match the independent extracted file |
| Kernel compatibility | Partial local coverage | Selector, thread and instruction tests; complete hardware behavior unverified |
| Rendering and audio | Not established | No successful native game window or audio output |
| Xbox controller support | Planned | No integrated controller result yet |
| Playable native game | Not available | No gameplay or complete boot claim |

A checked row means only the stated local condition passed. It does not imply a working game.

## Project scope

This public repository shares project status, milestones and references. Development tools and handoff notes are maintained separately. Game images, extracted executables, generated game code, commercial SDKs and copied game assets are not distributed here.

## Roadmap

1. Validate initializer effects and continue beyond the now-observed walker return.
2. Resolve reached runtime service and instruction-semantics failures with independent checks.
3. Establish actual game asset loading from the owner's local image.
4. Implement and verify the rendering and audio paths needed by the game.
5. Integrate controller input and test gameplay.

There is no reliable completion date. See [ROADMAP.md](ROADMAP.md).

## Downloads

**No playable release is available.** This repository does not contain a PC game executable or a game download. A future release will describe exactly what was tested and what owner-supplied data it needs.

## Repository layout

```text
docs/STATUS.md          Current evidence and limitations
ROADMAP.md             Development milestones
CHANGELOG.md           Published project updates
REFERENCES.md          Public technical references
THIRD_PARTY_NOTICES.md  Credits and licensing boundaries
.github/               Bug-report form
```

## Credits and license

**Saber** — project direction, integration and testing.

[PS2Recomp](https://github.com/ran-j/PS2Recomp) is the underlying open-source recompilation/runtime project used by the local investigation. Exact provenance and license boundaries are in [Third-party notices](THIRD_PARTY_NOTICES.md). The README layout takes inspiration from the owner's Xbox 360 achievement project.

Original material in this repository is under [GNU GPL version 3](LICENSE). This license does not grant rights to Manhunt 2 code or assets. This is an independent project with no Rockstar Games or Sony affiliation.
