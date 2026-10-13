# Receiver Calculator

Room XIII of the Fractal Youniverse collection — live at **https://fractalyouniverse.org/Receiver_calculator/**
(mirror: https://travisbergen2.github.io/Receiver_calculator/).

An exact math engine does the arithmetic (BigInt rationals, a parser, a solver and a
verifier — no `eval`). The RPCS-1 receiver (FT → TI → AR → SG, with UE governing change)
decides which interpretation of the input to commit to, when to ask instead, and how to
adapt from your feedback. Every parameter update, UE change and slot switch is logged with
its reason.

Built by Travis Bergen (October 2026). Companion to Room XII, *Receiver Life*.

## Two branches

- **Math** — goes to the exact engine. Precedence modes: standard, compare with
  left-to-right, no rule assumed. Works everywhere.
- **Gaussian mode** — `(2+i)^2`, `(3+2i)^2`, the square of `12+i`, or `7` read as `7+i`:
  shows z², its triangle, the mirror vectors, and the primes in the hypotenuse with their
  two-square and root-pair forms. Exact; bypasses the receiver. Works everywhere.
- **Any other question** — Claude lists 1–4 readings with probabilities (without answering),
  the receiver chooses one or holds them open, and only then is the chosen reading answered.
  **This branch needs the page to be opened inside Claude** (it uses the claude.ai artifact
  hook, not a network call). On this site the page detects that and shows *math only here*;
  nothing breaks.

## Rules in this build (provisional ones marked)

- **FT** admits a candidate if its plausibility p ≥ 0.6 · FT_eff · max p. Invalid parses are never
  admitted. Lowered when the intended reading was filtered out; raised when a higher threshold would
  have rejected a wrong pick and kept the right one. *Provisional.*
- **TI** sets the memory decay λ = 0.3 + 0.65 · TI_eff, so lower TI = shorter window = faster response
  (same convention as Receiver Life). Adjusted by comparing short (λ=0.3) and long (λ=0.95) window
  predictions. *Provisional.*
- **AR** scores admitted candidates by ln p + integrated evidence, and commits only if the top score
  leads by margin m = 0.35 · (0.5 + Θ of the + slot). Otherwise it reports the ambiguity and asks.
  AR has no parameter of its own.
- **SG** sets the confidence gain (0.5 + 3 · SG_eff) and the weight of evidence written to memory
  (0.5 + SG_eff). Adjusted for calibration (confidence vs outcome). Confidence never overrides the
  verifier. *Provisional.*
- **UE** sets the adaptation step η = 0.15 · UE_eff and the switch-attempt probability 0.5 · UE_eff.
  UE itself changes only by: drive = (ē − 0.25); ΔUE = 0.15 · drive · (1 − UE) if drive > 0, else
  0.15 · drive · UE; minus 0.15 · 0.3 · instability · UE. *Provisional hypothesis.*
- **Slots**: each parameter's data channel updates Θ of the slot it occupies. Θ biases the occupant's
  effective value by 0.3 · (Θ − ½). Q[k][j] tracks reward when parameter j sits in slot k; reassignment
  chooses among "stay" and the six transpositions by softmax on ΔQ (τ = 0.08).
- The numeric meaning of the `−` slots is not yet specified: *Labels only* is the default; the
  *sign scales occupant* option is an unvalidated guess and is marked experimental in the page.

## Running it

Open `index.html` in any modern browser. Self-contained: one external request (the Google Fonts
stylesheet), no analytics, no network calls. The pinned context and the receiver state are kept in
the browser's local storage only; *Reset* clears the receiver state.

## What this is and is not

An RPCS-1 toy that makes the commit-or-ask decision visible on exact arithmetic. The receiver rules are
the author's design choices (marked provisional above), not fitted to data; nothing here measures humans
or models, and nothing here bears on any mathematical conjecture. The receiver primitives are defined in
IMM Paper 9 (https://doi.org/10.5281/zenodo.19697792).

## Provenance

`index.html` sha256 `b66993711a8b5c10…` (full hash in the publish log, 2026-10-13).
