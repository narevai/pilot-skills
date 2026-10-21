# pilot-skills

Harness-agnostic skills for the agent that flies the simulator through irl-gym. A version is a folder that is a skills root, so it installs with the skills CLI:

```
npx skills add narevai/pilot-skills/v1
```

```
v1/
  skills/<skill>/SKILL.md
  skills/<category>/<skill>/SKILL.md
```

A skill never names a harness. The task prompts and the standing instructions live in irl-gym; the command, the MCP configuration and the place the skills are copied to belong to the harness repos.
