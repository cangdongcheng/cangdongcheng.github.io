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

Builds on the DIMON framework to create a fast surrogate that preserves physical constraints.

**DIMON framework:** Parameterises cardiac geometry and learns a solution operator on a unified domain — solving the geometry generalisation problem for PINNs. Does NOT learn diffeomorphic mappings; it parameterises geometry so the operator can generalise across anatomies.

**PINNs:** Physics-Informed Neural Networks encode PDE residuals directly in the loss function, allowing learning without full PDE supervision.

**Current extensions (built on top of DIMON):**
1. Extending to biventricular mesh (original work used simpler geometries)
2. Extending to 12-lead ECG prediction and reaction dynamics, beyond just activation time maps and repolarisation maps

## Tools

openCARP, DIMON, PINNs, Cobiveco, InSilicoHeartGen, LDRB, Python, HPC
