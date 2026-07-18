
# 5G NR/6G Wireless Engineering Portfolio

## Portfolio Overview

This repository contains a growing collection of wireless-engineering simulation and verification projects focused on **5G NR, future 6G systems, radio-channel analysis, physical-layer link simulation, beamforming, and Direction-of-Arrival estimation**.

The portfolio is designed to demonstrate an engineering workflow that connects:

- mathematical modeling;
- physical-layer algorithm design;
- MATLAB and Python implementation;
- deterministic verification;
- Monte Carlo simulation;
- performance analysis;
- reproducible technical documentation.

All projects in this repository are currently under development. Source code, result figures, verification evidence, and technical reports will be added progressively as each project reaches a stable and validated stage.

## Current Projects

| No. | Project | Main Technical Focus | Status |
|---:|---|---|---|
| 01 | [Radio Channel Sounding and Multipath Analysis](./01_radio_channel_sounding_multipath_analysis/) | Channel impulse response, power-delay profile, path loss, delay spread, coherence bandwidth, and multipath characterization | Ongoing |
| 02 | [5G NR/6G Downlink Physical-Layer Link Simulator](./02_5GNR_6G_Downlink_Physical-Layer_Link_Simulator/) | OFDM, MIMO, Massive MIMO, resource-grid processing, channel estimation, equalization, and link-level metrics | Ongoing |
| 03 | [Beamforming and Direction-of-Arrival Estimation](./03_beamforming_and_doa_estimation/) | Antenna arrays, spatial covariance, conventional and adaptive beamforming, MUSIC, Capon, and DoA estimation | Ongoing |

## Project 01: Radio Channel Sounding and Multipath Analysis

The first project focuses on the analysis of radio-propagation channels using channel-sounding measurements or simulated channel data.

Planned technical areas include:

- channel impulse response;
- power-delay profile;
- path-loss analysis;
- excess delay;
- mean delay;
- RMS delay spread;
- coherence bandwidth;
- multipath-component detection;
- line-of-sight and non-line-of-sight comparison;
- temporal and spatial channel variation;
- measurement-data cleaning;
- MATLAB and Python result comparison.

The project is intended to connect raw channel observations with physically meaningful radio-channel parameters.

### Expected Outputs

- processed channel-sounding datasets;
- channel impulse-response plots;
- power-delay-profile plots;
- delay-spread statistics;
- path-loss fitting;
- multipath-component tables;
- MATLAB and Python comparison records;
- a technical analysis report.

## Project 02: 5G NR/6G Downlink Physical-Layer Link Simulator

The second project focuses on a modular downlink link-level simulator for 5G NR-oriented and future 6G-oriented physical-layer studies.

Planned technical areas include:

- payload-bit generation;
- CRC processing;
- QAM modulation and demodulation;
- layer mapping;
- resource-grid construction;
- pilot or DM-RS-like reference signals;
- OFDM modulation and demodulation;
- SISO, MIMO, and Massive MIMO configurations;
- digital precoding and beamforming;
- frequency-selective channels;
- LS and optional LMMSE channel estimation;
- ZF and MMSE equalization;
- BER, BLER, EVM, NMSE, throughput, spectral efficiency, and capacity;
- deterministic verification;
- MATLAB and Python cross-verification;
- optional Simulink integration.

The project is intended as a configurable engineering and research platform rather than a complete standards-conformance implementation during its initial development stage.

### Expected Outputs

- reusable transmitter, channel, and receiver modules;
- configurable simulation profiles;
- verification-gate results;
- BER and BLER curves;
- EVM and NMSE analysis;
- throughput and capacity analysis;
- MATLAB–Python comparison records;
- reproducibility files;
- a technical report.

## Project 03: Beamforming and Direction-of-Arrival Estimation

The third project focuses on antenna-array processing for directional reception, interference suppression, and source-angle estimation.

Planned technical areas include:

- uniform linear arrays;
- optional uniform planar arrays;
- steering-vector generation;
- array-manifold construction;
- spatial covariance estimation;
- conventional delay-and-sum beamforming;
- MVDR beamforming;
- null steering;
- Bartlett spatial-spectrum estimation;
- Capon spectrum estimation;
- MUSIC DoA estimation;
- optional ESPRIT estimation;
- angular-resolution analysis;
- source-separation studies;
- output-SINR analysis;
- interference suppression;
- MATLAB and Python cross-verification.

The project will evaluate how SNR, antenna count, source separation, snapshot count, source correlation, and model mismatch affect beamforming and DoA-estimation performance.

### Expected Outputs

- beam-pattern plots;
- Bartlett, Capon, and MUSIC spectra;
- estimated-versus-true angle comparisons;
- DoA RMSE results;
- angular-resolution analysis;
- output-SINR results;
- interference-null analysis;
- MATLAB–Python comparison records;
- a technical report.

## Common Engineering Methodology

Each project is planned to follow the same verification-driven workflow.

1. Define the engineering problem and system assumptions.
2. Derive the mathematical model.
3. Define dimensions, indexing, normalization, and units.
4. Implement modular MATLAB functions.
5. Implement a corresponding Python model where appropriate.
6. Create deterministic unit and integration tests.
7. Verify noiseless or analytically predictable cases.
8. Run controlled Monte Carlo simulations.
9. retain raw result counts and configuration records.
10. compare MATLAB and Python outputs.
11. generate figures and result tables.
12. document limitations and interpretation.
13. publish only validated results.

