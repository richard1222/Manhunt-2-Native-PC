# Project updates

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
