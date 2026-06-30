# 01 - Radio Channel Sounding and Multipath Propagation Analysis

# Project Overview:

This project presents a complete RF/wireless channel-modeling portfolio project focused on radio-channel sounding, multipath propagation analysis, link-budget calculation, delay-domain statistics, and angular-domain algorithm validation at 5.25 GHz. This is a first-principles link-budget and simulation-based algorithm-validation project
The project connects first-principles RF propagation analysis with simulation-based channel-processing algorithms. The first part focuses on deterministic received-power calculation, including EIRP, free-space path loss, antenna gains, cable losses, attenuator loss, ASU insertion loss, LNA gain, and ITU single-knife-edge diffraction loss. The second part focuses on wireless-channel characterization, including channel impulse response modeling, power delay profile extraction, mean excess delay, RMS delay spread, coherence bandwidth, angular spread, spatial covariance, Bartlett beamforming, and MUSIC algorithm validation.

The purpose of this project is to demonstrate practical RF/wireless engineering capability, not only theoretical knowledge of propagation models. The repository includes the technical report, MATLAB/Python/C source files, generated result figures, and documentation explaining how the simulations were performed and interpreted.

This project is relevant to **5G NR / 6G wireless-channel modeling**, OFDM frequency-selectivity analysis, MIMO channel interpretation, link-budget engineering, and beamforming-oriented propagation studies.

# Topics Covered:

• 5.25 GHz radio-channel sounding analysis  
• First-principles RF link-budget calculation  
• Effective isotropic radiated power calculation  
• Free-space path loss analysis  
• ITU single-knife-edge diffraction-loss modeling  
• Antenna-input and post-LNA received-power reference planes  
• Channel impulse response modeling  
• Power delay profile estimation  
• Mean excess delay calculation  
• RMS delay-spread estimation  
• Coherence-bandwidth approximation  
• LoS, diffraction-rich NLoS, and reflection-rich NLoS propagation profiles  
• Uniform linear array signal model  
• Spatial covariance estimation  
• MDL/AIC source-count interpretation  
• Bartlett beamformer validation  
• MUSIC super-resolution algorithm validation  
• Scenario-wise angular-spread interpretation  
• MATLAB, Python, and C reference implementations  
• Algorithm complexity analysis  
• Engineering result traceability  

# Project Workflow:

The project follows a complete RF/wireless channel-analysis flow:

Known Wideband Channel-Sounding Model  
        ↓  
RF Link-Budget Reference-Plane Definition  
        ↓  
EIRP, FSPL, Antenna Gain, Cable Loss, ASU Loss, and LNA Gain Calculation  
        ↓  
ITU Knife-Edge Diffraction-Loss Modeling  
        ↓  
Channel Impulse Response Scenario Construction  
        ↓  
Power Delay Profile Extraction  
        ↓  
Mean Excess Delay and RMS Delay Spread Calculation  
        ↓  
Coherence-Bandwidth Estimation  
        ↓  
ULA Snapshot Generation for Angular Validation  
        ↓  
Spatial Covariance Estimation  
        ↓  
Bartlett Beamformer Angular Spectrum  
        ↓  
MUSIC Pseudo-Spectrum Validation  
        ↓  
Scenario-Wise Delay and Angular Statistics  
        ↓  
MATLAB / Python / C Cross-Validation  
        ↓  
Final Technical Report and Result Interpretation  

## Tools Used:

| Tool | Purpose |
|---|---|
| MATLAB | Main simulation pipeline, link-budget calculation, PDP generation, delay-statistics extraction, plotting, Bartlett/MUSIC validation |
| Python | Reference implementation for deterministic numerical cross-checking |
| C | Lightweight implementation for received-power and delay-statistics validation |
| RF propagation theory | EIRP, FSPL, Friis relation, diffraction loss, link-budget reference-plane calculation |
| Wireless channel theory | CIR, PDP, RMS delay spread, coherence bandwidth, angular spread, MIMO channel-model interpretation |
| Array signal processing | ULA steering vectors, spatial covariance, Bartlett beamforming, MUSIC subspace processing |
| Technical documentation | Report writing, result interpretation, traceability, and portfolio presentation |

