## November 12, 2026 — checkpoint 305 and bounded sound research

**Proven:** The latest native startup run is 305: exit 2 in 0.406 seconds at PC `0x3CC5AC`, stopping on `unsupported-sndscrm-call`. The diagnostic has 788 generated translation files, 4,535 original dispatch entries and 73 relocated addresses. These counts are a bounded subset, not percent completion. ELF/ISO hashes remained unchanged and all three copied-code spans matched. Five native regressions (kernel, kernel2, thread, leaf, EntryHi-after) passed with zero mismatches.

**What this means:** Startup reaches Rockstar's SNDSCRM sound service, sid `0xBCF7`. The first unsupported record is type 1, ID `0x100A`, three zero payload words; the complete CALL sends 108 bytes and requests a 1,472-byte reply. Dispatch uses the record ID, not RPC fno. The corrected parser recognizes six records. No new command implementation or successful sound response was added.

**Corrections to historical notes:** CDVD SCMD sid `0x80000593` is a control service, not the sector-read service. NCMD sid `0x80000595`, fno 1, is the sector-read path. Earlier run-198 READ/file-loading wording below is superseded; that checkpoint did not establish an actual file read. For this USA executable, `0x100A` maps through table target `0x1F80` to helper `0x1614`, not `0x1660` (used by `0x100C`). Its zero input scales to **1024**, not the 109 mistakenly stated in an early local note.

**Bounded evidence:** The original gain slice matched an independent signed-integer reference in 65,545 cases, with zero mismatches. Twenty-nine original-setter fixture cases and six missing-effect refusals passed. A voice initializer executed in 2,081 diagnostic-interpreter steps; nine setter replays passed. Ten deterministic Scream worker/timer wakeups preserved inactive voice state. Eight selected follow-on Scream calls and ten SNDSCRM libsd-helper trace cases completed in fixtures. These are not hardware-validated IOP results: imports, clocks and scheduling use explicit test doubles; module relocation, general load-delay timing and real audio effects remain unverified.

**Harness correction:** Two SNDSCRM checks initially copied trailing ELF section/relocation data into BSS. Successor checks load only PT_LOAD (file size 14,600; memory size 30,536) and zero-fill BSS. All ten helper cases and three worker-branch results remained unchanged. Prior receipts are preserved as superseded evidence, not silently erased.

**Unknown / next:** The full live state across both sound workers and libsd reset/transfer-handler effects. The queued worker path reaches sysmem ordinal 7, `QueryMaxFreeMemSize`; it must use represented allocator state. Independently validate interpreter semantics and required IOP effects before enabling `0x100A`. Next probe run is 306. No complete boot, rendered gameplay, audible sound or controller result is claimed.

Evidence remains local under `startup-probe-001/run-305/` and `resume-305/` (gain, execution, initializer, scheduler, follow-on, corrected libsd-init and worker receipts). Only this curated summary is published; game binaries, generated game C++, copied module bytes, commercial SDKs and local compiler installations remain excluded. No gameplay screenshot exists to publish.

---

## Historical status (superseded where corrected above)

# Current development status

As of November 9, 2026; checkpoint 198. Audience: project followers and developers. Success means a playable native Windows build with tested input, graphics and sound. That condition has not been achieved.

## Current truth

**Proven locally:** Run-198 resolves the native SIF RPC-init stall. The guest SIF handler at `0x3cc0c0` was linked into the startup probe, the native SIF reply was queued and completed through the game's own handler, and the guest RPC-init flag at `0x661ec0` moved to 1. The run exits 2 in 0.281 seconds with no watchdog stop (runs 194-197 exited 4 at the five-second watchdog). The run now stops at a new target: a cdvdfsv READ at `0x3cce90`, called from `0x3b8604` with `a1=0x80000593`, so startup has moved from SIF initialisation into CDVD file loading. Executable SHA256: `a2c62da1c85850d218510fba270f8c3a7166d40e545614d66b6efb702bae1307`.

