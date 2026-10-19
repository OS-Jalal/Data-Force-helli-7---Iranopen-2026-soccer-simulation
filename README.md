<div dir="ltr" align="left">

# Data Force helli 7

**1st Place · Iran Open RoboCup 2026 · Junior Soccer Simulation Secondary**

[English](README.md) | [فارسی](README.fa.md)

Python controllers for the yellow team in the Webots-based **RCJ Soccer Simulation** environment, released by **Data Force helli 7**, Allameh Helli 7 High School (SAMPAD), Iran.

## Competition and team

| Field | Details |
| :--- | :--- |
| Competition | Iran Open RoboCup 2026 |
| League | Junior Soccer Simulation Secondary |
| Result | 1st place |
| School | Allameh Helli 7 High School — SAMPAD |
| Mentor | Ali Asghar Jalalieh — علی اصغر جلالیه |
| Language / simulator | Python / Webots |

| Team member | Contribution |
| :--- | :--- |
| MohammedHossein Ghaffary | Equal, direct contribution to the team code |
| Alireza Bashiri vasegh | Equal, direct contribution to the team code |
| Sobhan Barzegar | Equal, direct contribution to the team code |
| Mohammadhasan Heydarifar | Equal, direct contribution to the team code |

The project was started from scratch by the team, as reported by its mentor. The team application is distinguished from the official simulator infrastructure and bundled interface code: see [NOTICE](NOTICE). All four members contributed equally; separate specialist roles are not assigned here.

