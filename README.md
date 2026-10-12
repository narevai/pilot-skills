# pilot-skills

Harness-agnostic instructions and skills for the agent that flies the simulator through irl-gym. A version is a folder:

```
v1/
  AGENTS.md              standing instructions
  prompt.md              task prompt template ({departure}, {destination}, {route_skill}, ...)
  skills/<skill>/SKILL.md          common skills, always installed
  routes/<route>/SKILL.md          route skills, chosen per run in the irl-gym dashboard
```

A skill never names a harness. A route skill may point to another route skill (the cold start continues with `yssy-yscb`); select both when queueing. The command, the MCP configuration and the place the skills are copied to belong to the harness repos (`pilot-opencode`, `pilot-cursor`).
