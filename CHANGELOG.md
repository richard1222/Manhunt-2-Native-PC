## November 12, 2026 — checkpoint 305 and bounded sound research

**Proven:** The latest native startup run is 305: exit 2 in 0.406 seconds at PC `0x3CC5AC`, stopping on `unsupported-sndscrm-call`. The diagnostic has 788 generated translation files, 4,535 original dispatch entries and 73 relocated addresses. These counts are a bounded subset, not percent completion. ELF/ISO hashes remained unchanged and all three copied-code spans matched. Five native regressions (kernel, kernel2, thread, leaf, EntryHi-after) passed with zero mismatches.

**What this means:** Startup reaches Rockstar's SNDSCRM sound service, sid `0xBCF7`. The first unsupported record is type 1, ID `0x100A`, three zero payload words; the complete CALL sends 108 bytes and requests a 1,472-byte reply. Dispatch uses the record ID, not RPC fno. The corrected parser recognizes six records. No new command implementation or successful sound response was added.

**Corrections to historical notes:** CDVD SCMD sid `0x80000593` is a control service, not the sector-read service. NCMD sid `0x80000595`, fno 1, is the sector-read path. Earlier run-198 READ/file-loading wording below is superseded; that checkpoint did not establish an actual file read. For this USA executable, `0x100A` maps through table target `0x1F80` to helper `0x1614`, not `0x1660` (used by `0x100C`). Its zero input scales to **1024**, not the 109 mistakenly stated in an early local note.

**Bounded evidence:** The original gain slice matched an independent signed-integer reference in 65,545 cases, with zero mismatches. Twenty-nine original-setter fixture cases and six missing-effect refusals passed. A voice initializer executed in 2,081 diagnostic-interpreter steps; nine setter replays passed. Ten deterministic Scream worker/timer wakeups preserved inactive voice state. Eight selected follow-on Scream calls and ten SNDSCRM libsd-helper trace cases completed in fixtures. These are not hardware-validated IOP results: imports, clocks and scheduling use explicit test doubles; module relocation, general load-delay timing and real audio effects remain unverified.

**Harness correction:** Two SNDSCRM checks initially copied trailing ELF section/relocation data into BSS. Successor checks load only PT_LOAD (file size 14,600; memory size 30,536) and zero-fill BSS. All ten helper cases and three worker-branch results remained unchanged. Prior receipts are preserved as superseded evidence, not silently erased.

**Unknown / next:** The full live state across both sound workers and libsd reset/transfer-handler effects. The queued worker path reaches sysmem ordinal 7, `QueryMaxFreeMemSize`; it must use represented allocator state. Independently validate interpreter semantics and required IOP effects before enabling `0x100A`. Next probe run is 306. No complete boot, rendered gameplay, audible sound or controller result is claimed.

Evidence remains local under `startup-probe-001/run-305/` and `resume-305/` (gain, execution, initializer, scheduler, follow-on, corrected libsd-init and worker receipts). Only this curated summary is published; game binaries, generated game C++, copied module bytes, commercial SDKs and local compiler installations remain excluded. No gameplay screenshot exists to publish.

---

# Project updates

## SIF RPC-init stall resolved - November 9, 2026

**Proven:** Run-198 resolves the native SIF RPC-init stall that stopped runs 194-197. The guest SIF handler at `0x3cc0c0` was linked into the startup probe; the native SIF reply was delivered through the game's own handler, the guest flag at `0x661ec0` moved to 1, and the run exits 2 in 0.281 seconds. Startup now reaches CDVD file loading and stops at a cdvdfsv READ at `0x3cce90`. The diagnostic subset now contains 561 original C++ files and 2,331 dispatch addresses.

**Unknown:** cdvdfsv service coverage, subsequent RPC bind/call coverage, rendering, audio, controller support and gameplay. No playable release is published. The next step is linking `0x3cce90` and assessing the cdvdfsv service. See [current status](docs/STATUS.md).

---

## SIF RPC-init stall identified - November 8, 2026

**Proven:** Runs 185-195 advance startup past the initializer walker. Run-195 reaches a native SIF RPC-init wait at `0x3cc9a0`/`0x3cc9a8`: the guest RPC-init request `[16, 0, 0x80000002, 1]` arrives in IOP memory, but the IOP RPC bridge ignores raw SIF transfers, so no reply arrives and the run stops at the five-second watchdog (exit 4). The diagnostic subset now contains 560 original C++ files and 2,330 dispatch addresses.

**Unknown:** Reply delivery and interrupt ordering, subsequent RPC bind/call coverage, rendering, audio, controller support and gameplay. No playable release is published. The next step is a bounded native SIF command endpoint with a correctly routed reply. See [current status](docs/STATUS.md).

---

## Initializer return observed - November 8, 2026

**Proven:** Run-183 returns past the original initializer walker, and run-184 progresses into subsequent startup routines. Fourteen additional local runs preserve inputs and startup copies. The diagnostic subset now contains 534 original C++ files. Bounded caller grouping is tested; an overlarge plan was refused and replaced with a smaller plan while preserving the failed result.

**Unknown:** Individual initializer correctness, complete boot, graphics, audio, controller support and gameplay. No playable release is published. The next step is the newly observed missing startup dependency. See [current status](docs/STATUS.md).

---

## Initial project page - November 8, 2026

**Proven:** Public project documentation now records checkpoint 170, scoped native test results, the initializer-planning correction and remaining work. The README uses a badge row, contents links and progress table inspired by the owner's Xbox 360 achievement project.

**Unknown:** Native gameplay, completed startup, graphics, audio and controller support. No playable release is published.