Competition reference: [official league page](https://iranopenrobocup.ir/leagues/junior-soccer-simulation-secondary). Team membership and placement above were supplied by the mentor; the results table could not be independently retrieved during documentation preparation.

## What is included?

This repository contains the **complete controller folder supplied for publication**, consisting of 11 Python files. It does **not** bundle the simulator world, referee, blue-team controller, textures, Webots application, match recordings, or original Git history. Obtain the simulation infrastructure from the [official RCJ Soccer Simulator](https://github.com/robocup-junior/rcj-soccersim).

The controller source is preserved byte for byte from the supplied archive. This documentation release does not certify that the supplied snapshot is identical to the exact code executed during the final match.

## Implemented behavior

The source contains:

- Three yellow robots, `Y1`, `Y2`, and `Y3`, sharing the same controller entry point.
- Dynamic allocation of two attacking roles and one defensive role.
- Ball localization from local sensing and teammate observations.
- Ball-position prediction and freshness handling for shared observations.
- JSON messages for positions, ball observations, role assignment, and coordinated trap stages.
- Positioning, ball approach, defensive tracking, and coordinated attacking behavior.
- Motion-progress monitoring and per-cycle motor stopping with throttled traceback reporting after unexpected controller errors.

These are code-level observations, not measured performance claims. No learned model or training pipeline is included.

## Requirements and compatibility

| Component | Requirement / status |
| :--- | :--- |
| Python | Python 3, 64-bit, configured as Webots' Python interpreter |
| Webots | The official simulator currently documents **R2025a**; the team's exact competition version was not supplied |
| Python dependency | `numpy` — imported by offense and defense modules |
| Webots Python API | `controller`, provided by Webots; **do not install an unrelated PyPI package named controller** |
| Simulator | Official RCJ Soccer Simulator; use the original competition base when available |
| Team supported by this snapshot | Yellow only; use `Y1`, `Y2`, `Y3` |

No dependency lockfile or exact original Python/NumPy versions were supplied. `requirements.txt` lists the observed third-party dependency without inventing version pins. The upstream revision below is a documented setup reference, not the verified competition revision.

## Setup

### 1. Obtain both projects

Download and extract this repository, then open a terminal in its root directory (the directory containing this README).

Clone the simulator into a **separate sibling directory**, not inside this repository:

</div>

<div dir="ltr" align="left">

```bash
git clone https://github.com/robocup-junior/rcj-soccersim.git ../rcj-soccersim
```

</div>

<div dir="ltr" align="left">

For the upstream revision inspected while preparing these instructions:

</div>

<div dir="ltr" align="left">

```bash
git -C ../rcj-soccersim checkout 446103610b63544dba784068b18a1c68b71433fc
```

</div>

<div dir="ltr" align="left">

If you have the actual Iran Open competition base, use that instead and record its revision. The referenced official world is `worlds/soccer.wbt`.

### 2. Install NumPy in Webots' Python environment

Use the **same Python executable** that Webots will use:

</div>

<div dir="ltr" align="left">

```bash
python -m pip install -r requirements.txt
python -c "import sys, numpy; print(sys.executable); print(numpy.__version__)"
```

</div>

<div dir="ltr" align="left">

On a system where Python 3 is called `python3`, substitute `python3` in both commands. If using a virtual environment, configure Webots with the environment's full Python executable path.

### 3. Install the yellow-team controller

1. Back up the simulator's existing `controllers/rcj_soccer_team_yellow/` folder outside the simulator.
2. Replace that folder with this repository's entire `rcj_soccer_team_yellow/` folder.
3. Do not copy only the entry-point file: all 11 `.py` files must remain together.
4. Keep the folder and entry-point names unchanged:
   - Folder: `controllers/rcj_soccer_team_yellow/`
   - Entry point: `rcj_soccer_team_yellow.py`

The resulting simulator installation must contain:

| Path inside the simulator | Source |
| :--- | :--- |
| `worlds/soccer.wbt` | Official simulator |
| `controllers/rcj_soccer_referee_supervisor/` | Official simulator |
| `controllers/rcj_soccer_team_blue/` | Official simulator / chosen opponent |
| `controllers/rcj_soccer_team_yellow/` | This team's complete controller folder |

### 4. Run in Webots

1. Install and start Webots.
2. Set **Tools → Preferences → Python command** to the Python executable used above (menu wording can vary by platform).
3. Choose **File → Open World** and open the simulator's `worlds/soccer.wbt`.
4. Check that the robots are named `Y1`, `Y2`, `Y3` and their `controller` field is `rcj_soccer_team_yellow`. The inspected official world already uses these names.
5. Leave the official referee and blue-team controller available.
6. Start the simulation and inspect the Webots console for import or controller errors.

Once Webots is installed and its executable is on PATH, the official world can also be launched from the simulator directory:

</div>

<div dir="ltr" align="left">

```bash
webots --mode=run worlds/soccer.wbt
```

</div>

<div dir="ltr" align="left">

Do **not** run `python rcj_soccer_team_yellow.py` as a standalone application. It depends on Webots robot devices and simulation startup.

## Source guide

| File under `rcj_soccer_team_yellow/` | Purpose |
| :--- | :--- |
| `rcj_soccer_team_yellow.py` | Creates Webots' Robot and starts MyRobot |
| `my_robot.py` | Main control loop, messaging, role dispatch, error handling |
| `data.py` | Local/shared observations, coordinates, freshness, ball prediction |
| `roles.py` | Dynamic role mapping and role changes |
| `offense1_yellow.py` | First attacking strategy |
| `offense2_yellow.py` | Second attacking strategy |
| `defense_yellow.py` | Defensive behavior and positioning |
| `progress.py` | Movement-progress tracking for robots and ball |
| `rcj_soccer_robot.py` | Official-simulator interface adaptation |
| `rcj_soccer_robot_compat.py` | Team-packet compatibility and heading helpers |
| `utils.py` | Direction helper |

See [technical notes](docs/TECHNICAL.md) for packet formats, important constants, and known review findings.

## Troubleshooting

| Symptom | What to check |
| :--- | :--- |
| `No module named numpy` | Install requirements with Webots' actual Python interpreter |
| `No module named controller` | Launch through Webots; check its Python configuration |
| Controller not found | Folder name and main `.py` filename must match |
| Local module import fails | Copy all 11 Python files together |
| Blue robots fail with this controller | This snapshot initializes yellow strategies only; retain a separate blue controller |
| World / referee missing | Install the official simulator separately |
| Motors stop and traceback appears | Inspect the first console traceback; see the technical review notes |

## Validation and limitations

Documentation preparation checked all 11 files for Python syntax, imported the local modules, and exercised representative message/data paths outside Webots. The source files were compared with the supplied archive to verify byte preservation.

**No full Webots match was run in the preparation environment.** Static/import checks do not establish match performance, complete behavior correctness, exact competition reproducibility, or compatibility with every upstream version. Known source-review findings are recorded in [docs/TECHNICAL.md](docs/TECHNICAL.md); the original controller was not silently changed.

## Reuse and attribution

Published under the **Apache License 2.0**. You may use, study, modify, and redistribute the code, including commercially, subject to the license terms. Keep the license and applicable notices, and identify changes as required by the license. See [LICENSE](LICENSE) and [NOTICE](NOTICE).

## Contributing

Please describe the change, the environment used, and how you tested it. For behavior changes, report the Webots version, simulator revision, opponent controller, match settings, and repeated-match results. Keep the English and Persian documentation synchronized.

## References

- [Iran Open league page](https://iranopenrobocup.ir/leagues/junior-soccer-simulation-secondary)
- [Official simulator source](https://github.com/robocup-junior/rcj-soccersim)
- [Official simulator documentation](https://robocup-junior.github.io/rcj-soccersim/)
- [Webots](https://cyberbotics.com/)

## Further documentation

- [Contributing](docs/CONTRIBUTING.md)
- [Validation record](docs/VALIDATION.md)
- [Publishing guide](docs/PUBLISH.md)

</div>
