# PRIS–MatterGen Candidates for High-Incompressibility Materials

This repository contains computationally generated inorganic crystal
structures obtained as part of the study:

**“Acoplamiento de reglas PRIS y aprendizaje automático para el
descubrimiento acelerado de materiales superduros”**

**Author:** Gabriel Epuyao Barrientos  
**Affiliation:** Doctorado en Ingeniería, Facultad de Ingeniería y Ciencias,
Universidad de La Frontera, Chile.

## Overview

A total of 1,000 hypothetical inorganic crystal structures were generated
using MatterGen conditioned toward high bulk modulus.

The generated structures were screened using PRIS
(8 Structural Plausibility Rules for Inorganic Solids).

- Initial generated structures: 1,000
- PRIS-viable structures: 62
- PRIS retention rate: 6.2%
- Candidates retaining K ≥ 250 GPa after relaxation: 29

The surviving candidates were relaxed using MatterSim and evaluated using
bulk modulus (K) and energy above the convex hull (Ehull).

## Important scientific note

The structures deposited here are computational candidates.

Passing PRIS does **not** demonstrate experimental synthesizability.
Likewise, high bulk modulus does **not** by itself demonstrate
superhardness.

The structures require further validation using higher-fidelity electronic
structure calculations, full elastic tensors, hardness models and
experimental synthesis/characterization.

## Repository contents

`candidatos_viables/`
: CIF files for the 62 structures that passed PRIS screening.

`top10_candidates.json`
: Data for the ten highest-priority candidates discussed in the poster.

`PRIS_rules.md`
: Definition of the eight PRIS screening rules.

## Top-ranked computational candidate

The current ranking identifies CoRe9(BW)5 as the highest-priority
candidate, with:

- Bulk modulus: 342.61 GPa
- Energy above hull: 0.0000 eV/atom
- Space group: Pm (#6)

These values correspond to the computational workflow used in this study
and should not be interpreted as experimental validation.

## Citation

If you use these structures, please cite this repository and the associated
workshop contribution.
