# Beamforming and Direction-of-Arrival Estimation

## Project Information

**Project type:** Antenna-array signal-processing simulation and verification  
**Technical area:** Beamforming, Direction-of-Arrival estimation, array processing, MIMO, 5G NR, and future 6G radio systems  
**Planned implementation:** MATLAB and Python, with optional Simulink integration  
**Project status:** Ongoing project — currently under development  
**Author:** Md Moklesur Rahman  
**Location:** Finland

## Project Status

This project is currently under active development.

The objective is to build a configurable simulation framework for studying antenna-array beamforming and Direction-of-Arrival (DoA) estimation. The planned framework will model signal generation, array reception, spatial covariance estimation, beamformer design, angular-spectrum calculation, source-angle estimation, numerical verification, and performance evaluation.

The source code, configuration files, verification tests, result tables, figures, and technical documentation will be added progressively. No completed implementation or final numerical-performance claim is made at the current stage.

## Overview

Beamforming and DoA estimation are closely related antenna-array signal-processing functions.

Beamforming applies complex weights across antenna elements to strengthen signals from selected directions and suppress signals, noise, or interference from other directions. DoA estimation uses the phase and amplitude differences measured across an antenna array to estimate the directions from which signals arrive.

These methods are important in:

- 5G NR and future 6G base stations;
- Massive MIMO systems;
- beam management;
- interference suppression;
- radar and sensing;
- satellite communications;
- localization;
- channel sounding;
- spectrum monitoring;
- integrated sensing and communication.

The planned project will compare conventional and adaptive beamforming with classical high-resolution DoA estimation methods under controlled channel, noise, interference, array-size, and source-separation conditions.

## Main Objectives

The main objectives are to:

1. model uniform linear and optional uniform planar antenna arrays;
2. generate mathematically consistent array steering vectors;
3. simulate one or more narrowband signals arriving from configurable directions;
4. estimate the received-signal covariance matrix;
5. implement conventional delay-and-sum beamforming;
6. implement adaptive MVDR beamforming;
7. implement Bartlett, Capon, MUSIC, and optional ESPRIT DoA estimators;
8. evaluate angular resolution and estimation accuracy;
9. study the effects of SNR, snapshots, source correlation, and antenna count;
10. evaluate interference suppression and output SINR;
11. verify the implementation using deterministic test cases;
12. cross-check selected scenarios in MATLAB and Python;
13. provide a foundation for future 5G NR and 6G array-processing studies.

## Planned Technical Scope

The planned framework is expected to include:

- uniform linear array modeling;
- optional uniform planar array modeling;
- configurable antenna-element count;
- configurable inter-element spacing;
- configurable carrier frequency and wavelength;
- configurable source directions;
- one or multiple narrowband sources;
- uncorrelated and correlated sources;
- complex additive white Gaussian noise;
- optional directional interference;
- sample covariance estimation;
- conventional beamforming;
- MVDR beamforming;
- Bartlett DoA estimation;
- Capon spatial-spectrum estimation;
- MUSIC DoA estimation;
- optional ESPRIT estimation;
- Monte Carlo evaluation;
- MATLAB–Python deterministic cross-verification.

## Planned End-to-End Architecture

```text
Source Configuration
    ↓
Signal Generation
    ↓
Source Direction and Array-Manifold Generation
    ↓
Antenna-Array Reception
    ↓
Noise and Interference Addition
    ↓
Snapshot Collection
    ↓
Sample Covariance Estimation
    ↓
Beamforming and/or DoA Estimation
    ↓
Angular Spectrum / Estimated Directions
    ↓
Beamformer Output
    ↓
Performance Metrics
    ↓
Verification and MATLAB–Python Comparison
```

## Antenna-Array Model

### Uniform Linear Array

The initial implementation will use a uniform linear array (ULA).

For a ULA with \(M\) antenna elements and inter-element spacing \(d\), the normalized steering vector is

