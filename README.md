# pilot-skills

Harness-agnostic skills for the agent that flies the simulator through irl-gym. A version is a folder that is a skills root, so it installs with the skills CLI:

```
npx skills add narevai/pilot-skills/v2
```

`v1` is kept as it was. `v2` is split by phase of flight (`before-takeoff`, `takeoff`, `climb-and-cruise`, `descent-and-approach`, `final-and-landing`, `go-around`, `stall-and-upset-recovery`) with `fly-the-airplane` limited to how the tools work.

```
v2/
  skills/<skill>/SKILL.md
  skills/<category>/<skill>/SKILL.md
```

A skill never names a harness. The task prompts and the standing instructions live in irl-gym; the command, the MCP configuration and the place the skills are copied to belong to the harness repos.