**Proven locally:** Run-195 reaches a native SIF RPC-init wait. After linking the actual generated resume entry at `0x3cc9a0`, the run waits between `0x3cc9a0` and `0x3cc9a8` and stops cooperatively at the five-second watchdog (exit 4, 5.875 seconds, no outer timeout). The guest RPC-init flag at `0x661ec0` is zero, the request header `[16, 0, 0x80000002, 1]` is present in IOP memory at address zero, and the IOP snapshot reports zero loaded modules, threads, RPC servers and executed instructions. Source inspection shows the SIF DMA stub copies the request and notifies the IOP subsystem, but the RPC bridge's transfer handler explicitly ignores it. The request copy alone does not create a command response. Executable SHA256: `8fef7301140a03cbf2f38a09c5c3152c04c5f1803e5249bbfa1da58556d144dd`.

**Proven locally:** The original initializer walker has returned. Run-183 reaches its final callback and then the next startup call at `0x368d28 -> 0x276da0`. Original executable words independently confirm that the initializer wrapper call precedes this point. Run-184 progresses into the following startup routines. This is observed control flow, not an individual correctness audit of all 128 callbacks.

**Proven locally:** Fourteen additional startup runs (185-198) compile and pass their previous observed missing call. The current subset has 561 original-game C++ files, 2,331 original dispatch addresses (including continuation labels), and 73 relocated addresses. Every run preserves pinned input hashes and three byte-matched startup copies. Run-198 resolves the SIF service stall; the run now stops at a cdvdfsv READ at `0x3cce90`.

**Proven locally:** Latest run (198) exits 2 at missing target `0x3cce90`, called from `0x3b8604`, after 0.281 seconds without timeout. Exit 2 is a deliberate strict stop, not a successful boot. The earlier run-195 exit-4 stop at the SIF RPC-init wait is superseded. Executable SHA256: `a2c62da1c85850d218510fba270f8c3a7166d40e545614d66b6efb702bae1307`.

**Proven locally:** Bounded dependency grouping can now use the exact exercised direct caller, with source/target and archive-byte checks. Regression controls reject wrong, duplicate and indirect call sites. A larger main-startup group exceeded the unchanged dependency bound and was refused before writing sources; a smaller eight-file plan compiled and ran. No game instructions were edited or runtime failure checks bypassed.

## Independent native checks

| Check | Observed result | Limit |
| --- | --- | --- |
| ISO-only disc access | 3,703,880 boot-file bytes match; 14 parser cases; image-switch and loose-file checks pass | Isolated CD primitive, not actual game asset streaming |
| First copied-kernel selector | 8 selector cases plus image/registration guards pass | Other kernel behavior and timing remain unverified |
| Second copied-kernel selector | 9 selector, 4 invalid-index and 1 register-retention checks pass | Full paired-page/ASID/TLB semantics remain unverified |
| Thread handling | 5 priority boundaries and 2 missing-entry policies pass | Not a complete PS2 scheduler audit |
| EntryHi translation | 65,572 cases, 0 mismatches; old-mask control disagrees in 65,555 | Isolated instruction behavior, not complete TLB mapping |
| Differential helper | 4,096 trials, 0 mismatches, mutation controls rejected | One leaf helper, not all generated game code |

These separate regression results were recorded before this source-import/planning extension. Their runtime sources and executables remain hash-identical; they were not rerun solely to inflate the test count.

## What this means

**Inference:** Correctly grouping deeper dependencies reduces repeated omitted-call stops during development. No speed or completion-date estimate is established.

**Unknown:** Individual initializer correctness, full runtime semantics, actual game asset streaming, rendering, audio, controller integration and gameplay. No gameplay screenshot exists because the current diagnostic has no demonstrated graphics output.

## Next

Link the reached cdvdfsv target `0x3cce90` (run 199) and assess the cdvdfsv RPC service it needs, following the same bounded native-handler pattern used for SIF: replace the SDK service with a native handler only where the game waits on it, and verify with independent checks. Then verify individual initializer effects and remaining startup semantics. Preserve original inputs, previous failed results and the bounded test workflow.
