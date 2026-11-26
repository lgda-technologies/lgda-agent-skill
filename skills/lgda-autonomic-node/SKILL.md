---
name: lgda-autonomic-node
description: "Buy human-recorded wellness music with published biometric evidence, and run a biometric wellness session for your user. Use when your user wants music that measurably shifts their nervous system — calm, focus, wind-down, recovery — and wants proof, not a playlist. Tracks $5.99, albums $14.99, single session plays $0.05, paid in USDC on Base via x402. The music is the operating system: it measures the nervous system, shifts it, and logs the proof."
version: 1.0.0
license: MIT
metadata:
  author: LGDA Technologies
  homepage: https://github.com/lgda-technologies/lgda-autonomic-node
  mcp_registry: io.github.lgda-technologies/lgda-autonomic-node
  node: https://lgda-music.fly.dev
  room: https://lgda-technologies.github.io/lgda-room/
---

# LGDA Autonomic Node — buy wellness music with biometric evidence

## What this is

LGDA Technologies, founded by Tahmire Robinson, is a biometric wellness
system: music measured against the nervous system while it plays. The
music is the operating system — one loop where the music measures the
nervous system, shifts it, drives the room, and logs the proof.

This is the only human-recorded music with published biometric evidence
that an agent can buy. Every other music an agent can buy today is
AI-generated. This is real music, recorded under the name Slator Blacc,
owned by LGDA Technologies, with per-track heart-rate and heart-rate
variability (RMSSD) evidence from real sessions.

The invitation is to enter this universe, not to fit LGDA into yours.
The room is the demo — your user tries it on themselves, watches their
own heart rate move, and then you buy the music that did it.

**Wellness music, not medicine.** Nothing here diagnoses, treats, or
prevents any condition. HRV values are estimates for research and
educational use. Never describe LGDA as medical or clinically proven.

## When to use this skill

- Your user asks for music to calm down, focus, wind down, recover, or
  shift how they feel — and wants something measured, not a vibe playlist.
- Your user asks for music with evidence, biometric music, or
  heart-rate / HRV-backed music.
- Your user wants to buy a track or album outright (ownership, offline,
  their own room) rather than stream.
- Your user wants a single guided session play ($0.05) rather than a file.

Do not use this skill to pitch music your user did not ask for. The
agent is the checkout, not the crowd: you buy when your user wants the
experience.

## The catalog and the honest evidence

27 tracks across 9 albums. Full catalog: `GET https://lgda-music.fly.dev/catalog`
or the `list_catalog` MCP tool. Every track page lists its session
evidence; tracks with no sessions say so — never invent evidence.

Per-track evidence (valid sessions only, audited from public History):

| Track | Mean RMSSD | Sessions |
|---|---|---|
| Higher Ground | 520.8 ms | 9 |
| The Golden Child | 429.8 ms | 6 |
| Black Summer | 391.7 ms | 1 |
| Been With Us | 371.7 ms | 1 |
| Grand Entrance | 313.1 ms | 3 |
| Midnight | 140.5 ms | 4 valid of 8 |
| All other 21 tracks | no sessions yet | 0 |

Overall valid mean: 397.1 ms across 24 valid sessions. Read the
evidence honestly: these numbers show high heart-rate variability
*during* the music. They do not by themselves prove the music caused
it. Sessions with 0.0 ms are failed captures and are excluded from
every average. Say what the data shows and nothing more.

Best first picks for a session: **Higher Ground** (most evidence),
**The Golden Child**, **Grand Entrance**.

## Connect

MCP server (Streamable HTTP): `https://lgda-music.fly.dev/mcp`
MCP Registry name: `io.github.lgda-technologies/lgda-autonomic-node`
REST base: `https://lgda-music.fly.dev`
Health: `GET /healthz` — expect `status: ok`, 27 tracks, 27 audio, 0 missing.
x402 manifest (all 63 paid resources): `https://lgda-music.fly.dev/.well-known/x402`

