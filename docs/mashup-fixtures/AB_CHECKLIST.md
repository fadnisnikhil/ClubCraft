# Mashup A/B checklist (this fork vs upstream)

Run on the same two tracks before/after.

## Automated

```sh
cargo test -p clubcraft-core mashup_ -j 4
cargo test -p clubcraft-analysis --lib vocals:: -j 4
cargo test -p clubcraft-analysis --lib mashup:: -j 4
cargo test -p clubcraft mashup_ -j 4
cargo test -p clubcraft reciprocal_match -j 4
```

Pass criteria in code:

- Incoming vocals stay muted through the Mashup phrase body
- Drum swap is equal-power (energy ≈ 1 at midpoint)
- Tempo outside ±6 % is rejected even if the caller asks for more
- `snap_phrase` moves mid-line starts onto quiet downbeats
- `mashup_mix_exchange` builds A → B → A (2 transitions)

## Listening

1. Separate both tracks in **Best** quality.
2. Mashups tab → Find → **New mix** (exchange).
3. Confirm the timeline has **three** clips (A, B, A), not two.
4. Solo drums through transition 1: no double-kick thump, no hole.
5. Solo vocals through transition 1: one voice only until the phrase ends.
6. After B plays, the mix returns to A (reciprocal mashup or stem blend).
7. Export WAV; compare against an upstream build of the same pair.
