# Char-retentive pyrolysis of wind turbine blades for floating solar evaporators

Simulation code for the manuscript:

> [Muhammad Irfan Habib]. *Char-Retentive Partial Pyrolysis of End-of-Life Wind Turbine Blades for Floating Solar Interfacial Evaporators: A Novel Model-Based Design Study*. Submitted to *Sustainable Materials and Technologies* (2026).

## What the model does

End-of-life wind turbine blades are glass-fibre-reinforced polymer. Recycling usually burns off the pyrolysis char to recover clean fibres. This model studies the opposite route: the char is kept on the fibres and used as a solar absorber, the blade foam core is used as the float, and blade laminate is used as a perforated ballast plate. The code links pyrolysis conditions to absorber optics, evaporation, salt rejection, brine return through the feed channels and buoyancy.

Sub-models:

- single-step resin pyrolysis kinetics with Kissinger calibration
- char-coat thickness and Kubelka–Munk optics under the AM1.5G spectrum
- surface heat and mass balance with salinity-dependent water activity
- analytical salt balance of a square channel cell, with radial bounds and a 1-D strip for comparison
- salt-conserving brine return through the channels (continuous or intermittent convection)
- capillary supply, buoyancy and ballast
- design screening under three rules (nominal, Rayleigh-robust, combined-robust)
- Monte Carlo uncertainty analysis, model-form scenarios and numerical verification

All results are model predictions for an idealised material. They have not been validated experimentally.

## Requirements

- MATLAB R2022b, or GNU Octave 8.4.0. Core functions only; no toolboxes are needed.
- Python 3 with Matplotlib, only for the Figure 1 schematic.

## How to run

```matlab
cd matlab
main_CR_WTB_evaporator     % all numbers, Tables 4-8 and Figures 2-7 (about 15 min)
convergence_check          % 5000-sample Monte Carlo check of Section 4.6 (about 10 min)
```

```bash
cd figure1
python3 fig1.py 17.1       % Figure 1 schematic; 17.1 is the ballast thickness in mm
```

Outputs:

| File | Content |
|---|---|
| `matlab/results.txt` | every number reported in the text and in Tables 4–8 |
| `matlab/convergence_results.txt` | 5000-sample convergence check |
| `figures/Figure_1.png` … `Figure_7.png` | figures at 600 dpi |

## Reproducing the published values

The values reported in the manuscript were generated with GNU Octave 8.4.0 on Linux. Copies of those outputs are in `reference_output/`. Compare your `results.txt` with `reference_output/results.txt`.

- Deterministic results should agree with the reference output.
- Monte Carlo statistics can differ slightly between MATLAB and Octave because the two programs can produce different random-number streams from the same seed (`rng(42)`, `rand('state', 7)`). The sampled input ranges are the same.

## Repository structure

```
matlab/              model code (main script, convergence script, parameters, 27 functions, AM1.5G spectrum)
matlab/README.txt    map from each equation in the paper to the function that implements it
figure1/fig1.py      script for the Figure 1 schematic
reference_output/    results.txt, convergence_results.txt and Figures 1-7 from the published run
```

Main files:

| File | Purpose |
|---|---|
| `main_CR_WTB_evaporator.m` | runs the complete study |
| `params_default.m` | all parameters and their sources (Table 3 of the paper) |
| `mc_ranges.m` | Monte Carlo ranges |
| `evaluate_design.m` | solves the coupled model for one geometry |
| `optimise_combined.m` | selects the recommended design |
| `verify_absorber_pde.m` | independent finite-difference check of the absorber solution |

## Citation

If you use this code, please cite the paper and this repository. Citation details are in `CITATION.cff`, and GitHub shows them under **Cite this repository**.

## Licence

MIT licence. See `LICENSE`.

