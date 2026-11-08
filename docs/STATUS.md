# Current development status

As of November 8, 2026; checkpoint 184. Audience: project followers and developers. Success means a playable native Windows build with tested input, graphics and sound. That condition has not been achieved.

## Current truth

**Proven locally:** The original initializer walker has returned. Run-183 reaches its final callback and then the next startup call at `0x368d28 -> 0x276da0`. Original executable words independently confirm that the initializer wrapper call precedes this point. Run-184 progresses into the following startup routines. This is observed control flow, not an individual correctness audit of all 128 callbacks.

**Proven locally:** Fourteen additional startup runs (171-184) compile and pass their previous observed missing call. The current subset has 534 original-game C++ files, 2,240 original dispatch addresses (including continuation labels), and 73 relocated addresses. Every run preserves pinned input hashes and three byte-matched startup copies.

**Proven locally:** Latest run exits 2 at missing target `0x3b8b50`, called from `0x3b8bf0`, after 0.281 seconds without timeout. Exit 2 is a deliberate strict stop, not a successful boot. Executable SHA256: `8867f6307ea39422d4e89a6470252d4edc3cf961e0c1fadd41008fc75a6d9870`.

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

Resolve measured missing target `0x3b8b50`, then verify the next actual execution result. Investigate service/semantic failures independently; do not replace failures with fake success. Preserve original inputs, previous failed results and the bounded test workflow.
