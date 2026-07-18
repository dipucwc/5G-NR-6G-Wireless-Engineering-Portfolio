# 5G NR/6G Downlink Physical-Layer Link Simulator

## Project Information

**Project type:** Wireless physical-layer simulation and verification  
**Technical area:** 5G NR, future 6G radio access, OFDM, MIMO, Massive MIMO, beamforming, channel estimation, equalization, and link-level performance analysis  
**Planned implementation:** MATLAB and Python, with optional Simulink integration  
**Project status:** Ongoing project — currently under development  
**Author:** Md Moklesur Rahman  
**Location:** Finland

## Project Status

This project is currently under active development.

The objective is to build a modular downlink physical-layer link simulator for studying 5G NR-oriented and future 6G-oriented transmission methods. The planned simulator will connect transmitter processing, wireless-channel modeling, receiver processing, numerical verification, and performance evaluation in one reproducible framework.

The source code, configuration files, verification tests, result tables, figures, and technical documentation will be added progressively. No completed implementation, final numerical result, or standards-conformance claim is made at the current stage.

## Overview

Modern 5G NR and future 6G wireless systems require flexible link-level simulators for evaluating waveform processing, multi-antenna transmission, reference-signal design, channel-estimation accuracy, receiver equalization, link adaptation, and spectral efficiency.

The planned simulator will model the main downlink processing stages between source bits and recovered receiver bits.

The framework is expected to include:

- payload-bit generation;
- CRC attachment;
- optional channel coding and rate matching;
- QAM modulation;
- spatial-layer mapping;
- resource-grid construction;
- pilot or DM-RS-like reference-signal insertion;
- OFDM modulation and demodulation;
- MIMO precoding and beamforming;
- frequency-selective MIMO channels;
- additive noise and optional RF impairments;
- channel estimation;
- ZF and MMSE equalization;
- symbol and bit recovery;
- BER, BLER, EVM, NMSE, throughput, and capacity analysis;
- Monte Carlo simulation;
- MATLAB and Python cross-verification.

The project is intended as an engineering research and verification platform. It will be 5G NR-oriented, but the first versions may use simplified processing blocks instead of a bit-exact implementation of every 3GPP option.

## Main Objectives

The main objectives are to:

1. design a modular downlink physical-layer simulation architecture;
2. implement reusable transmitter, channel, receiver, and metric modules;
3. support configurable OFDM numerology and resource allocation;
4. support SISO, MIMO, and Massive MIMO configurations;
5. compare multiple modulation and precoding methods;
6. evaluate ideal and estimated channel-state-information branches;
7. implement LS and optional LMMSE channel estimation;
8. compare ZF, MMSE, and gain-corrected MMSE equalization;
9. evaluate BER, BLER, EVM, NMSE, throughput, and spectral efficiency;
10. verify every major processing stage using deterministic tests;
11. cross-check selected scenarios in MATLAB and Python;
12. provide a foundation for future 6G-oriented algorithm studies.

## Planned Technical Scope

The simulator is expected to support:

- configurable FFT size;
- configurable subcarrier spacing;
- configurable cyclic prefix;
- configurable active subcarriers;
- configurable OFDM-symbol allocation;
- QPSK, 16-QAM, 64-QAM, and optional 256-QAM;
- configurable transmit and receive antenna counts;
- configurable spatial-layer count;
- SISO, MIMO, and Massive MIMO profiles;
- identity, codebook, and SVD-based precoding;
- pilot or DM-RS-like reference signals;
- AWGN and frequency-selective tapped-delay-line channels;
- ideal and estimated CSI;
- ZF and MMSE-family equalizers;
- adaptive Monte Carlo stopping;
- reproducible configuration and result export.

## Planned End-to-End Architecture

```text
Payload Bits
    ↓
CRC Attachment
    ↓
Optional Channel Coding and Rate Matching
    ↓
QAM Mapping
    ↓
Layer Mapping
    ↓
Resource-Grid and Reference-Signal Construction
    ↓
MIMO Precoding / Beamforming
    ↓
OFDM Modulation
    ↓
Frequency-Selective MIMO Channel + AWGN
    ↓
OFDM Demodulation
    ↓
Reference-Signal Extraction
    ↓
Channel Estimation
    ↓
ZF / MMSE Equalization
    ↓
QAM Demapping
    ↓
Optional Rate Recovery and Channel Decoding
    ↓
CRC Check
    ↓
BER / BLER / EVM / NMSE / Throughput / Capacity
```