# Repository Structure:

```text
01_radio_channel_sounding_multipath_analysis/
│
├── README.md
│
├── docs/
│   └── Radio_Channel_Sounding_Multipath_Propagation_Analysis.pdf
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
├── figures/
│   ├── figure_1_channel_sounding_architecture.png
│   ├── figure_2_processing_pipeline.png
│   ├── figure_7_validation_pdp_profiles.png
│   ├── delay_coherence_angular_spread_corrected.png
│   └── bartlett_music_validation.png
│
├── results/
│   └── validation_summary.md
│
└── notes/
    ├── algorithm_complexity_summary.md
    └── result_traceability.md
```

## Key Results:

| Scenario | RMS Delay Spread | Coherence BW at 0.5 Level | Angular Spread | Interpretation |
|---|---:|---:|---:|---|
| LoS | 2.54 ns | 78.7 MHz | 0.29 rad | Narrow dominant-path channel |
| Diffraction-rich NLoS | 14.24 ns | 14.0 MHz | 0.29 rad | Moderate delay dispersion |
| Reflection-rich NLoS | 23.38 ns | 8.6 MHz | 0.88 rad | Rich dispersive multipath channel |

The results show the expected transition from a narrow dominant-path channel to a richer dispersive multipath channel. As RMS delay spread increases, coherence bandwidth decreases, indicating stronger frequency selectivity.

# RF Link-Budget and Reference-Plane Analysis:

The received-power calculation keeps the RF reference plane explicit. This is important because antenna-input received power and receiver-chain amplified power are not the same physical quantity.

The effective isotropic radiated power is calculated as

```math
\mathrm{EIRP} = P_T + G_T - L_T - L_{\mathrm{att}}
```

The received power at the antenna-input reference plane is calculated as

```math
P_{\mathrm{RX,in}} =
\mathrm{EIRP} - L_f - L_d + G_R - L_R - L_{\mathrm{ASU}}
```

The received power at the post-LNA reference plane is calculated as

```math
P_{\mathrm{RX,LNAout}} =
P_{\mathrm{RX,in}} + G_{\mathrm{LNA}}
```

# For the LoS reference case:

| Reference Plane | Received Power |
|---|---:|
| Antenna-input reference plane | approximately −54.04 dBm |
| Post-LNA reference plane | approximately −29.04 dBm |

The 25 dB difference corresponds to the LNA gain. Keeping these two values separate prevents antenna-terminal received power from being confused with receiver-chain amplified power.

# ITU Knife-Edge Diffraction Modeling:

The diffraction-rich NLoS scenario includes an ITU single-knife-edge excess-loss model. The diffraction parameter is evaluated from the transmitter-obstacle, obstacle-receiver, wavelength, and obstruction geometry.

The excess diffraction loss is included as an additional positive propagation loss:

```math
P_{\mathrm{RX,in}} =
\mathrm{EIRP} - L_f - L_d + G_R - L_R - L_{\mathrm{ASU}}
```

where \(L_d = 0\) for the LoS reference case and positive for the diffraction-dominated NLoS case.

This separates free-space spreading loss from additional obstacle-induced diffraction loss.

# Channel Impulse Response and PDP Processing:

The channel impulse response is represented in delay as

```math
h(\tau) = \sum_{p=1}^{N_p} \alpha_p \delta(\tau - \tau_p)
```

where \(\alpha_p\) is the complex gain of the \(p\)-th path and \(\tau_p\) is its delay.

The power delay profile is estimated from the complex CIR as

```math
P(\tau_k) =
\frac{1}{N_rL}
\sum_m \sum_l
|h_m(\tau_k,t_l)|^2
```

where \(N_r\) is the number of receive branches, \(L\) is the number of snapshots, and \(N_t\) is the number of delay taps.

The PDP is then gated to remove noise-dominated delay taps before extracting delay statistics.

# Delay-Statistics Estimation:

The gated PDP samples are normalized as

```math
w_k =
\frac{P(\tau_k)}
{\sum_i P(\tau_i)}
```

