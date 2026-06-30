# Radio Channel Sounding and Multipath Propagation Analysis


# Project Overview:

This project presents a complete RF/wireless channel-modeling and propagation-analysis portfolio project focused on **5.25 GHz radio-channel sounding, multipath propagation, link-budget analysis, delay-domain statistics, and angular-domain algorithm validation**.
The first part focuses on first-principles RF link-budget analysis, including EIRP calculation, free-space path loss, antenna gains, cable losses, attenuator loss, ASU insertion loss, LNA gain, and explicit received-power reference-plane separation. The second part focuses on channel-processing algorithms, including channel impulse response modeling, power delay profile extraction, RMS delay-spread estimation, coherence-bandwidth calculation, and scenario-wise LoS, diffraction-rich NLoS, and reflection-rich NLoS interpretation.
The third part focuses on angular-domain algorithm validation using a uniform linear array signal model, spatial covariance estimation, Bartlett beamforming, and MUSIC super-resolution processing on simulated ULA snapshots.
The purpose of this project is to demonstrate practical RF/wireless system-engineering capability, not only theoretical familiarity with channel models. The repository includes the technical report, MATLAB/Python/C source files, generated figures, validation results, and documentation explaining how the simulations were performed and interpreted.

---
# Topics Covered:

•	5.25 GHz radio-channel sounding analysis
•	Multipath propagation modeling
•	First-principles RF link-budget calculation
•	EIRP, free-space path loss, and Friis relation
•	Antenna-input and post-LNA received-power reference planes
•	ITU single-knife-edge diffraction-loss modeling
•	Channel impulse response modeling
•	Power delay profile extraction
•	Mean excess delay calculation
•	RMS delay-spread estimation
•	Coherence-bandwidth estimation
•	LoS, diffraction-rich NLoS, and reflection-rich NLoS propagation profiles
•	Uniform linear array signal model
•	Spatial covariance estimation
•	Bartlett beamformer validation
•	MUSIC super-resolution algorithm validation
•	Delay-domain and angular-domain channel-statistics interpretation
•	MATLAB, Python, and C reference implementations
•	Algorithm complexity analysis
•	Engineering result traceability

---

# Project Workflow:

The project follows a complete wireless-channel analysis and algorithm-validation flow:

```text
5.25 GHz Channel Scenario Definition
        ↓
RF Link-Budget Parameter Setup
        ↓
EIRP / FSPL / Diffraction-Loss Calculation
        ↓
Received-Power Reference-Plane Separation
        ↓
Channel Impulse Response Modeling
        ↓
Power Delay Profile Extraction
        ↓
Mean Excess Delay and RMS Delay Spread
        ↓
Coherence-Bandwidth Estimation
        ↓
LoS / Diffraction-Rich / Reflection-Rich Scenario Comparison
        ↓
ULA Snapshot Generation for Algorithm Validation
        ↓
Spatial Covariance Estimation
        ↓
Bartlett Beamformer Validation
        ↓
MUSIC Super-Resolution Validation
        ↓
Algorithm Complexity Summary
        ↓
Final Technical Report and Result Interpretation
```

---

## Tools Used:

| Tool | Purpose |
|---|---|
| MATLAB | Main simulation, channel-analysis pipeline, figure generation, PDP analysis, delay statistics, Bartlett/MUSIC validation |
| Python | Reference numerical validation and deterministic cross-checking of selected processing blocks |
| C | Lightweight implementation for received-power and delay-statistics checks |
| RF theory / hand calculation | EIRP, FSPL, Friis relation, diffraction loss, delay-spread equations, coherence-bandwidth approximation |
| Channel-sounding theory | CIR, PDP, delay-domain statistics, multipath interpretation |
| Array signal processing | ULA steering vector, covariance matrix, Bartlett spectrum, MUSIC pseudo-spectrum |
| Technical documentation | Report writing, result interpretation, traceability, and portfolio presentation |

---

# Repository Structure:

```text
01_radio_channel_sounding_multipath_analysis/
│
├── README.md
│
├── docs/
│   └── Radio_Channel_Sounding_Multipath_Propagation_Analysis.pdf
│
├── figures/
│   ├── figure_1_channel_sounding_architecture.png
│   ├── figure_2_processing_pipeline.png
│   ├── figure_7_validation_pdp_profiles.png
│   ├── delay_coherence_angular_spread_corrected.png
│   └── bartlett_music_validation.png
│
├── src/
│   ├── matlab/
│   │   └── channel_analysis.m
│   │
│   ├── python/
│   │   └── channel_analysis.py
│   │
│   └── c/
│       └── channel_analysis.c
│
├── results/
│   └── validation_summary.md
│
└── notes/
    ├── algorithm_complexity_summary.md
    └── result_traceability.md
```
---