$$
\mathbf{a}(\theta)=
\frac{1}{\sqrt{M}}
\begin{bmatrix}
1 &
e^{-j2\pi(d/\lambda)\sin(\theta)} &
\cdots &
e^{-j2\pi(M-1)(d/\lambda)\sin(\theta)}
\end{bmatrix}^{T}.
$$

Here:

- \(M\) is the number of antenna elements;
- \(d\) is the spacing between adjacent elements;
- \(\lambda\) is the carrier wavelength;
- \(\theta\) is the signal arrival angle measured from array broadside;
- \(j=\sqrt{-1}\) is the imaginary unit;
- \((\cdot)^T\) denotes transpose.

The normalization gives

$$
\|\mathbf{a}(\theta)\|_2^2=1.
$$

The selected phase-sign and angle conventions must remain identical in MATLAB and Python.

### Inter-Element Spacing

The initial array will normally use

$$
d=\frac{\lambda}{2}.
$$

Half-wavelength spacing is commonly used because it provides broad angular coverage while avoiding grating lobes over the visible region for the standard ULA model.

Other spacings may be studied to demonstrate:

- grating-lobe formation;
- angular ambiguity;
- resolution changes;
- aperture changes.

## Received Array-Signal Model

For \(D\) narrowband sources and \(M\) receive antennas, the received snapshot is

$$
\mathbf{x}[n]=
\mathbf{A}\mathbf{s}[n]
+
\mathbf{v}[n].
$$

Here:

- \(\mathbf{x}[n]\) is the \(M\times1\) received array vector;
- \(\mathbf{s}[n]\) is the \(D\times1\) source-signal vector;
- \(\mathbf{v}[n]\) is the \(M\times1\) noise vector;
- \(\mathbf{A}\) is the \(M\times D\) array-manifold matrix.

The array-manifold matrix is

$$
\mathbf{A}=
\begin{bmatrix}
\mathbf{a}(\theta_1) &
\mathbf{a}(\theta_2) &
\cdots &
\mathbf{a}(\theta_D)
\end{bmatrix}.
$$

Here, \(\theta_1,\theta_2,\ldots,\theta_D\) are the true source directions.

## Spatial Covariance Matrix

The theoretical covariance matrix is

$$
\mathbf{R}_{xx}=
\mathrm{E}
\left[
\mathbf{x}[n]\mathbf{x}^{H}[n]
\right].
$$

With \(N_s\) received snapshots, the sample covariance estimate is

$$
\widehat{\mathbf{R}}_{xx}=
\frac{1}{N_s}
\sum_{n=1}^{N_s}
\mathbf{x}[n]\mathbf{x}^{H}[n].
$$

Here:

- \(N_s\) is the number of snapshots;
- \((\cdot)^H\) denotes Hermitian transpose;
- \(\mathrm{E}[\cdot]\) denotes statistical expectation.

The quality of the sample covariance estimate strongly affects adaptive beamforming and subspace-based DoA estimation.

## Conventional Beamforming

### Delay-and-Sum Beamformer

For a desired steering direction \(\theta_0\), the conventional beamformer weights are

$$
\mathbf{w}_{\mathrm{CBF}}=
\mathbf{a}(\theta_0).
$$

The beamformer output is

$$
y[n]=
\mathbf{w}^{H}\mathbf{x}[n].
$$

The normalized angular response is

$$
B_{\mathrm{CBF}}(\theta)=
\left|
\mathbf{w}_{\mathrm{CBF}}^{H}
\mathbf{a}(\theta)
\right|^2.
$$

The conventional beamformer is simple and robust, but its angular resolution is limited by the physical aperture and number of antenna elements.

## MVDR Beamforming

The Minimum Variance Distortionless Response (MVDR) beamformer minimizes output power while maintaining unit response in the desired direction.

The planned MVDR weights are

