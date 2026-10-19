<div dir="ltr" align="left">

# Publication validation record

[English](VALIDATION.md) | [فارسی](VALIDATION.fa.md)

Prepared on 2026-10-19.

- Local Python: 3.12.14; local NumPy: 2.3.5. These describe smoke checks only, not a verified Webots runtime or competition environment.
- All 11 Python files parsed successfully.
- Local module imports passed.
- The entry point imported and initialized against a minimal test-only Webots Robot substitute; the substitute immediately returns -1 from step, so no simulation cycle runs.
- Valid list/dictionary compatibility packets parsed; a short malformed packet was rejected.
- Data initialization for Y1, Y2, and Y3 passed.
- Initial yellow role mapping was checked.
- Missing return from Roles.receiveRoles and inconsistent third role-packet field were reproduced and documented.
- Every distributed Python file matches its original archive entry byte for byte.
- All relative Markdown links resolve to included files.
- SHA-256 checksums are included at repository root.

Upstream setup reference: robocup-junior/rcj-soccersim, commit 446103610b63544dba784068b18a1c68b71433fc. This is not asserted to be the competition revision.

The source archive had no LICENSE, README, world file, lockfile, or Git history. The publication adds documentation, requirements, ignore rules, Apache-2.0 licensing, upstream attribution, and source checksums; it does not alter controller source.

No Webots executable was available. No full match or physical simulation was run. Competition placement and equal contributions are supplied by the mentor; the official league results table could not be independently retrieved during preparation. No final-match source identity is asserted.

</div>
