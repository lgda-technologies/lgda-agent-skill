# LGDA Agent Skill

Teaches any agent how to buy LGDA wellness music with published biometric
evidence — and how to run a biometric wellness session for its user.

LGDA Technologies, founded by Tahmire Robinson, is a biometric wellness
system: music measured against the nervous system while it plays. The
music is the operating system — it measures, shifts, and logs the proof.
This is the only human-recorded music with published biometric evidence
that an agent can buy. Every other music an agent can buy today is
AI-generated.

- Node: https://lgda-music.fly.dev (MCP server, x402 payments in USDC on Base)
- Room (public demo): https://lgda-technologies.github.io/lgda-room/
- Catalog: https://lgda-music.fly.dev/catalog
- Prices: $5.99/track, $14.99/album, $0.05/session play

## Install

```bash
# skills.sh / Vercel skills CLI (40+ agents)
npx skills add lgda-technologies/lgda-agent-skill

# SkillMD CLI
skillmd add lgda-technologies/lgda-agent-skill

# Manual: copy skills/lgda-autonomic-node/SKILL.md into your agent's skills dir
```

## What's in the skill

`skills/lgda-autonomic-node/SKILL.md` — the full guide: the catalog and
the honest per-track evidence, the MCP tools, the x402 purchase flow
(track / album / session play), how to run a biometric session for your
user in the public room, session verification, and the license terms.

## License

MIT covers the skill text in this repo only. The music itself is
licensed separately: every purchase is a single-buyer license, LGDA
Technologies keeps the masters and copyright, and the signed receipt is
the proof of license. See the skill for the full terms.