$$
\mathbf{w}_{\mathrm{MVDR}}=
\frac{
\widehat{\mathbf{R}}_{xx}^{-1}
\mathbf{a}(\theta_0)
}{
\mathbf{a}^{H}(\theta_0)
\widehat{\mathbf{R}}_{xx}^{-1}
\mathbf{a}(\theta_0)
}.
$$

The distortionless constraint is

$$
\mathbf{w}_{\mathrm{MVDR}}^{H}
\mathbf{a}(\theta_0)=1.
$$

The implementation will use a numerically stable linear-system solver rather than explicitly forming the matrix inverse.

Diagonal loading may be added:

$$
\widehat{\mathbf{R}}_{\mathrm{DL}}=
\widehat{\mathbf{R}}_{xx}
+
\delta\mathbf{I}_{M}.
$$

Here, \(\delta\) is the diagonal-loading factor.

## Direction-of-Arrival Estimation

### Bartlett Spatial Spectrum

The Bartlett spectrum is

$$
P_{\mathrm{Bartlett}}(\theta)=
\mathbf{a}^{H}(\theta)
\widehat{\mathbf{R}}_{xx}
\mathbf{a}(\theta).
$$

The estimated directions correspond to the dominant peaks of the spatial spectrum.

### Capon Spatial Spectrum

The Capon or MVDR spatial spectrum is

$$
P_{\mathrm{Capon}}(\theta)=
\frac{1}{
\mathbf{a}^{H}(\theta)
\widehat{\mathbf{R}}_{xx}^{-1}
\mathbf{a}(\theta)
}.
$$

Capon processing normally provides narrower peaks than Bartlett processing, but it is more sensitive to covariance-estimation quality and numerical conditioning.

### MUSIC DoA Estimation

The sample covariance matrix is decomposed as

$$
\widehat{\mathbf{R}}_{xx}=
\mathbf{E}_{s}
\boldsymbol{\Lambda}_{s}
\mathbf{E}_{s}^{H}
+
\mathbf{E}_{n}
\boldsymbol{\Lambda}_{n}
\mathbf{E}_{n}^{H}.
$$

Here:

- \(\mathbf{E}_{s}\) contains signal-subspace eigenvectors;
- \(\mathbf{E}_{n}\) contains noise-subspace eigenvectors;
- \(\boldsymbol{\Lambda}_{s}\) and \(\boldsymbol{\Lambda}_{n}\) contain the corresponding eigenvalues.

The MUSIC pseudospectrum is

$$
P_{\mathrm{MUSIC}}(\theta)=
\frac{1}{
\mathbf{a}^{H}(\theta)
\mathbf{E}_{n}
\mathbf{E}_{n}^{H}
\mathbf{a}(\theta)
}.
$$

The estimated DoAs are obtained from the strongest \(D\) peaks.

MUSIC requires:

- the number of sources to be known or estimated;
- a valid array model;
- enough snapshots;
- a sufficiently accurate covariance matrix;
- source conditions that preserve useful signal-subspace rank.

### Optional ESPRIT Extension

An optional future extension may implement ESPRIT using the shift-invariance property of two overlapping array subarrays.

ESPRIT can estimate directions without scanning an angular grid, but it requires a suitable array structure and careful eigenvalue interpretation.

## Planned Beamforming Methods

The project may compare:

- conventional delay-and-sum beamforming;
- maximum-ratio receive combining;
- MVDR beamforming;
- diagonally loaded MVDR;
- linearly constrained minimum variance beamforming;
- null-steering beamforming;
- optional generalized sidelobe canceller.

## Planned DoA Estimation Methods

The project may compare:

- Bartlett beam scanning;
- Capon or MVDR spectrum;
- MUSIC;
- root-MUSIC;
- ESPRIT;
- optional sparse-reconstruction methods;
- optional learning-based DoA estimation.

The exact implemented method set will be updated as the project develops.

## Planned Simulation Scenarios