## Mathematical Signal Model

### Layer-Domain Symbol Vector

For OFDM symbol index \(m\) and subcarrier index \(k\), the layer-domain symbol vector is

$$
\mathbf{s}[m,k]=
\begin{bmatrix}
s_1[m,k] &
s_2[m,k] &
\cdots &
s_L[m,k]
\end{bmatrix}^{T}.
$$

Here:

- \(L\) is the number of spatial layers;
- \(s_l[m,k]\) is the symbol transmitted on layer \(l\);
- \((\cdot)^T\) denotes transpose.

### MIMO Precoding

The layer-domain symbol vector is mapped to the transmit antennas using

$$
\mathbf{x}[m,k]=\mathbf{W}[k]\mathbf{s}[m,k].
$$

Here:

- \(\mathbf{W}[k]\) is the precoding matrix;
- \(\mathbf{x}[m,k]\) is the transmit-antenna-domain vector.

For a semi-unitary precoder,

$$
\mathbf{W}^{H}[k]\mathbf{W}[k]=\mathbf{I}_{L}.
$$

This condition supports equal-power comparison between precoding methods.

### Frequency-Domain MIMO Channel

The received vector is

$$
\mathbf{y}[m,k]=
\mathbf{H}[k]\mathbf{x}[m,k]
+
\mathbf{n}[m,k].
$$

After substituting the precoder,

$$
\mathbf{y}[m,k]=
\mathbf{H}[k]\mathbf{W}[k]\mathbf{s}[m,k]
+
\mathbf{n}[m,k].
$$

Here:

- \(\mathbf{H}[k]\) is the MIMO channel matrix;
- \(\mathbf{n}[m,k]\) is the complex noise vector;
- \((\cdot)^H\) denotes Hermitian transpose.

### Effective Channel

The precoded layer-domain effective channel is

$$
\mathbf{G}[k]=\mathbf{H}[k]\mathbf{W}[k].
$$

The received signal can therefore be written as

$$
\mathbf{y}[m,k]=
\mathbf{G}[k]\mathbf{s}[m,k]
+
\mathbf{n}[m,k].
$$

## OFDM Model

A simplified time-domain OFDM symbol is

$$
x[n]=
\frac{1}{\sqrt{N_{\mathrm{FFT}}}}
\sum_{k=0}^{N_{\mathrm{FFT}}-1}
X[k]e^{j2\pi kn/N_{\mathrm{FFT}}}.
$$

Here:

- \(N_{\mathrm{FFT}}\) is the FFT size;
- \(X[k]\) is the frequency-domain symbol on subcarrier \(k\);
- \(x[n]\) is the time-domain sample.

A cyclic prefix will be inserted before transmission and removed before receiver FFT processing.

## Channel Model

The first implementation is expected to include:

- AWGN;
- configurable tapped-delay-line channels;
- independent MIMO fading coefficients;
- configurable delay and power profiles.

A frequency-selective channel can be represented as

$$
\mathbf{H}[k]=
\sum_{p=0}^{P-1}
\mathbf{H}_{p}
e^{-j2\pi k\tau_p/N_{\mathrm{FFT}}}.
$$

Here:

- \(P\) is the number of channel taps;
- \(\mathbf{H}_{p}\) is the MIMO matrix of tap \(p\);
- \(\tau_p\) is the tap delay in samples.

Later versions may include:

- 3GPP TDL and CDL profiles;
- Doppler and mobility;
- spatial correlation;
- line-of-sight and non-line-of-sight conditions;
- blockage;
- timing offset;
- carrier-frequency offset;
- phase noise;
- IQ imbalance;
- PA nonlinearity.

## Reference Signals and Channel Estimation

The simulator will include a configurable pilot or DM-RS-like reference pattern.

For a scalar pilot observation, the LS estimate is

$$
\widehat{H}_{\mathrm{LS}}[k_p]=
\frac{Y[k_p]}{X[k_p]}.
$$

Here:

- \(k_p\) is a pilot-subcarrier index;
- \(X[k_p]\) is the known transmitted pilot;
- \(Y[k_p]\) is the received pilot.

