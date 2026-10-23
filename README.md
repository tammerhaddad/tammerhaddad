## Tammer Haddad

I build and run machine-learning systems on deliberately small hardware — one Razer Blade Stealth
acting as a home server, with a `GTX 1650 Ti` holding **4 GB** of VRAM and the CPU package capped
at **28 W** sustained. The point is not that it works. The point is that every number here is one
I measured, with the command sitting next to it, and that I take figures *down* when they stop
reproducing.

### [PhotoSearch](https://github.com/tammerhaddad/PhotoSearch) — semantic + keyword photo search

Replaces Immich's retired smart search. **26,624 photos** and **2,274 videos** indexed;
**26,624 of 26,624** photos carry an embedding; **25,952** carry a generated caption. One
embedding model, `ViT-L-16-SigLIP2-256`, and **0 cloud model calls** — every model runs on the box.

**3 model pipelines** (embed → caption → faces) run *sequentially*, one pass at a time, because
only one of them fits in 4 GB of VRAM at once. That constraint is the architecture, not a
footnote: the indexer is a resumable multi-pass job precisely so a single GPU can be the
bottleneck without being a failure mode.

Measured 2026-10-23 00:48 UTC against the live SQLite store, read-only.

### A long-form TTS quality harness — chunking, normalization, loudness, gated best-of-N

Same model, same reference clip, same seed; the only change is the pipeline around it. Character
error rate falls **0.359 → 0.102**. The useful finding is the loudness stage: the unnormalized
conditions clip past full scale, and the naturalness judge returns non-finite on all of them, so
peak and loudness handling is what makes the output *scoreable at all* — not merely louder. Word
error rate improves too, but the judge is itself unstable on badly degraded audio, so that
baseline is a range and I report it as one.

*Harness and benchmark are written up but not yet published.*

### A League of Legends match archive

**1,073,147** valid matches, **157.28 GiB**, collected by a rate-limited crawler and re-verified
2026-10-18 against the archive database.

*Local only; not published.*

---

Most of what I have published so far is coursework and scratch work, and it is labelled as such.
These three are the ones with measurements behind them.