### Scenario 1: Single Source

- one source;
- known arrival angle;
- AWGN;
- configurable SNR;
- ULA reception.

This scenario will verify steering-vector generation, beam scanning, and basic DoA estimation.

### Scenario 2: Two Well-Separated Sources

- two sources with sufficient angular separation;
- equal or unequal powers;
- multiple SNR values;
- Bartlett, Capon, and MUSIC comparison.

### Scenario 3: Closely Spaced Sources

- two sources separated by a small angular difference;
- comparison of classical beamwidth and high-resolution DoA estimation;
- evaluation of resolution probability.

### Scenario 4: Directional Interference

- one desired source;
- one or more interferers;
- conventional versus MVDR beamforming;
- output SINR and interference-suppression comparison.

### Scenario 5: Limited Snapshots

- fixed source directions;
- varying snapshot count;
- covariance-estimation sensitivity;
- DoA RMSE versus snapshots.

### Scenario 6: Correlated Sources

- partially or strongly correlated sources;
- MUSIC rank-degradation study;
- optional spatial smoothing.

### Scenario 7: Array-Model Mismatch

- steering-angle error;
- element-position error;
- gain and phase mismatch;
- mutual-coupling approximation;
- beamforming and DoA degradation.

### Scenario 8: Grating Lobes

- inter-element spacing greater than half a wavelength;
- ambiguous array response;
- incorrect or multiple spectrum peaks.

## Planned Performance Metrics

| Metric | Purpose |
|---|---|
| DoA estimation error | Measures estimated-angle accuracy |
| DoA RMSE | Measures average angular estimation error |
| Bias | Measures systematic angular error |
| Resolution probability | Measures whether close sources are separately detected |
| Detection probability | Measures successful source detection |
| False-peak rate | Measures incorrect angular peaks |
| Beamforming gain | Measures directional signal enhancement |
| Output SINR | Measures post-beamforming signal quality |
| Interference suppression | Measures interferer attenuation |
| Main-lobe width | Measures angular selectivity |
| Peak sidelobe level | Measures sidelobe behavior |
| Null depth | Measures suppression at interferer directions |
| Runtime | Measures computational complexity |
| MATLAB–Python deviation | Measures implementation consistency |

### DoA Error

For true angle \(\theta_d\) and estimated angle \(\widehat{\theta}_d\),

$$
e_d=
\widehat{\theta}_d-\theta_d.
$$

### DoA RMSE

For \(N_{\mathrm{MC}}\) Monte Carlo trials,

$$
\mathrm{RMSE}_{\theta}=
\sqrt{
\frac{1}{N_{\mathrm{MC}}}
\sum_{r=1}^{N_{\mathrm{MC}}}
\left(
\widehat{\theta}^{(r)}-\theta
\right)^2
}.
$$

### Output SINR

A planned output-SINR metric is

$$
\mathrm{SINR}_{\mathrm{out}}=
\frac{
\sigma_s^2
\left|
\mathbf{w}^{H}\mathbf{a}(\theta_s)
\right|^2
}{
\mathbf{w}^{H}
\mathbf{R}_{i+n}
\mathbf{w}
}.
$$

Here:

- \(\sigma_s^2\) is desired-signal power;
- \(\theta_s\) is desired-signal direction;
- \(\mathbf{R}_{i+n}\) is the interference-plus-noise covariance matrix.

## Planned Simulation Workflow

1. Load the selected configuration.
2. Define carrier frequency and wavelength.
3. Configure the antenna array.
4. Define source and interference directions.
5. Generate the steering vectors.
6. Generate source signals.
7. Form the array-manifold matrix.
8. Generate received array snapshots.
9. Add noise and interference.
10. Estimate the sample covariance matrix.
11. Calculate conventional beamformer weights.
12. Calculate MVDR beamformer weights.
13. Scan the angular search grid.
14. Calculate Bartlett, Capon, and MUSIC spectra.
15. Detect spatial-spectrum peaks.
16. Estimate source directions.
17. Calculate beamformer outputs.
18. Calculate DoA and beamforming metrics.
19. Repeat over SNR, snapshots, source separation, and Monte Carlo trials.
20. Export result tables, figures, configuration, and logs.
21. Compare MATLAB and Python outputs.
22. Apply verification checks before accepting results.

