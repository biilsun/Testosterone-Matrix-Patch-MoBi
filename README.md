# Mechanistic Testosterone Matrix Patch Template (MoBi)

![OSP Suite](https://img.shields.io/badge/OSP%20Suite-v12.0-blue)
![MoBi](https://img.shields.io/badge/MoBi-Mechanistic%20Modeling-green)
![License](https://img.shields.io/badge/License-GPL_v2.0-blue)

> ⚠️ **IMPORTANT SCIENTIFIC WARNING:** This repository provides an **uncalibrated architectural proof-of-concept**. The parameter values included (e.g., Higuchi constant, Enhancement Factor = 10000) are structural placeholders intended ONLY to validate ODE solver stability and cross-compartment references. This model **must not** be used for clinical decision-making or regulatory submissions without rigorous, independent Parameter Identification (PI) against *in vivo* data.

## 🎯 Abstract
This repository contains a heavily refactored Transdermal Delivery System (TDS) structural template developed in **Open Systems Pharmacology (MoBi)**. 

The original 40-sublayer skin architecture (based on *Dancik et al.*) has been completely redesigned to simulate **solid polymer matrix patches**, using a Testosterone patch (e.g., Androderm-type architecture) as a structural reference design. 

## 🛠️ Model Modifications

### 1. Matrix Release Kinetics (Higuchi Equation)
The default Fickian diffusion from the vehicle was replaced with Higuchi release kinetics to represent solid matrices. 
* **Lag Time:** A `t_lag_matrix` parameter was introduced to account for matrix wetting time.
* **Thermodynamic Gradient:** The release incorporates the formula `Higuchi_k * Ar / (2 * sqrt((Time/60) + t_lag_matrix)) * ((c_v / K_v_m) - c_sc1)`. This accounts for the mechanistic decrease in the release rate as the stratum corneum lipid pool reaches saturation.

### 2. Vehicle Thermodynamics (`K_v_m`)
The patch-to-skin partition coefficient (`K_v_m`) was changed from an aqueous-based QSPR formula to an independent structural constant, as aqueous QSPR equations are not applicable to solid polymer adhesives.

### 3. Chemical Enhancers
A dimensionless `Enhancement_Factor` (EF) was introduced in the vehicle block. The baseline lipid diffusion coefficient (`D_lip`) in the stratum corneum sublayers is dynamically multiplied by this factor, allowing for permeability adjustments without breaking the underlying baseline QSPR thermodynamics.

### 4. Solver Settings Optimization
To ensure numerical stability for the stiff ODE system during initial structural validation, global CVODE settings were strictified:
* `HMax` = 0.001
* `AbsTol` = 1E-15
* *(Note: Users are advised to relax these tolerances during the Parameter Identification process to improve computational performance).*

## 🎛️ Parameter Values
Key variables are centralized into a single `Parameter Values` building block (`Generic_Matrix_Patch_Profile`):

* `Higuchi_k`: Matrix release rate constant.
* `Enhancement_Factor`: Dimensionless permeability modifier.
* `t_lag_matrix`: Matrix wetting time.
* `K_v_m`: Solid-matrix partition constant.
* `Area of application` & `Mass_dose_per_area`: Patch geometry and drug loading.

## 🚀 Usage
1. **PBPK Coupling:** Export the spatial structure and couple it with a PBPK model in PK-Sim.
2. **Clinical Data Integration:** Import *in vivo* observed data.
3. **Parameter Identification:** Use the MoBi PI module to optimize `Higuchi_k`, `K_v_m`, and `Enhancement_Factor`.

## 👨‍🔬 Authors & Provenance
* **Original Skin Architecture:** This project is a derivative work based on the mechanistic dermal absorption model by *Dancik et al.* The original MoBi project file can be found in the official [OSP Skin-permeation-model Repository](https://github.com/Open-Systems-Pharmacology/Skin-permeation-model). The baseline thermodynamic QSPR architecture corresponds to:
  > *Dancik, Y., Miller, M. A., Jaworska, J., & Kasting, G. B. (2013). Design and performance of a spreadsheet-based model for estimating bioavailability of chemicals from dermal exposure. Advanced Drug Delivery Reviews, 65(2), 221-236.*
* **Matrix Patch Adaptation:** Refactoring, integration of Higuchi kinetics, and model reconfiguration by **Alexander Loginov** (2026).
* ✉️ **Contact:** [biilsun81@gmail.com](mailto:biilsun81@gmail.com)

## ⚖️ Trademark & Liability Disclaimer
Androderm® is a registered trademark of Allergan/Actavis. This independent academic project is not affiliated with, endorsed by, or sponsored by the trademark owners. The reference to the brand is strictly for scientific contextualization of the structural design. The model is provided **"as is"**, without warranty of any kind. The authors assume no liability for any scientific conclusions resulting from the use of this project.
