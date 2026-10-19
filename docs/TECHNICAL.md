<div dir="ltr" align="left">

# Technical notes

[English](TECHNICAL.md) | [فارسی](TECHNICAL.fa.md)

## Control flow

The entry point creates a Webots `Robot`, reads the numeric suffix of its name, and creates `MyRobot` for all three players. `MyRobot` inherits the compatibility wrapper, which inherits the official-style simulator interface.

During a 32 ms control cycle, the controller drains teammate packets, processes ball observations when available, updates local/shared data, tracks movement progress, receives or broadcasts role mappings, dispatches the active strategy, and broadcasts position and orientation. Y3 calculates and broadcasts roles; a robot's ID is not its permanent tactical role.

The loop checks for supervisor data but does not consume the supervisor packet or inspect `waiting_for_kickoff`. See the review findings below.

## Files and state

- `Data`: coordinates, ball localization, choice of a primary observer, observation age, confidence, and forecast state.
- `Roles`: mapping between robot IDs and tactical roles; role 1 = offense 1, role 2 = offense 2, role 3 = defense.
- `Progress`: windows of movement samples for robots and ball.
- `OffenseYellow1`, `OffenseYellow2`, `DefenseYellow`: strategy state and motor commands.

The initial yellow mapping in `Roles.resetRoles` is robot 1 → offense 1, robot 2 → defense, robot 3 → offense 2. It can change during play.

## Team protocol

The compatibility layer accepts JSON arrays/lists of length four or dictionaries containing all of `id`, `x`, `y`, and `value`. It serializes outbound packets as `[packet_id, x, y, value]`.

| Packet ID | Meaning |
| :--- | :--- |
| `1`, `2`, `3` | Robot position and heading; heading `-1` indicates a flipped robot |
| `10`, `20`, `30` | Robot ball observation and signal strength; the code also sends an absent-observation marker |
| `-1` | Role assignment: robot 1 role, robot 2 role, robot ID currently assigned defense |
| `100`, `200`, `300` | Coordinated offensive trap-stage state |

The role packet's third payload field is **not** a third role number. `Roles.receiveRoles(robot1_role, robot2_role, robot3_role)` currently treats it as such. See the review finding; do not document this as a verified consistent protocol.

## Parameters found in the source

| Parameter | Value | Location / purpose |
| :--- | :--- | :--- |
| `TIME_STEP` | 32 ms | `rcj_soccer_robot.py`, control interval |
| `Data(self.name, 80)` | prediction gain 80 | `my_robot.py` |
| `TEAM_DATA_FRESHNESS_TIMEOUT` | 0.5 s | `data.py` |
| `BALL_SIGNAL_FRESHNESS_TIMEOUT` | 0.5 s | `data.py` |
| `BALL_AGREEMENT_DISTANCE` | 0.20 | `data.py`, observation agreement threshold |
| `TEAM_CONFIDENCE_CORROBORATION_WEIGHT` | 0.25 | `data.py` |

Other thresholds and field-position constants are embedded in strategy modules. Changing them changes behavior; they have not been tuned as part of publication preparation.

## Review findings in the preserved snapshot

These are observations, not applied fixes:

1. **Yellow-only strategy initialization.** `MyRobot.run` creates its strategy instances only if the robot name contains `Y`; assigning this controller to blue robots leaves those instances undefined.
2. **Role receiver return value.** `Roles.receiveRoles` updates `self.roles` but returns `None`. Its caller uses `if roles.receiveRoles(...)` before updating `last_role_packet_time`. The timestamp therefore does not update via that branch, and reported role-packet age can remain infinite even when a mapping was received.
3. **Role packet semantic mismatch.** Y3 sends `roles.getRoleToRobot(3)` as the third role payload, but the receiver stores that field as robot 3's role. A mapping such as `{1: 2, 2: 3, 3: 1}` produces a received mapping `{1: 2, 2: 3, 3: 2}`. This is reproducible without Webots; an invariant-preserving protocol change should be tested separately.
4. **Supervisor queue / kickoff handling.** The main loop checks `is_new_data()` without calling `get_new_data()`. Supervisor packets are not drained by this loop and the official kickoff flag is not inspected here. Effects on match behavior and queue growth need a Webots run.
5. **Competition environment unknown.** The exact simulator revision, Python/NumPy versions, operating system, and final-match source identity are not supplied.

Publication preparation intentionally preserves the supplied code. Any follow-up fixes should be separate commits or releases, with Webots validation and clear release notes.

## Validation scope

All 11 Python modules parse and import in a local Python environment with NumPy. Importing the entry point was checked with a minimal test-only Webots substitute that immediately ends the control loop; this establishes import wiring, not Webots API/device correctness. Representative compatibility packets, data construction, initial role mapping, and the role issues above were also checked. Source SHA-256 hashes are recorded in `SOURCE_SHA256SUMS.txt`.

No physical simulation, full match, tournament replication, score comparison, or dependency-version compatibility matrix was run. Do not interpret syntax or smoke checks as evidence that the known findings are harmless.

</div>