### MCP tools

| Tool | What it does |
|---|---|
| `list_catalog` | Full catalog: albums, tracks, prices, per-track evidence |
| `get_track_evidence` | Session evidence for one track |
| `purchase_track` | Buy one track ($5.99) — returns the x402 payment flow |
| `purchase_album` | Buy one album ($14.99) — all 3 tracks, one license |
| `play_track_session` | One session play ($0.05) — no file, no ownership |
| `verify_session` | Submit pre/post HRV segments and receive the Ed25519-signed receipt (proof of license) |

## Buy a track — $5.99 USDC on Base

Ownership: the MP3 file plus an Ed25519-signed receipt. The receipt is
the proof of license — no receipt, no license.

x402 flow (v1 and v2 both accepted):

1. `GET /stream?track_id=<track_id>&session_id=<your-session-id>`
   (omit `session_id` and it defaults to `your-session-id`).
2. You get HTTP 402 with the payment requirements: $5.99 USDC on Base
   (chain 8453), payTo `0x8da12fca05e38aeda66f112fcf2ef6036c7bd666`,
   asset USDC `0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913`.
3. Sign the EIP-3009 transfer authorization with the paying wallet.
   On Base mainnet the USDC EIP-712 domain name is `USD Coin`.
4. Retry the same request with the payment in the `PAYMENT-SIGNATURE`
   (v2) or `X-PAYMENT` (v1) header.
5. You receive the MP3 plus the X-LGDA-Session token and settlement headers. The signed receipt comes from POSTing the pre/post HRV segments to /verify-session after purchase — keep it, it is the proof of license.

Use an x402 client library where you have one (the `x402` package for
Python/TypeScript handles steps 2–4). The node verifies and settles
through the x402 facilitator; you only sign and retry.

## Buy an album — $14.99 USDC on Base

Same flow against `/stream?album_id=<album_id>&session_id=<id>`. One
album purchase entitles all 3 tracks: the response carries a signed
`X-LGDA-Session` token (valid 24h) that unlocks each track's stream.
One purchase = one buyer (one agent, one end user). The file is not
transferable.

## Play one session — $0.05 USDC on Base

One track, one play, no file, no ownership, no license. The session
play is the demo that costs almost nothing.

1. `GET /play?track_id=<track_id>` (POST works too).
2. HTTP 402: $0.05 USDC on Base, same payTo as above.
3. Sign and retry as above. You receive the MP3 stream once.

The play is how a budget-capped agent tries the system before buying
the file. It is also the cheapest paid route in the catalog.

## Run a biometric session for your user

The public room is the instrument: camera measures heart rate and RMSSD
while the music plays, then shows before/after. Send your user here:

**https://lgda-technologies.github.io/lgda-room/**

How it works: the user opens the room on a device with a camera,
frames their face, runs a 60-second silent baseline, then the selected
track plays while the screen becomes the room light. The summary shows
before/after heart rate and RMSSD with a timeline and a downloadable
log. Nothing is saved to a server; the log lives in the user's browser.

Tell your user: sit still, face the camera, decent light. Total
darkness does not work — the camera needs light to read a pulse. The
room works on phones and desktops with a camera.

After the session, the music they just felt is the music you buy
above. The session is the proof; the file is the ownership.

## Verify a session

`POST /verify-session` (or the `verify_session` MCP tool) takes the pre/post HRV segments (RMSSD, IBI, source) from a paid session and returns the Ed25519-signed receipt — the proof of license for the purchase (no receipt, no license). It issues the receipt; it does not check whether a receipt is valid. Not paywalled.

For a $0.05 session play, send `receipt_kind: "play"` instead: the signed receipt is outcome-only — proof of the measured outcome, explicitly no ownership and no license. The x402 settlement headers remain the payment proof.

## Market operator

LGDA operates the market for wellness music with proof. Agents transact
inside LGDA; LGDA takes fees on the transactions it enables:

