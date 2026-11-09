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

**Checkpoint 195 · November 8, 2026.** Read the [current status](docs/STATUS.md) and [roadmap](ROADMAP.md) for evidence and remaining work.

The local Windows diagnostic includes 560 original-game C++ translation files and 73 explicitly registered relocated-code addresses. All 128 addresses in the checked initializer table are linked. Run-183 reaches the final callback and returns past the original initializer walker; runs 185-195 continue through startup, and run-195 reaches a native SIF RPC-init wait at `0x3cc9a0`/`0x3cc9a8`: the guest RPC-init request arrives in IOP memory, but the IOP RPC bridge ignores raw SIF transfers, so no reply arrives and the run stops at the five-second watchdog. These counts describe the diagnostic subset, not a percentage of a finished port or an audit of every callback.

The latest 11 bounded startup runs (185-195) compile and execute past their previous missing call. Run-195 is a service stall, not another missing translation: the SIF RPC-init request is delivered but unanswered. All runs preserve input hashes and three startup code copies. A batching-tool fix now discovers missing dependencies two calls deep when that depth is requested.

Independent native checks cover ISO filename lookup and exact boot-file reads, copied-kernel selectors, thread policy, a corrected EntryHi instruction translation, and one differential helper. Their passing scope is detailed in [status](docs/STATUS.md).

## Progress chart

| Workstream | State | Evidence / limit |
| --- | --- | --- |
| Input preservation | Locally checked | ISO and extracted executable hashes are checked before/after startup tests |
| Function mapping and translation | In progress | 560 C++ files in the current native subset; other generated material stays local |
| Initializer linkage | Locally checked | 128/128 callback addresses linked; walker return observed; individual callback correctness unverified |
| Strict startup diagnostics | Locally checked | Missing code stops explicitly instead of returning fabricated success |
| SIF RPC initialization | Stall identified | Run-195 waits on RPC-init; request arrives but the IOP bridge ignores raw SIF transfers; a bounded native SIF command endpoint is the next step |
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