## Verification Principles

The projects will not rely only on visually reasonable plots. Planned verification includes:

- dimension and array-shape checks;
- normalization checks;
- deterministic test vectors;
- noiseless round-trip tests;
- covariance and matrix-property checks;
- expected performance-ordering checks;
- tolerance-based MATLAB–Python comparison;
- fixed-input reproducibility;
- configuration and result logging.

## Main Technical Areas

This portfolio covers the following engineering areas:

- 5G NR physical-layer modeling;
- future 6G research concepts;
- radio-channel sounding;
- multipath propagation;
- OFDM;
- MIMO and Massive MIMO;
- antenna arrays;
- beamforming;
- spatial signal processing;
- Direction-of-Arrival estimation;
- channel estimation;
- ZF and MMSE equalization;
- link-level performance analysis;
- Monte Carlo simulation;
- MATLAB;
- Python;
- optional Simulink integration;
- numerical verification;
- technical documentation.

## Repository Structure

```text
5G-NR-6G-Wireless-Engineering-Portfolio/
├── README.md
├── 01_radio_channel_sounding_multipath_analysis/
│   ├── README.md
│   ├── matlab/
│   ├── python/
│   ├── data/
│   ├── results/
│   ├── verification/
│   └── docs/
├── 02_5GNR_6G_Downlink_Physical-Layer_Link_Simulator/
│   ├── README.md
│   ├── matlab/
│   ├── python/
│   ├── simulink/
│   ├── results/
│   ├── verification/
│   └── docs/
└── 03_beamforming_and_doa_estimation/
    ├── README.md
    ├── matlab/
    ├── python/
    ├── simulink/
    ├── results/
    ├── verification/
    └── docs/
```

The internal folder structures are planned and may be extended as the projects develop. Once executable source files are added, their filenames should be preserved to avoid breaking imports, function calls, and testbench dependencies.

## Current Portfolio Status

- [x] Main portfolio repository created
- [x] Project 01 folder created
- [x] Project 02 folder created
- [x] Project 03 folder created
- [x] Initial project scopes defined
- [x] Initial project README files prepared
- [ ] Project 01 implementation completed
- [ ] Project 01 verification completed
- [ ] Project 02 implementation completed
- [ ] Project 02 verification completed
- [ ] Project 03 implementation completed
- [ ] Project 03 verification completed
- [ ] MATLAB–Python cross-verification completed for all applicable projects
- [ ] Final result figures added
- [ ] Final technical reports added
- [ ] Stable software releases created

This checklist will be updated as development progresses.

## Software Environment

The projects are expected to use combinations of:

### MATLAB

- MATLAB;
- optional Communications Toolbox;
- optional 5G Toolbox;
- optional Phased Array System Toolbox;
- optional Signal Processing Toolbox;
- optional Simulink.

### Python

- Python 3;
- NumPy;
- SciPy;
- Matplotlib;
- pandas;
- optional pytest;
- optional h5py or SciPy I/O.

Exact software dependencies will be documented separately inside each project folder after the implementation becomes stable.

## How to Use This Repository

Open the folder of the project you want to review and read its project-specific `README.md`.

Each project README will describe:

- project status;
- system scope;
- mathematical model;
- planned or completed algorithms;
- implementation structure;
- verification methodology;
- simulation scenarios;
- performance metrics;
- limitations;
- future extensions;
- execution instructions when available.

The projects are not yet presented as complete executable releases. Final run instructions will be added separately to each project after implementation and verification.

## Development and Release Policy

This repository distinguishes clearly between planned work and completed work.

A feature will be marked as completed only after:

- the implementation exists;
- the relevant verification tests pass;
- the configuration is documented;
- the reported results can be reproduced;
- known limitations are stated.

No final result should be interpreted as validated until the corresponding project README and verification records explicitly mark it as completed.

## Expected Portfolio Outcomes

After completion, this portfolio is expected to demonstrate:

- end-to-end wireless-system modeling;
- physical-layer algorithm development;
- radio-channel analysis;
- antenna-array signal processing;
- modular MATLAB and Python implementation;
- deterministic verification;
- Monte Carlo performance evaluation;
- cross-platform validation;
- reproducible engineering documentation;
- awareness of model limitations and standards boundaries.

## Future Portfolio Expansion

Possible future projects include:

- 5G NR beam-management simulation;
- synchronization and carrier-frequency-offset estimation;
- channel coding and link adaptation;
- hybrid beamforming;
- near-field Massive MIMO;
- reconfigurable intelligent surfaces;
- cell-free Massive MIMO;
- non-terrestrial networks;
- integrated sensing and communication;
- AI-assisted channel estimation;
- AI-assisted beam selection;
- semantic communications;
- RF-impairment modeling;
- hardware-in-the-loop validation;
- over-the-air measurement analysis.

## License

A repository-wide license will be selected before source code is released publicly as a stable package.

Until a `LICENSE` file is added, the repository should be treated as **all rights reserved**.

## Author

**Md Moklesur Rahman**  
Wireless/RF/PHY System Engineer  
Finland

## Notice

This repository contains completed and ongoing engineering projects.

Project descriptions, planned features, and proposed repository structures may change during implementation. Features marked as planned have not yet been presented as completed, validated, or standards compliant.
