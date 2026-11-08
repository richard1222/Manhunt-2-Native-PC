# Current development status

As of November 8, 2026; checkpoint 170. Audience: project followers and developers. Success means a playable native Windows build with tested input, graphics and sound. That condition has not been achieved.

## Current truth

**Proven locally:** Thirteen additional startup runs (158-170) build and pass their previous observed missing call. The current subset has 502 original-game C++ files, 2,143 original dispatch addresses (including continuation labels), and 73 relocated addresses. All 128 startup callback addresses are linked. Every run preserves pinned input hashes and three byte-matched startup copies.

**Proven locally:** Latest run exits 2 at missing target `0x1fb2b8`, called from `0x1f6034`, after 0.297 seconds, without timeout. Exit 2 is a deliberate strict stop, not a successful boot. The executable SHA256 is `d5f2df9054b5a118a932cae90a15f968a165c7a2b2d5732f37c0ff3d57374459`.

**Proven locally:** A planning regression reproduces missed grandchildren in the old initializer selection, and the corrected depth-bounded search passes depth, limit, cycle and unknown-target cases. Its integration selects previously overlooked dependencies; it does not alter game instructions or runtime failure policy.

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

**Unknown:** Completed initializer execution, full runtime semantics, actual game asset streaming, rendering, audio, controller integration and gameplay. No gameplay screenshot exists because the current diagnostic has no demonstrated graphics output.

## Next

Resolve measured missing target `0x1fb2b8`, then verify the next actual execution result. Investigate service/semantic failures independently; do not replace failures with fake success. Preserve original inputs, previous failed results and the bounded test workflow.