The mean excess delay is

```math
\bar{\tau} =
\sum_k w_k \tau_k
```

The RMS delay spread is

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

These parameters summarize the delay dispersion and frequency-selective behavior of the propagation channel.

# Scenario-Based Multipath Analysis:

Three propagation profiles are analyzed:

| Scenario | Propagation Character |
|---|---|
| LoS | Dominant direct ray with small delay spread |
| Diffraction-rich NLoS | Delayed components caused by obstacle diffraction |
| Reflection-rich NLoS | Richer multipath with larger delay and angular spread |

The LoS profile has the smallest RMS delay spread and largest coherence bandwidth. The reflection-rich profile has the largest RMS delay spread and smallest coherence bandwidth. This matches the expected physical behavior of multipath channels.

# Angular-Domain Processing:

The angular-domain validation uses a uniform linear array model. The array snapshot matrix is

```math
X \in \mathbb{C}^{N \times L}
```

where \(N\) is the number of ULA elements and \(L\) is the number of snapshots.

The spatial covariance matrix is estimated as

```math
R = \frac{1}{L}XX^H
```

The covariance model can be written as

```math
R = ASA^H + \sigma_n^2 I
```

where \(A\) contains the steering vectors, \(S\) is the source covariance matrix, and \(\sigma_n^2 I\) represents spatially white noise.

# Bartlett Beamformer Validation:

The Bartlett beamformer is the conventional power-scan estimator. For each candidate angle \(\theta\), the ULA steering vector is applied to the covariance matrix.

The Bartlett angular spectrum is

```math
P_B(\theta) =
\frac{
a^H(\theta) R a(\theta)
}{
a^H(\theta)a(\theta)
}
```

Bartlett beamforming is robust and simple, but its angular resolution is limited by the array aperture. Therefore, closely spaced arrivals may merge into a single broad lobe.

# MUSIC Algorithm Validation:

MUSIC is a subspace-based direction-of-arrival estimator. After eigendecomposition of the spatial covariance matrix, the noise-subspace matrix \(E_n\) is formed from the eigenvectors associated with the smallest eigenvalues.

The MUSIC pseudo-spectrum is

```math
P_{\mathrm{MUSIC}}(\theta) =
\frac{1}
{
a^H(\theta)E_nE_n^Ha(\theta)
}
```

In this project, MUSIC is used as an algorithm-validation module using simulated ULA snapshots. The resulting DoA peaks should not be interpreted as measured DoA results unless calibrated complex array measurements are added.

# Algorithm Complexity Summary:

| Algorithm | Main Operation | Complexity |
|---|---|---:|
| Received-power reconstruction | PDP formation, gating, energy summation, reference-plane calculation | \(O(N_r L N_t)\) |
| Delay-statistics estimator | PDP normalization, mean delay, RMS delay spread, coherence bandwidth | \(O(N_t)\) |
| Spatial covariance estimation | \(R = \frac{1}{L}XX^H\) | \(O(N^2L)\) |
| Bartlett beamformer | Angular scan over candidate steering vectors | \(O(|\Theta|N^2)\) |
| MDL/AIC source-count estimation | Eigenvalue-based source-number selection after eigendecomposition | \(O(N)\) after EVD |
| MUSIC estimator | Eigendecomposition and angular pseudo-spectrum scan | \(O(N^3 + |\Theta|N^2)\) |

where:

| Symbol | Meaning |
|---|---|
| \(N_r\) | number of receive branches |
| \(L\) | number of snapshots |
| \(N_t\) | number of delay taps |
| \(N\) | number of ULA elements |
| \(|\Theta|\) | number of candidate scan angles |

# MATLAB, Python, and C Implementation:

The MATLAB implementation is the main report-aligned simulation and visualization pipeline. It computes the link budget, generates scenario-based CIR/PDP profiles, extracts delay statistics, estimates coherence bandwidth, and validates Bartlett/MUSIC angular-domain processing.

The Python implementation provides deterministic numerical cross-checks of the main processing blocks.

The C implementation provides a lightweight reference for received-power reconstruction and delay-statistics estimation. This is useful for embedded or real-time receiver-oriented development where lower-complexity processing is preferred.