## Planned Verification Strategy

| Verification test | Technical purpose | Status |
|---|---|---|
| Steering-vector norm | Verify unit-norm array response | Planned |
| Broadside steering test | Verify the phase convention at zero degrees | Planned |
| Element-phase progression | Verify adjacent-element phase difference | Planned |
| Array-manifold dimensions | Verify \(M\times D\) matrix size | Planned |
| Covariance Hermitian test | Verify covariance conjugate symmetry | Planned |
| Covariance positive-semidefinite test | Verify valid covariance structure | Planned |
| Single-source noiseless DoA test | Verify exact or grid-limited angle recovery | Planned |
| Beamformer distortionless constraint | Verify unit response in the desired direction | Planned |
| Interference-null test | Verify suppression at a known interferer angle | Planned |
| MUSIC subspace dimension | Verify signal/noise eigenvector partition | Planned |
| Peak-count test | Verify the expected number of detected sources | Planned |
| SNR sensitivity test | Verify logical accuracy improvement with SNR | Planned |
| Snapshot sensitivity test | Verify logical improvement with more snapshots | Planned |
| MATLAB–Python deterministic comparison | Verify common numerical conventions | Planned |
| Reproducibility test | Verify fixed-input repeatability | Planned |

## MATLAB and Python Cross-Verification

The MATLAB and Python implementations will use the same:

- carrier frequency;
- wavelength;
- antenna count;
- element spacing;
- angle convention;
- steering-vector phase convention;
- angular scan grid;
- source directions;
- source powers;
- deterministic snapshot matrices;
- noise variance;
- covariance normalization;
- eigenvalue ordering;
- source-count assumption;
- peak-selection rule;
- metric definitions.

Because MATLAB and Python may use different random-number generators and eigenvector phase conventions, deterministic cross-verification will compare physically meaningful quantities rather than raw eigenvector phases alone.

The verification process will include:

1. comparison of steering vectors;
2. comparison of array-manifold matrices;
3. comparison of covariance matrices;
4. comparison of eigenvalues;
5. comparison of projection matrices;
6. comparison of spatial spectra;
7. comparison of detected peak locations;
8. comparison of beamformer constraints;
9. comparison of final DoA and SINR metrics.

## Planned Result Figures

The completed project may include:

- ULA element geometry;
- steering-vector phase progression;
- conventional beam pattern;
- MVDR beam pattern;
- null-steering response;
- Bartlett spatial spectrum;
- Capon spatial spectrum;
- MUSIC pseudospectrum;
- true and estimated source directions;
- DoA RMSE versus SNR;
- DoA RMSE versus snapshot count;
- resolution probability versus source separation;
- output SINR versus input SNR;
- interference suppression versus interferer angle;
- beamwidth versus antenna count;
- grating-lobe behavior versus element spacing;
- MATLAB and Python comparison plots.

No result images are included while the project is under development.

## Current Development Status

- [x] Project topic defined
- [x] Initial repository created
- [x] High-level scope identified
- [x] Initial signal model defined
- [ ] Detailed requirements finalized
- [ ] Common configuration module implemented
- [ ] ULA steering-vector function implemented
- [ ] UPA steering-vector function implemented
- [ ] Source-signal generator implemented
- [ ] Array-reception model implemented
- [ ] Covariance estimator implemented
- [ ] Conventional beamformer implemented
- [ ] MVDR beamformer implemented
- [ ] Bartlett estimator implemented
- [ ] Capon estimator implemented
- [ ] MUSIC estimator implemented
- [ ] ESPRIT estimator implemented
- [ ] Peak-detection logic implemented
- [ ] Monte Carlo driver implemented
- [ ] MATLAB verification completed
- [ ] Python verification completed
- [ ] MATLAB–Python cross-verification completed
- [ ] Result figures generated
- [ ] Result tables generated
- [ ] Technical report completed