For precoded MIMO transmission, the receiver may estimate the effective channel \(\mathbf{G}[k]\) directly.

Planned estimation methods include:

- least squares;
- frequency interpolation;
- time interpolation;
- two-dimensional interpolation;
- optional LMMSE estimation;
- ideal-CSI reference mode.

## Equalization

### Zero-Forcing Equalizer

The ZF equalizer is

$$
\mathbf{F}_{\mathrm{ZF}}[k]=
\widehat{\mathbf{G}}^{\dagger}[k].
$$

Here, \((\cdot)^{\dagger}\) denotes the Moore–Penrose pseudoinverse.

### MMSE Equalizer

The MMSE equalizer is

$$
\mathbf{F}_{\mathrm{MMSE}}[k]=
\left(
\widehat{\mathbf{G}}^{H}[k]
\widehat{\mathbf{G}}[k]
+
\sigma_n^2\mathbf{I}_{L}
\right)^{-1}
\widehat{\mathbf{G}}^{H}[k].
$$

The implementation will use a stable linear-system solver instead of explicitly calculating the inverse.

## Planned Precoding and Beamforming Methods

The framework may include:

- identity precoding;
- fixed beamforming;
- codebook-based precoding;
- wideband SVD precoding;
- subband SVD precoding;
- per-subcarrier SVD precoding;
- phase-aligned SVD precoding;
- optional hybrid analog–digital beamforming;
- optional channel-estimation-aware precoding.

## Planned Simulation Profiles

### Profile 1: SISO-OFDM Baseline

- one transmit antenna;
- one receive antenna;
- one data stream;
- AWGN and frequency-selective fading;
- ideal and estimated CSI.

### Profile 2: Small MIMO-OFDM

- \(2\times2\) or \(4\times4\) MIMO;
- one to four spatial layers;
- identity or SVD precoding;
- ZF and MMSE equalization.

### Profile 3: Massive MIMO-OFDM

- configurable transmit array such as 32 or 64 antennas;
- configurable receive array;
- multiple spatial layers;
- wideband, subband, or frequency-selective precoding.

### Profile 4: Future 6G-Oriented Extension

Possible studies include:

- extremely large antenna arrays;
- near-field MIMO;
- sub-THz-oriented channels;
- hybrid beamforming;
- beam squint;
- cell-free or distributed MIMO;
- reconfigurable intelligent surfaces;
- integrated sensing and communication;
- AI-assisted channel estimation and link adaptation.

These are planned research directions and are not yet implemented.

## Planned Performance Metrics

| Metric | Purpose |
|---|---|
| BER | Measures erroneous payload bits |
| BLER | Measures failed blocks |
| RMS EVM | Measures equalized-symbol distortion |
| Channel-estimation NMSE | Measures channel-estimation accuracy |
| Post-equalization SINR | Measures effective layer quality |
| Throughput | Measures successfully delivered payload rate |
| Spectral efficiency | Measures rate per unit bandwidth |
| Effective-channel capacity | Provides an information-theoretic reference |
| CRC pass rate | Measures block-level integrity |
| Runtime | Measures computational complexity |
| MATLAB–Python deviation | Measures cross-implementation consistency |

### BER

$$
\mathrm{BER}=
\frac{N_{\mathrm{bit,error}}}
{N_{\mathrm{bit,total}}}.
$$

### BLER

$$
\mathrm{BLER}=
\frac{N_{\mathrm{block,error}}}
{N_{\mathrm{block,total}}}.
$$

### Channel-Estimation NMSE

$$
\mathrm{NMSE}=
\frac{
\sum_k
\|\widehat{\mathbf{G}}[k]-\mathbf{G}[k]\|_F^2
}{
\sum_k
\|\mathbf{G}[k]\|_F^2
}.
$$

### RMS EVM

$$
\mathrm{EVM}_{\mathrm{RMS}}=
\sqrt{
\frac{
\sum_q|\widehat{s}_q-s_q|^2
}{
\sum_q|s_q|^2
}
}.
$$

## Planned Simulation Workflow

