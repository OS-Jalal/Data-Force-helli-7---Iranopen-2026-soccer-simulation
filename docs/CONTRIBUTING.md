<div dir="ltr" align="left">

# Contributing

[English](CONTRIBUTING.md) | [فارسی](CONTRIBUTING.fa.md)

Contributions that improve clarity, reproducibility, and team behavior are welcome.

## Reporting a problem

Include the Webots version, Python/NumPy versions, simulator commit, robot names, opponent controller, reproduction steps, and the relevant console traceback. Describe what you expected and what actually happened.

## Proposing changes

1. Work on a dedicated branch or fork.
2. Describe the problem and the resulting behavior in your pull request.
3. Keep unrelated changes separate. Preserve upstream license and attribution notices.
4. Update both README languages if setup or behavior changes.
5. For strategy changes, document match settings and repeated-match results; a single score does not establish improvement.
6. Identify any behavior change from the original published source.

## Validation

For syntax checking from the repository root:

</div>

<div dir="ltr" align="left">

```bash
python -m compileall -q rcj_soccer_team_yellow
```

</div>

<div dir="ltr" align="left">

This command does not run a Webots match. Validate runtime changes in Webots with the official simulator. Include the versions and findings in the pull request. Check the known source findings in [TECHNICAL.md](TECHNICAL.md) before modifying the role protocol or supervisor handling.

## Licensing

By intentionally submitting a contribution for inclusion in this project, you agree to its licensing under Apache License 2.0, subject to the contribution terms in [LICENSE](../LICENSE). Retain applicable third-party notices.

</div>
