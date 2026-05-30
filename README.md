# spiking-aeon — Package 26

**Neuromorphic SNN Hardware Bridge (Intel Loihi 2)** — GenesisAeon Entropy Atlas Package 26

[![CI](https://github.com/GenesisAeon/spiking-aeon/actions/workflows/ci.yml/badge.svg)](https://github.com/GenesisAeon/spiking-aeon/actions/workflows/ci.yml)
[![Python 3.11+](https://img.shields.io/badge/python-3.11%2B-blue)](https://www.python.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.19645351.svg)](https://doi.org/10.5281/zenodo.19645351)
[![Package 26](https://img.shields.io/badge/GenesisAeon-Package%2026-blueviolet)](https://github.com/GenesisAeon/spiking-aeon)
[![NeuEdge](https://img.shields.io/badge/arXiv-2602.02439-red)](https://arxiv.org/abs/2602.02439)

Bridges the GenesisAeon CREP weight system to physical Spiking Neural Network
hardware (Intel Loihi 2). CREP tensor components C, R, E, P are translated into
LIF neuron parameters. Software simulation runs without hardware via the pure-Python
Brian2-compatible backend.

Calibrated against **NeuEdge (arXiv:2602.02439, 2026)**:
847 GOp/s/W · 2.3 ms latency · 89% core utilisation · 312x energy improvement over GPU.

---

## SNN UTAC Model

| Parameter | Value | Meaning |
|-----------|-------|---------|
| **K** | 1000 Hz | Max sustainable Loihi 2 firing rate |
| **H*** | 320 Hz | Critical firing rate for task performance |
| **eta** | 0.32 | H*/K — SNN operating setpoint |
| **Gamma** | ~0.150 | arctanh(0.32) / 2.2 — SNN hardware criticality index |

### CREP -> LIF Parameter Mapping

| CREP | LIF Parameter | High value effect |
|------|---------------|-------------------|
| **C** | tau_m (membrane time constant) | Longer integration window |
| **R** | I_noise amplitude | Optimal stochastic resonance |
| **E** | Synaptic weight W | Stronger collective coupling |
| **P** | tau_ref (refractory period) | Richer temporal coding |

---

## Install

```bash
pip install spiking-aeon
# or
uv add spiking-aeon
```

---

## Usage

```python
from spiking_aeon.system import SpikingAeon

sim = SpikingAeon(n_neurons=1000, seed=42)

# Software simulation (no hardware required)
result = sim.run_cycle(duration_ms=1000.0)
print(f"Mean firing rate: {result['mean_rate_hz']:.1f} Hz")

# CREP criticality tensor
crep = sim.get_crep_state()
print(f"Gamma = {crep['Gamma']:.4f}  (reference: {crep['Gamma_ref']})")

# UTAC state
utac = sim.get_utac_state()
print(f"H = {utac['H']:.4f}, below threshold: {utac['below_threshold']}")

# Zenodo record
record = sim.to_zenodo_record()
```

### Stochastic Resonance

```python
sr = sim.optimise_stochastic_resonance(n_steps=20)
print(f"Optimal noise D_res = {sr['D_res']:.3f}  (min CV = {sr['min_CV']:.3f})")
```

### Deploy to Loihi 2

```python
# Requires INRC access + pip install lava-nc
# https://www.intel.com/content/www/us/en/research/neuromorphic-computing.html
sim.deploy_to_loihi()
```

---

## Package Structure

```
src/spiking_aeon/
├── __init__.py              # version 0.1.0, gamma=0.150, package_number=26
├── constants.py             # Loihi 2 specs, LIF params, UTAC constants
├── lif_neuron.py            # LIF neuron with CREP-modulated firing threshold
├── stdp_plasticity.py       # Spike-Timing Dependent Plasticity
├── stochastic_resonance.py  # Optimal noise sweep (Ferreira 2025)
├── crep_to_weights.py       # CREP {C,R,E,P} -> {tau_m, noise, weight, tau_ref}
├── crep_snn.py              # SNN CREP tensor, Gamma_ref ~= 0.150
├── brian2_backend.py        # Pure-Python LIF network simulation
├── loihi_adapter.py         # Lava SDK interface (requires INRC)
├── system.py                # Diamond interface (run_cycle, get_crep_state, ...)
└── benchmark.py             # NeuEdge 2026 benchmark suite
```

---

## CREP Criticality Spectrum Position

```
Domain                       Pkg   Gamma    eta
---------------------------  ----  -------  ----
Qubit decoherence (T2)       P24   0.050    ~5%
Apoptosis (ATP threshold)    P25   0.090    20%
Amazon Rainforest            P19   0.116    12%
SNN firing (Loihi 2)         P26   0.150    32%   <- spiking-aeon
Seismic b=1.5 (GR law)       P23   0.200    40%
AMOC / Neural criticality    P20   0.251    50%
BTW Sandpile (SOC)           P22   0.296    58%
```

---

## BibTeX

```bibtex
@software{romer2026spiking_aeon,
  author    = {Romer, Johann},
  title     = {spiking-aeon: Neuromorphic SNN Hardware Bridge (Package 26)},
  year      = {2026},
  version   = {0.1.0},
  publisher = {Zenodo},
  doi       = {10.5281/zenodo.19645351},
}

@misc{neuredge2026,
  title  = {NeuEdge: Energy-Efficient Neuromorphic Edge Computing},
  year   = {2026},
  eprint = {2602.02439},
  note   = {847 GOp/s/W on Loihi 2},
}
```