## Key Results:

| Scenario | RMS Delay Spread | Coherence BW at 0.5 Level | Angular Spread | Interpretation |
|---|---:|---:|---:|---|
| LoS | 2.54 ns | 78.7 MHz | 0.29 rad | Narrow dominant-path channel |
| Diffraction-rich NLoS | 14.24 ns | 14.0 MHz | 0.29 rad | Moderate delay dispersion |
| Reflection-rich NLoS | 23.38 ns | 8.6 MHz | 0.88 rad | Rich dispersive multipath channel |

The results show the expected physical progression from a narrow, dominant-path channel to a richer dispersive multipath channel. As RMS delay spread increases, coherence bandwidth decreases, indicating stronger frequency selectivity.

---

# RF Link-Budget and Reference-Plane Analysis:

A first-principles link-budget calculation was performed to estimate the received power at clearly defined RF reference planes. The transmit-side reference power is converted into effective isotropic radiated power as

```math
\mathrm{EIRP} = P_T + G_T - L_T - L_{\mathrm{att}}
```

where \(P_T\) is the transmitter output power, \(G_T\) is the transmit-antenna gain, \(L_T\) is the transmit-chain loss, and \(L_{\mathrm{att}}\) is the calibrated attenuator loss.

The antenna-input received power is calculated as

```math
P_{\mathrm{RX,in}} =
\mathrm{EIRP} - L_f - L_d + G_R - L_R - L_{\mathrm{ASU}}
```

where \(L_f\) is the free-space path loss, \(L_d\) is the diffraction loss, \(G_R\) is the receive-antenna gain, \(L_R\) is the receive-chain loss, and \(L_{\mathrm{ASU}}\) is the antenna-switching-unit insertion loss.

The post-LNA received power is

```math
P_{\mathrm{RX,LNAout}} =
P_{\mathrm{RX,in}} + G_{\mathrm{LNA}}
```

For the LoS reference case:

| Reference Plane | Received Power |
|---|---:|
| Antenna-input reference plane | approximately −54.04 dBm |
| Post-LNA reference plane | approximately −29.04 dBm |

This reference-plane separation prevents antenna-terminal received power from being confused with receiver-chain amplified power.

---

# Channel Impulse Response and Power Delay Profile

The channel is represented using a discrete-time complex channel impulse response,

```math
h_m(\tau_k,t_l)
```

where \(m\) is the receive branch index, \(\tau_k\) is the delay tap, and \(t_l\) is the snapshot index.

The power delay profile is estimated by averaging the squared magnitude of the complex CIR over receive branches and snapshots:

```math
P(\tau_k) =
\frac{1}{N_r L}
\sum_m \sum_l
\left|h_m(\tau_k,t_l)\right|^2
```

A noise gate is applied below the PDP peak to remove weak delay taps dominated by the noise floor. The retained PDP taps are used for delay-statistics extraction.

---

# Delay-Statistics Estimation:

The gated PDP is normalized into delay-domain weights:

```math
w_k =
\frac{P(\tau_k)}
{\sum_i P(\tau_i)}
```

The mean excess delay is calculated as

```math
\bar{\tau} =
\sum_k w_k \tau_k
```

The RMS delay spread is calculated as

```math
\sigma_\tau =
\sqrt{
\sum_k w_k \tau_k^2 - \bar{\tau}^2
}
```

The coherence bandwidth is approximated as

```math
B_c^{0.5} \approx \frac{1}{5\sigma_\tau}
```

and

```math
B_c^{0.9} \approx \frac{1}{50\sigma_\tau}
```

These parameters summarize the delay dispersion and frequency selectivity of the channel. A small RMS delay spread indicates a narrow dominant-path channel, while a larger RMS delay spread indicates stronger multipath dispersion and smaller coherence bandwidth.

---

# Diffraction-Loss Modeling:

The diffraction-rich NLoS case includes ITU single-knife-edge diffraction loss. The diffraction parameter is used to estimate excess propagation loss caused by an obstruction between the transmitter and receiver.

The ITU-style knife-edge excess loss is modeled as an additional propagation loss \(L_d\), which is subtracted in the received-power equation:

```math
P_{\mathrm{RX,in}} =
\mathrm{EIRP} - L_f - L_d + G_R - L_R - L_{\mathrm{ASU}}
```

For the LoS reference case, \(L_d = 0\). For the diffraction-rich NLoS case, \(L_d\) is positive and increases the total path loss.

---

# Angular-Domain Processing:

The angular-domain validation uses simulated ULA snapshots. The array snapshot matrix is

```math
X \in \mathbb{C}^{N \times L}
```

where \(N\) is the number of ULA elements and \(L\) is the number of snapshots.

The spatial covariance matrix is estimated as

```math
R =
\frac{1}{L}XX^H
```

where \((\cdot)^H\) denotes the Hermitian transpose.

The covariance matrix provides the basis for Bartlett beamforming and MUSIC subspace-based direction-of-arrival estimation.

---

# Bartlett Beamformer Validation:

The Bartlett beamformer is a conventional power-scan estimator. For each candidate angle \(\theta\), the ULA steering vector \(a(\theta)\) is applied to the spatial covariance matrix \(R\). The Bartlett spectrum is

```math
P_B(\theta) =
\frac{
a^H(\theta) R a(\theta)
}{
a^H(\theta)a(\theta)
}
```

Bartlett beamforming is robust and simple, but its angular resolution is limited by the array aperture. For a ULA with half-wavelength element spacing, closely spaced arrivals may merge into one broad lobe.

---

# MUSIC Super-Resolution Validation:

MUSIC is a subspace-based direction-of-arrival estimator. After eigendecomposition of the covariance matrix, the noise-subspace matrix \(E_n\) is formed from the eigenvectors associated with the smallest eigenvalues.

The MUSIC pseudo-spectrum is

```math
P_{\mathrm{MUSIC}}(\theta) =
\frac{1}
{
a^H(\theta)E_nE_n^Ha(\theta)
}
```

MUSIC forms sharp peaks when the candidate steering vector is nearly orthogonal to the noise subspace.

In this project, MUSIC is used as an algorithm-validation module using simulated ULA snapshots. The resulting DoA peaks should not be interpreted as measured DoA results unless calibrated complex array measurements are added in future work.

---

# Scenario Comparison:

The three propagation profiles demonstrate how delay dispersion and coherence bandwidth change with multipath richness.

| Scenario | Channel Behavior | Delay-Domain Effect | Frequency-Domain Effect |
|---|---|---|---|
| LoS | Dominant direct path | Small RMS delay spread | Large coherence bandwidth |
| Diffraction-rich NLoS | Obstructed propagation with delayed components | Moderate RMS delay spread | Reduced coherence bandwidth |
| Reflection-rich NLoS | Rich multipath with stronger delayed components | Largest RMS delay spread | Smallest coherence bandwidth |

The trend

```text
RMS delay spread: 2.54 ns → 14.24 ns → 23.38 ns
```

and

```text
Coherence bandwidth: 78.7 MHz → 14.0 MHz → 8.6 MHz
```

confirms the expected inverse relationship between delay spread and coherence bandwidth.

---

# Validation Methodology:

The project uses simulation-based algorithm validation and deterministic numerical cross-checking.

| Validation Area | Method | Outcome |
|---|---|---|
| Link budget | Hand calculation and code implementation | Reference-plane powers reproduced consistently |
| PDP extraction | Controlled CIR scenario profiles | Delay taps and relative power levels extracted |
| Delay statistics | PDP moment calculation | RMS delay spread and coherence bandwidth estimated |
| Diffraction loss | ITU knife-edge model | Excess NLoS path loss included |
| Bartlett beamforming | Simulated ULA snapshots | Conventional angular spectrum validated |
| MUSIC | Simulated ULA snapshots and covariance eigendecomposition | Subspace pseudo-spectrum validated |
| Cross-language check | MATLAB, Python, and C implementations | Core numerical blocks verified |

---

# MATLAB, Python, and C Implementation:

The MATLAB implementation is the main report-aligned simulation and visualization pipeline. It performs link-budget calculation, scenario-profile generation, PDP analysis, delay-statistics extraction, figure generation, and optional Bartlett/MUSIC validation.

The Python implementation provides reference numerical validation for selected processing blocks.

The C implementation provides a lightweight baseline for received-power and delay-statistics checks. This is useful for understanding how the low-complexity parts of the algorithm could be moved toward embedded or real-time receiver-oriented implementation





