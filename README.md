# Speiser_Lehmer_identity — verification suite

Reproducibility package for

> M. Ismail, *Tight zero pairs of the Riemann zeta function at height
> 8.4·10⁹: a Speiser–Lehmer identity, a displacement law, Euler mediation of
> the floor, and an X-free certificate*, 2026.

Every numerical claim of the paper — every table, every measured constant,
and the certified bounds of Section 9 — is recomputed by `verify_all.py`
(test groups **T1–T22, 124 checks, all passing**), independently of the text,
with pass/fail criteria stated in the script.

## 1. Requirements and use

Python ≥ 3.10 with `numpy`, `scipy`, `mpmath` (1.3.0). No network access is
required; all input data ship in `data/`.

```
python3 verify_all.py          # T1–T22, ~3 minutes single-core
python3 verify_all.py --slow   # adds the extended Monte-Carlo tiers
```

The data root is `$RH_DATA` (default: `./data`).

## 2. Map: paper → test groups

| Paper location | Content | Groups |
|---|---|---|
| §2, Tables 2.1–2.2 | ensembles; error budget (phase error 2.24·10⁻¹⁰ at the floor scale) | T1 |
| §3, Table 3.1; Lemma D, Theorem A, Cors A.1–A.3 | Speiser–Lehmer identity r²(1+1/(8C_mid)) = 1; detector monotonicity | T8–T10 |
| §4, Fig. 4.1; Theorem B, Cor B.1 | displacement law δ = (g²/8)λ_far; 15-row verification, corr 0.999967 | T7 |
| §5, Tables 5.1–5.2, Fig. 5.1; Empirical Law 1, Lemma B′.1, Thms B′(a)/(b), Cor B′.2 | floor fits (a, b, R² = 0.951); fold analysis; g_app = √(g²−4h₀²) | T6, T6X |
| §6, Tables 6.1–6.2, Fig. 6.1 | mediation: partial correlations 0.027 vs 0.873; Euler deficit −3.13σ at X = 10⁶ | T11–T13 |
| §7, Tables 7.1–7.3, Fig. 7.1; Theorem E | far-field law 2.5·dens²; shield 1.354/1.360; GUE small-gap tails, C_GUE = 4π²/15 | T2–T5, T14–T15 |
| §8, Table 8.1; Proposition 8.1 | median per-pair margin; novelty ledger | T22 (sweep 2736/2736) |
| §9, Tables 9.1–9.3; Lemma 9.1 (Δ₀ = 0.2387), Cors 9.1′/9.2 + Rem 9.2′, Lemmas 9.3/9.4/9.7, Thms 9.5/9.8/9.9, Prop 9.6 | the X-free certificate; certified Chernoff table; union bound | T16–T21 |
| Appendix B, Table B.1 | displacement verification, 15 tightest pairs | T7 |
| Appendix C | full-ensemble detector sweep; out-of-sample row at t ≈ 7005 (1.0 %) | T22 |

## 3. `pipeline/` — provenance of the derived data

The nine scripts that produced the derived arrays in `data/` from the raw
certified zeros, shipped for full provenance (they are **not** needed to run
`verify_all.py`): `parta_compute.py`, `parta_analyze.py` (floors, L_X, fits),
`partb_eiv.py` (errors-in-variables correction), `gen_disp_table.py`
(Appendix B), `smooth_cutoff_scan.py` (§10.3 smooth-weight scan),
`scan_floors.py`, `compute_lx.py`, `analyze_moments.py` (§6),
`gue_mc.py` (the GUE Monte-Carlo reference of §7).

## 4. Data provenance

`data/lmfdb_zeros_parsed.npy` — 772 719 certified ordinates on
[8.436146·10⁹, 8.436377·10⁹] (Platt / LMFDB, certified to ±2.5·10⁻³¹);
`zeros1.gz`, `zeros6.gz` — Odlyzko's tables near heights 10⁰ and 10⁶;
the remaining 33 files are derived by `pipeline/` and shipped so that
`verify_all.py` runs without recomputation. `SHA256SUMS.txt` covers every
file in the package. Conventions: tight pair s < 0.15; floor = max|Z| over
the gap (33-point grid + parabolic refinement); L_X phases in 80-bit
long double.