1. Load the selected configuration.
2. Initialize deterministic random seeds.
3. Generate payload bits.
4. Attach CRC.
5. Apply optional channel coding and rate matching.
6. Map bits to QAM symbols.
7. Map symbols to spatial layers.
8. Generate the pilot or DM-RS-like pattern.
9. Construct the frequency-time resource grid.
10. Calculate the selected precoder.
11. Apply precoding to data and reference symbols.
12. Perform OFDM modulation.
13. Propagate the waveform through the selected channel.
14. Add noise and optional RF impairments.
15. Perform OFDM demodulation.
16. Extract received pilot symbols.
17. Estimate the physical or effective channel.
18. Equalize the data symbols.
19. Demap the recovered QAM symbols.
20. Apply optional decoding and CRC checking.
21. Accumulate BER, BLER, EVM, NMSE, throughput, and runtime.
22. Repeat over frames and SNR points.
23. Export results, figures, configuration, and logs.
24. Compare MATLAB and Python outputs.

## Planned Verification Strategy

| Verification test | Technical purpose | Status |
|---|---|---|
| Constellation power | Verify normalized modulation power | Planned |
| QAM round trip | Verify mapper and demapper ordering | Planned |
| OFDM round trip | Verify IFFT, cyclic prefix, FFT, and indexing | Planned |
| CRC valid-block test | Verify acceptance of a correct block | Planned |
| CRC corrupted-block test | Verify rejection of a modified block | Planned |
| Precoder semi-unitarity | Verify equal-power normalization | Planned |
| Channel dimension test | Verify antenna and layer dimensions | Planned |
| Noiseless SISO recovery | Verify the basic transmitter–receiver chain | Planned |
| Noiseless MIMO recovery | Verify layer mapping and equalization | Planned |
| Flat-channel LS recovery | Verify pilot division | Planned |
| MMSE regularization test | Verify stable low-SNR equalization | Planned |
| Ideal-versus-estimated CSI | Verify expected scenario ordering | Planned |
| SNR monotonicity check | Verify logical performance improvement | Planned |
| MATLAB–Python deterministic test | Verify common numerical conventions | Planned |
| Reproducibility test | Verify fixed-input repeatability | Planned |

## MATLAB and Python Cross-Verification

Both implementations will use the same:

- configuration values;
- matrix dimensions;
- array-ordering convention;
- modulation normalization;
- layer ordering;
- resource-grid mapping;
- pilot locations;
- deterministic channel coefficients;
- noise variance;
- precoding rule;
- interpolation convention;
- equalizer definition;
- metric definitions.

Because MATLAB and Python may use different random-number generators, cross-verification will use deterministic shared inputs and stage-by-stage tolerance checks.

## Planned Result Figures

The completed project may include:

- BER versus SNR;
- BLER versus SNR;
- RMS EVM versus SNR;
- channel-estimation NMSE versus SNR;
- throughput versus SNR;
- spectral efficiency versus SNR;
- capacity versus SNR;
- ideal-CSI versus estimated-CSI performance;
- ZF versus MMSE performance;
- modulation-order comparison;
- layer-count comparison;
- antenna-configuration comparison;
- channel-profile comparison;
- precoding-method comparison;
- received constellation diagrams;
- channel-frequency-response plots;
- resource-grid visualization;
- MATLAB and Python comparison plots.

No result images are included while the project is under development.

## Current Development Status

- [x] Project topic defined
- [x] Initial repository created
- [x] High-level system scope identified
- [x] Initial mathematical model defined
- [ ] Detailed requirements finalized
- [ ] Common configuration module implemented
- [ ] QAM mapper and demapper implemented
- [ ] CRC module implemented
- [ ] Resource-grid builder implemented
- [ ] OFDM modulator and demodulator implemented
- [ ] SISO channel implemented
- [ ] MIMO channel implemented
- [ ] Precoding module implemented
- [ ] Reference-signal generator implemented
- [ ] Channel estimator implemented
- [ ] ZF equalizer implemented
- [ ] MMSE equalizer implemented
- [ ] Metric functions implemented
- [ ] Monte Carlo driver implemented
- [ ] MATLAB verification completed
- [ ] Python verification completed
- [ ] MATLAB–Python cross-verification completed
- [ ] Result figures generated
- [ ] Result tables generated
- [ ] Technical report completed

## Planned Repository Structure

