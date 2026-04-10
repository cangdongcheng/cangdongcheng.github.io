# FYP — Physics-Informed ECG Surrogate Model for Efficient Cardiac Digital Twin

**Status:** In Progress (Final Year Project, 2025–2026)
**Program:** B.Eng. Biomedical Engineering, NUS

## Problem

Cardiac Digital Twins (CDTs) are personalised virtual replicas of patient hearts used for precision medicine. Current FEM solvers are too slow for real-time clinical use, especially with high-resolution anatomical meshes and complex inverse problems.

## Two-Track Approach

### Track 1 — EP FEM Simulation (openCARP)

Generates high-fidelity ground-truth electrophysiological fields and synthesises 12-lead ECG data for neural network training.

**Mesh processing pipeline:**
1. Surface truncation and standardisation at basal plane (removing valvular structures)
2. Volumetric discretisation via InSilicoHeartGen (tetrahedral volume mesh)
3. Surface labelling and partitioning via Cobiveco coordinate system (basal plane, RV/LV endocardium, epicardium)
4. Anisotropic fiber assignment via LDRB (Laplace-Dirichlet Rule-Based) algorithm

**Simulation:** Ionic models (ten Tusscher–Panfilov) coupled with monodomain/bidomain PDEs, solved via FEM in openCARP. Parallelised across HPC cluster nodes.

### Track 2 — Neural Operator Surrogate (DIMON + PINNs)

Replaces expensive PDE solves with a learned mapping that preserves physical constraints.

**DIMON framework:** Uses diffeomorphic mappings to translate diverse patient-specific cardiac anatomies into a canonical reference template. Enables the surrogate to generalise across different subjects.

**PINNs:** Physics-Informed Neural Networks encode PDE residuals directly in the loss function, allowing learning without full PDE supervision.

**Target:** Real-time 12-lead ECG prediction from patient-specific parameters.

## Tools

openCARP, DIMON, PINNs, Cobiveco, InSilicoHeartGen, LDRB, Python, HPC
