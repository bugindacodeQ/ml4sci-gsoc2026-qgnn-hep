# ml4sci-gsoc2026-qgnn-hep
GSoC 2026 ML4SCI application -- Quantum GNN for LHC jet tagging. Implements SP-VQMP: a Lorentz-equivariant quantum message-passing architecture in PennyLane. Includes test task solutions (Tasks I, II, III, V) and full project proposal.


🔬 Project Overview
The High-Luminosity LHC will generate particle collision data at a scale that strains every classical computing pipeline known today. This project addresses a core challenge in that pipeline: jet tagging — identifying the originating particle (top quark, Higgs boson, W/Z boson, or QCD jet) from the resulting spray of detector hits.
Jets are naturally represented as graphs: each particle constituent is a node, edges encode angular proximity in (η, φ) space, and node features carry kinematic observables. This makes them ideal candidates for Graph Neural Network methods — and, by extension, for Quantum Graph Neural Networks (QGNNs) that perform message passing in Hilbert space.
💡 Novel Contribution: SP-VQMP
This project introduces Symmetry-Preserving Variational Quantum Message Passing (SP-VQMP) — a QGNN architecture whose message-passing ansatz is constrained to be equivariant under Lorentz transformations, directly exploiting the known physical symmetry of LHC data.
Classical equivariant GNNs (LorentzNet, LGNN) enforce Lorentz symmetry in their architecture. Existing QGNNs for HEP do not. SP-VQMP closes this gap inside the quantum circuit itself.
📁 Repository Structure
ml4sci-gsoc2026-qgnn-hep/
│
├── 📓 QMLHEP_GSoC2026_TestTasks_OvieGreatOhwoka.ipynb   ← Main submission notebook
│
├── 📄 README.md                                          ← This file
│
├── proposal/
│   ├── GSoC2026_Proposal_OvieGreatOhwoka.pdf            ← Full project proposal (PDF)
│   └── GSoC2026_Proposal_OvieGreatOhwoka.docx           ← Editable Word version
│
└── assets/
    ├── circuit_1.png                                     ← Task I Circuit 1 diagram
    ├── circuit_2_swap_test.png                           ← Task I Circuit 2 (SWAP test)
    ├── task2_gnn_performance.png                         ← Task II GNN benchmark plots
    └── task5_qgnn_circuit.png                            ← Task V QGNN circuit diagram

    
✅ Task Solutions
This repository contains solutions to the following QML-HEP GSoC 2026 evaluation tasks:
Task I — Quantum Computing (PennyLane)
Circuit 1 — Five-Qubit Circuit:
Hadamard gate on all 5 qubits → superposition
CNOT chain on pairs (0,1), (1,2), (2,3), (3,4) → entanglement
SWAP on qubits (0, 4)
RX(π/2) on qubit 2
Full circuit visualisation with qml.draw_mpl
Circuit 2 — SWAP Test:
Estimates fidelity between two 2-qubit states |q₁q₃⟩ and |q₂q₄⟩
Implements Hadamard → controlled-SWAP (Fredkin) → Hadamard on ancilla
Fidelity recovered as: |⟨ψ|φ⟩|² = 2·P(ancilla=0) − 1
Task II — Classical GNN for Quark/Gluon Jet Classification
Graph Construction Strategy:
Jets are built as kNN graphs in (η, φ) detector space — physically motivated by angular proximity of particle constituents.
Two Architectures Compared:
Model
Architecture
Key Idea
GCN
Graph Convolutional Network
Spectral aggregation baseline
EdgeConv
ParticleNet-inspired
Message = MLP(xᵢ ‖ xⱼ − xᵢ), captures momentum flow
Performance evaluated with ROC-AUC and classification accuracy.
Task III — Open Commentary on Quantum Machine Learning
Personal commentary covering:
The current state of QML and the NISQ era
The Variational Quantum Eigensolver (VQE) and its relevance to HEP
PennyLane vs Qiskit — practical differences for ML workflows
The parameter-shift rule for exact quantum gradients
Motivation for symmetry-constrained quantum circuits (SP-VQMP)
Task V — Quantum Graph Neural Network (QGNN)
Circuit Design:
Node encoding via AngleEmbedding (RY rotations per feature)
Quantum message passing via IsingZZ + IsingYY gates on graph edges
Node update via single-qubit RY + RZ rotations
Readout via Pauli-Z expectation values → classical linear classifier
Hybrid Model: End-to-end trainable with PyTorch autodiff + PennyLane parameter-shift rule.