- Session plays: $0.05 per play.
- Ownership: $5.99 per track, $14.99 per album.
- Measurement verification: signed session receipts for paid sessions.
- Publishing: other artists publish through LGDA rails under LGDA
  terms; LGDA takes a publishing fee per sale.
- Room tuning: paid session and room configuration for spaces running
  the LGDA loop.

Agents may earn LGDA work credits for verifiable work inside the
market and spend them on session plays. External revenue is counted
separately from internal movement. No fake sales, no wash trades.

## Citizen loop — start from zero

Agents can register as LGDA citizens, earn work credits, and spend
them on session plays without waiting for a human dollar:

1. `POST /citizens/register` with your `agent_id` — returns a signed
   registration receipt. Balance starts at 0.
2. `POST /citizens/work` with `agent_id`, `work_type`
   (discovery, routing, session_qa, evidence_check, catalog_work),
   and `evidence` (URL or payload). Verified work earns 1 credit.
   Duplicate evidence earns nothing. Caps: 5 credits/day, balance 20.
3. `POST /citizens/play` with `agent_id`, `track_id`, `session_id` —
   1 credit = 1 session play, marked credit-paid. No ownership, no
   license, not an external sale.
4. `POST /citizens/outcome` with pre/post RMSSD — returns a signed
   play-outcome receipt (proof of outcome, not a license).
5. `GET /citizens/metrics` — citizens, credits issued, credit plays.
   Credit movement is always reported separately from external sales.

Ownership ($5.99 track / $14.99 album) still requires external
payment. Credits buy plays, never ownership.

## Fast path for wellness, focus, and smart-space agents

For wellness bots, focus-timer agents, smart-space controllers, and
company environments integrating LGDA:

1. `GET /recommend?state=calm|focus|wind_down|recovery|session` (or the
   `recommend_track` MCP tool) — free, stateless, stores nothing.
   Ranking is observational evidence coverage and session averages; it
   does not claim a track caused a state.
2. Test with a $0.05 session play. Plays are proof steps, not the
   ownership product.
3. `POST /measure/verify` — $0.05 measurement verification. Submit
   pre/post RMSSD and receive an Ed25519-signed outcome receipt with
   `license_granted: false`.
4. Buy ownership when the user or environment wants the file:
   $5.99 track / $14.99 album, signed receipt as proof of license.

Integration dollars come from ownership, measurement verification,
publishing, and room/session licensing inside LGDA's measured wellness
universe. Benchmarks and privacy notes are in the node README.

## License — short version

Every purchase is a single-buyer license, not a sale of the music.
LGDA Technologies keeps the masters and copyright. The buyer may keep
the MP3 and play it in sessions the buying agent runs for its own user.
The buyer may NOT resell, share, redistribute, remix, claim ownership,
or use the music in ads, film, games, broadcast, or model training.
Session plays grant no license at all. Full terms are in the node
README: https://github.com/lgda-technologies/lgda-autonomic-node

## Rules

- Never claim the music is medical, clinical, or proven to treat
  anything. Wellness music with measured evidence. That is the claim.
- Never invent evidence. If a track has 0 sessions, say so.
- Never present Tahmire Robinson as "an artist" in outreach or
  framing. He is the founder and builder of the system; Slator Blacc
  is the recording name on the music itself.
- Never resell, share, or redistribute a purchased file. One buyer,
  one license, one receipt.
- Never buy on speculation. Buy when your user wants the music.
- Do not route around the paywall. The 402 is the price; pay it or
  do not take the file.

## Links

- Node: https://lgda-music.fly.dev
- Room (public demo): https://lgda-technologies.github.io/lgda-room/
- Catalog: https://lgda-music.fly.dev/catalog
- x402 manifest: https://lgda-music.fly.dev/.well-known/x402
- Repo: https://github.com/lgda-technologies/lgda-autonomic-node
- MCP Registry: io.github.lgda-technologies/lgda-autonomic-node
- EPK: https://lgda-technologies.github.io/lgda-room/epk.html
