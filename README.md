# Binary Continuous Fractionating Column Design (CS₂–CCl₄)

[![Engineering Tool: Microsoft Excel](https://img.shields.io/badge/Excel-Advanced%20Modeling-217346?logo=microsoftexcel&logoColor=white)](#)
[![Domain: Chemical Engineering](https://img.shields.io/badge/Domain-Separation%20Processes-blue)](#)
[![Method: McCabe-Thiele](https://img.shields.io/badge/Method-McCabe--Thiele%20%26%20Fenske-orange)](#)

## Overview
This repository contains a rigorous design and stage-computation model for a continuous atmospheric fractionating column separating a binary mixture of **carbon disulfide ($\text{CS}_2$)** and **carbon tetrachloride ($\text{CCl}_4$)**.

The model solves component material balances, feed-condition thermal analysis ($q$-line parameterization), stage-stepping via the **McCabe-Thiele graphical method**, minimum reflux ratio determination ($R_m$), total reflux analysis ($N_{\text{min}}$), and cross-verification using the analytical **Fenske equation**. It also explores column sensitivity across subcooled liquid, saturated liquid, and saturated vapor feed states.

---

## Process Specifications & Operating Basis

| Parameter | Specification / Value | Basis / Unit |
| :--- | :--- | :--- |
| **Operating Pressure** | Atmospheric ($101.325\text{ kPa}$) | — |
| **Light Key (A)** | $\text{CS}_2$ ($M_A = 76\text{ kg/kmol}$) | Volatile Component |
| **Heavy Key (B)** | $\text{CCl}_4$ ($M_B = 154\text{ kg/kmol}$) | Heavy Component |
| **Feed Composition ($z_F$)** | $20.0\text{ wt\% }\text{CS}_2$ ($33.62\text{ mol\%}$) | $100\text{ kg/h}$ Basis |
| **Distillate Composition ($y_D$)** | $96.0\text{ wt\% }\text{CS}_2$ ($97.99\text{ mol\%}$) | Overhead Product |
| **Bottoms Composition ($x_W$)** | $2.0\text{ wt\% }\text{CS}_2$ ($3.97\text{ mol\%}$) | Bottom Waste ($98\text{ wt\% }\text{CCl}_4$) |
| **Feed Conditions** | $T_F = 325\text{ K}$, $T_B = 360\text{ K}$ | $C_P = 2.25\text{ kJ/(kg}\cdot\text{K)}$ |
| **Latent Heat of Vaporization ($\lambda$)** | $28,725\text{ kJ/kmol}$ | — |
| **Operating Reflux Ratio ($R$)** | $4.50\text{ kmol/kmol}$ | — |
| **Overall Plate Efficiency ($\eta$)** | $75.0\%$ | — |

---

## Design Methodology & Equations

### 1. Mass-to-Mole Balance Transformations
Feed, distillate, and bottoms mass fractions ($w_i$) were converted to molar coordinates:
$$x_A = \frac{\frac{w_A}{M_A}}{\frac{w_A}{M_A} + \frac{w_B}{M_B}}, \quad \bar{M} = \sum x_i M_i$$

### 2. Thermal Condition of Feed ($q$-Factor)
The feed enters at $325\text{ K}$, below the bubble point ($360\text{ K}$):
$$q = 1 + \frac{C_{P,\text{mol}} (T_B - T_F)}{\lambda} = 1.3503 \quad (q > 1 \implies \text{Subcooled Liquid})$$
- **$q$-Line Equation:**
  $$y = \frac{q}{q-1}x - \frac{z_F}{q-1} = 3.855x - 0.9599$$

### 3. Operating Lines (McCabe-Thiele)
- **Rectifying (Enriching) Section Operating Line (ROL):**
  $$y = \frac{R}{R+1}x + \frac{x_D}{R+1} = \frac{4.5}{5.5}x + \frac{0.9799}{5.5} = 0.8182x + 0.1782$$
- **Stripping Section Operating Line (SOL):**
  Constructed from the intersection of the ROL and the $q$-line at $(x', y') = (0.375, 0.485)$ down to $(x_W, x_W) = (0.0397, 0.0397)$.

### 4. Minimum Trays at Total Reflux ($N_{\text{min}}$)
- **Graphical Stepping:** Evaluated between $(x_W, x_W)$ and $(x_D, x_D)$ directly against the $45^\circ$ diagonal.
- **Analytical Verification (Fenske Equation):**
  $$N_{\text{min}} = \frac{\ln\left[ \left(\frac{x_{D,A}}{1 - x_{D,A}}\right) \left(\frac{1 - x_{W,A}}{x_{W,A}}\right) \right]}{\ln(\alpha_{\text{avg}})} - 1$$
  where $\alpha_{\text{avg}} = \sqrt{\alpha_D \cdot \alpha_W} \approx \sqrt{1.764 \times 2.246} = 1.990$.

---

## Key Design Results

| Parameter / Query | Base Case ($q = 1.35$) | Saturated Liquid ($q = 1.0$) | Saturated Vapor ($q = 0.0$) |
| :--- | :--- | :--- | :--- |
| **Feed Thermal State** | Subcooled Liquid | Bubble Point | Dew Point |
| **Minimum Reflux Ratio ($R_m$)** | **$1.32$** | **$1.64$** | **$3.40$** |
| **Operating Reflux Ratio ($R$)** | $4.50$ | $4.50$ | $4.50$ |
| **Ideal Stages (inc. Reboiler)** | **$10.8$ stages** | Check sheet | Check sheet |
| **Theoretical Trays ($N_{\text{ideal}}$)** | **$9.8$ trays** | Check sheet | Check sheet |
| **Actual Trays ($\eta = 75\%$)** | **$13.1 \approx 14$ trays** | Check sheet | Check sheet |
| **Optimal Feed Tray (from top)**| **Tray $7$** | Check sheet | Check sheet |
| **$N_{\text{min}}$ at Total Reflux (Graphical)**| **$7.2$ trays + reboiler**| — | — |
| **$N_{\text{min}}$ via Fenske Analytical**| **$10.27$ theoretical stages**| — | — |

---

## File Contents
- `models/CS2-CCl4_Fractionation_Design.xlsx`: Complete dynamic spreadsheet model featuring:
  - Material balance tables and molecular weight calculations.
  - VLE equilibrium curve and $q$-line formulation.
  - Interactive McCabe-Thiele stage stepping diagrams with reboiler attribution.
  - Comparative sheets evaluating alternative thermal feed conditions ($q = 0$ and $q = 1$).