# How to Run:

## MATLAB

```matlab
cd src/matlab
channel_analysis
```

## Python

```bash
cd src/python
python channel_analysis.py
```

## C

```bash
cd src/c
gcc channel_analysis.c -o channel_analysis -lm
./channel_analysis
```

On Windows with MinGW:

```bash
gcc channel_analysis.c -o channel_analysis.exe -lm
channel_analysis.exe
```

# Validation Summary:

The validation establishes three main points.
First, the link-budget implementation reproduces the expected received-power reference planes and keeps antenna-input and post-LNA powers separate.
Second, the delay-statistics estimator produces a physically ordered progression across the three scenario profiles:
```text
2.54 ns → 14.24 ns → 23.38 ns
```

At the same time, the coherence bandwidth decreases:

```text
78.7 MHz → 14.0 MHz → 8.6 MHz
```

This confirms the expected relationship between multipath richness, RMS delay spread, and coherence bandwidth.

Third, the angular-domain pipeline validates covariance formation, Bartlett beamforming, and MUSIC pseudo-spectrum processing using controlled simulated ULA snapshots.

# Figures:

Recommended figures included in this project:

| Figure | Description |
|---|---|
| `figure_1_channel_sounding_architecture.png` | Channel-sounding architecture |
| `figure_2_processing_pipeline.png` | Delay-, power-, and angular-domain processing pipeline |
| `figure_7_validation_pdp_profiles.png` | Normalized PDP profiles for LoS, diffraction-rich, and reflection-rich scenarios |
| `delay_coherence_angular_spread_corrected.png` | Scenario-wise RMS delay spread, coherence bandwidth, and angular spread |
| `bartlett_music_validation.png` | Bartlett and MUSIC angular-spectrum validation |

# Limitations and Future Work:

The main limitation of this project is that the angular-domain validation uses simulated ULA snapshots rather than directly processed calibrated complex array measurements. Processing measured array snapshots with full per-element amplitude and phase calibration would convert the Bartlett/MUSIC section from algorithm validation into measured DoA extraction.

A second limitation is that the project provides channel-sounding and multipath statistics for MIMO channel modeling, but it does not perform a full measured MIMO channel-matrix analysis with singular values, capacity, condition number, or measured spatial-correlation metrics.

Future work should include:

• calibrated measured complex ULA snapshots  
• real measured CIR datasets  
• uncertainty and repeatability analysis  
• repeated measurement locations  
• measured K-factor and Doppler extraction from time-varying channel envelopes  
• elevation-angle characterization using planar or cylindrical arrays  
• comparison against standardized channel models such as 3GPP TR 38.901  
• spatial smoothing for correlated multipath arrivals before MUSIC processing  

# Engineering Relevance:

This project demonstrates the following RF/wireless engineering skills:

• RF link-budget calculation  
• EIRP and received-power reference-plane analysis  
• Free-space path loss and diffraction-loss modeling  
• Channel impulse response interpretation  
• Power delay profile processing  
• Mean excess delay and RMS delay-spread estimation  
• Coherence-bandwidth analysis  
• LoS and NLoS multipath interpretation  
• Uniform linear array signal modeling  
• Spatial covariance estimation  
• Bartlett beamformer validation  
• MUSIC algorithm validation  
• MATLAB-based wireless-channel simulation  
• Python reference implementation  
• C-based lightweight numerical validation  
• Algorithm complexity analysis  
• 5G NR / 6G channel-modeling fundamentals  
• OFDM frequency-selectivity interpretation  
• MIMO and beamforming-oriented propagation analysis  

# Project Status:

Status: Completed  
Tool: MATLAB, Python, C  
Project Type: RF / Wireless Channel-Modeling Portfolio  
Main Focus: Radio-channel sounding, multipath propagation, link-budget analysis, delay-domain statistics, and angular-domain algorithm validation  

# Author

Md Moklesur Rahman | RF / Wireless / System Specification Engineer | LinkedIn: linkedin.com/in/md-moklesur-rahman-65a63962