```text
05-5G-NR-6G-Downlink-Physical-Layer-Link-Simulator/
├── README.md
├── matlab/
│   ├── main.m
│   ├── config.m
│   ├── transmitter/
│   ├── channel/
│   ├── receiver/
│   ├── metrics/
│   └── verification/
├── python/
│   ├── main.py
│   ├── config.py
│   ├── transmitter/
│   ├── channel/
│   ├── receiver/
│   ├── metrics/
│   └── verification/
├── simulink/
│   ├── model/
│   ├── builders/
│   └── testbench/
├── results/
│   ├── figures/
│   ├── tables/
│   └── csv/
├── verification/
│   ├── deterministic_vectors/
│   ├── matlab_python_comparison/
│   └── logs/
├── docs/
│   ├── architecture/
│   ├── equations/
│   ├── algorithms/
│   └── technical_report/
└── LICENSE
```

The structure is planned and may change during implementation. Once source files exist, their exact filenames should be preserved to avoid breaking imports, function calls, and testbench dependencies.

## Software Requirements

### MATLAB

The MATLAB implementation is expected to use:

- MATLAB;
- optional Communications Toolbox;
- optional 5G Toolbox;
- optional Phased Array System Toolbox;
- optional Simulink.

### Python

The Python implementation is expected to use:

- Python 3;
- NumPy;
- SciPy;
- Matplotlib;
- pandas;
- optional pytest;
- optional h5py or SciPy I/O for deterministic shared vectors.

## How to Run

The project is not yet ready for complete execution.

Planned Python command:

```bash
python python/main.py
```

Planned MATLAB command:

```matlab
run("matlab/main.m")
```

The final commands will be updated to match the actual implementation.

## Expected Engineering Outcomes

The completed simulator is expected to demonstrate:

- end-to-end downlink physical-layer processing;
- the relationship between resource-grid design and receiver operation;
- the effect of modulation order on BER and EVM;
- the effect of channel-estimation quality on equalization;
- the difference between ZF and MMSE detection;
- the gain of MIMO and Massive MIMO precoding;
- the difference between ideal and practical CSI;
- the effect of antenna count and spatial-layer count;
- the effect of frequency-selective fading;
- the relationship between BER, BLER, throughput, and spectral efficiency;
- reproducible MATLAB and Python implementations;
- verification-driven simulation development.

No final numerical conclusion is claimed while the project remains under development.

## Initial Model Limitations

The first version may include:

- single-user downlink;
- ideal synchronization;
- simplified DM-RS-like mapping;
- no scheduler;
- no HARQ;
- no complete MAC-layer interaction;
- no complete RF front-end;
- simplified channel coding;
- perfect or simplified transmitter CSI;
- no feedback delay;
- no reciprocity error;
- no hardware-in-the-loop validation;
- no over-the-air validation;
- no claim of complete 3GPP conformance.

## Future 6G Extensions

Possible future extensions include:

- extremely large MIMO arrays;
- near-field MIMO;
- sub-THz channels;
- beam squint;
- hybrid beamforming;
- cell-free Massive MIMO;
- distributed MIMO;
- reconfigurable intelligent surfaces;
- integrated sensing and communication;
- non-terrestrial networks;
- semantic communications;
- AI-assisted channel estimation;
- AI-assisted beam selection;
- AI-assisted link adaptation;
- learned receivers;
- digital-twin-assisted simulation;
- real-time testbed integration.

## Skills Demonstrated

After completion, this project is expected to demonstrate experience in:

- 5G NR physical-layer modeling;
- future 6G research concepts;
- OFDM;
- MIMO and Massive MIMO;
- beamforming and precoding;
- reference-signal processing;
- channel estimation;
- ZF and MMSE equalization;
- digital modulation;
- link-level simulation;
- Monte Carlo analysis;
- MATLAB;
- Python;
- Simulink;
- numerical verification;
- cross-platform validation;
- RF/PHY performance analysis;
- technical documentation.

## Contribution

This is currently an individual engineering simulation project under active development.

Technical comments and improvement suggestions may be considered after the first executable version is released.

## License

A license will be selected before the source code is publicly released.

Until a `LICENSE` file is added, the project should be treated as **all rights reserved**.

## Author

**Md Moklesur Rahman**  
Wireless/RF/PHY System Engineer  
Finland

## Notice

This README describes the planned scope of an ongoing project.

Features marked as planned have not yet been presented as completed, validated, or standards compliant. The README will be updated as implementation, verification, result generation, and technical documentation progress.