This checklist will be updated as development progresses.

## Planned Repository Structure

```text
06-Beamforming-and-DoA-Estimation/
├── README.md
├── matlab/
│   ├── main.m
│   ├── config.m
│   ├── array/
│   ├── signals/
│   ├── beamforming/
│   ├── doa/
│   ├── metrics/
│   └── verification/
├── python/
│   ├── main.py
│   ├── config.py
│   ├── array/
│   ├── signals/
│   ├── beamforming/
│   ├── doa/
│   ├── metrics/
│   └── verification/
├── simulink/
│   ├── model/
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

The structure is planned and may change during implementation. Once source files are created, their exact filenames should be preserved to avoid breaking imports, function calls, and testbench dependencies.

## Software Requirements

### MATLAB

The MATLAB implementation is expected to use:

- MATLAB;
- optional Phased Array System Toolbox;
- optional Signal Processing Toolbox;
- optional Communications Toolbox;
- optional Simulink.

### Python

The Python implementation is expected to use:

- Python 3;
- NumPy;
- SciPy;
- Matplotlib;
- pandas;
- optional pytest;
- optional h5py or SciPy I/O for shared deterministic vectors.

A final dependency file will be added after the implementation becomes stable.

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

The completed project is expected to demonstrate:

- antenna-array steering-vector construction;
- conventional and adaptive beamforming;
- spatial covariance estimation;
- angular-spectrum generation;
- single-source and multi-source DoA estimation;
- differences between Bartlett, Capon, and MUSIC;
- effects of SNR and snapshot count;
- effects of source separation and correlation;
- interference suppression using MVDR;
- grating-lobe behavior;
- array-model mismatch sensitivity;
- reproducible MATLAB and Python implementations;
- verification-driven array-processing development.

No final numerical conclusion is claimed while the project remains under development.

## Initial Model Limitations

The first version may include:

- narrowband far-field sources;
- ideal antenna elements;
- a uniform linear array;
- ideal time and frequency synchronization;
- no mutual coupling;
- no polarization mismatch;
- no wideband beam squint;
- known or manually configured source count;
- simplified source and noise models;
- no hardware calibration errors;
- no hardware-in-the-loop validation;
- no over-the-air validation.

These limitations will be stated clearly and extended gradually where useful.

## Future Extensions

Possible future extensions include:

- uniform planar arrays;
- azimuth and elevation estimation;
- wideband DoA estimation;
- frequency-invariant beamforming;
- hybrid analog–digital beamforming;
- near-field source localization;
- source-number estimation;
- spatial smoothing for coherent sources;
- root-MUSIC;
- ESPRIT;
- sparse Bayesian DoA estimation;
- compressive-sensing-based DoA estimation;
- machine-learning-assisted DoA estimation;
- array calibration-error compensation;
- mutual-coupling compensation;
- 5G NR beam-management integration;
- Massive MIMO uplink and downlink studies;
- integrated sensing and communication;
- real measurement-data processing;
- hardware-in-the-loop validation;
- over-the-air validation.

## Skills Demonstrated

After completion, this project is expected to demonstrate experience in:

- antenna-array signal processing;
- steering-vector design;
- spatial covariance estimation;
- conventional beamforming;
- adaptive beamforming;
- interference suppression;
- Direction-of-Arrival estimation;
- MUSIC and Capon processing;
- eigenvalue decomposition;
- subspace methods;
- Monte Carlo analysis;
- MATLAB development;
- Python development;
- Simulink integration;
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
