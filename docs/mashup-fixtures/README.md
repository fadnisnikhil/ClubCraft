# Mashup quality fixtures

Hand-marked failure modes to A/B against before/after builds. Fill in real library track names when reproducing.

## Fixture F1 — overlapping beats / jagged instrumental swap

| Field | Value |
|---|---|
| Symptom | Double kick / flammed drums for ~1 bar; bass cliff; hard edge at swap |
| Cause (upstream) | `TransitionStyle::Mashup` swaps drums/melody linearly over **1 bar**; bass hard-switches at mid-bar |
| Stem quality | Often Fast (`htdemucs`) — also leaks into vocal map |
| Transition | `Mashup` or `MashupIn` |
| Expected after fix | Equal-power ~2-bar instrumental crossfade; soft bass handoff; no double-kick thump |

Reproduce: pick two same-key house tracks ±3 BPM, New mix from Mashups tab, solo drums through the transition.

## Fixture F2 — mid-phrase / jagged vocal edges

| Field | Value |
|---|---|
| Symptom | Vocal phrase starts mid-line or chops the last syllable |
| Cause | Unit search shifted whole fixed length ±2 bars; end not snapped independently to a quiet bar |
| Stem quality | Fast stems amplify false singing |
| Expected after fix | Start snapped to quiet half-bar before downbeat; length adjusted so end lands in a line break; mid-line units score 0 and drop |

Reproduce: `cargo run -p clubcraft-analysis --example mashup_places -- <vocal> <inst>` and listen to top unit boundaries.

## Fixture F3 — one-way A → B only (no return)

| Field | Value |
|---|---|
| Symptom | Mashup is track 1 → one transition → track 2 forever; never returns to A or alternates |
| Cause | `mashup_mix` built exactly two `MixTrack`s and one transition |
| Expected after fix | Default **exchange** mix: A → Mashup → B → return MashupIn → A (outro), when a reciprocal match exists |

Reproduce: New mix from a VocalsFirst hit; timeline should show three clips (A, B, A) and two mashup transitions when a return phrase is found.

## How to A/B

```sh
# before (upstream main) vs this branch
cargo test -p clubcraft-core mashup_ -- --nocapture
cargo test -p clubcraft-analysis mashup_ -- --nocapture
cargo test -p clubcraft-app mashup_exchange -- --nocapture
```

Export the same pair from Studio → WAV, listen at the instrumental swap and at phrase edges.
Compare against the criteria in the plan: no double-vocal, no double-kick through the phrase, phrase on quiet bars, return to A when reciprocal exists.
