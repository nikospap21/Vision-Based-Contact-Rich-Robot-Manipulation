# Project 2: Vision-Based Contact-Rich Robot Manipulation

This repository contains the complete implementation for Project 2 of the **Intelligent Control** course (Academic Year 2025-2026).  
Developed by **Team 2**:

| Name | Student ID |
| :--- | :--- |
| Mpantekas Nikolaos | 1092562 |
| Papoutsopoulos Nikolaos | 1101000 |
| Stathopoulos Stavros | 1101069 |

---

## 🎯 Objective & System Overview

The objective is to control a 7-DOF **Franka Emika Panda** robotic arm equipped with a **PandaGripper** in a contact-rich assembly task (**NutAssembly**) within the **MuJoCo** simulation engine (via **robosuite**). The robot must execute a sequential closed-loop policy:

1. **Perceive & Grasp:** Locate and securely grasp a nut without privileged ground-truth state data.
2. **Transport & Align:** Transfer the nut above the designated peg and align it with millimeter precision.
3. **Insertion:** Execute contact-rich insertion while handling stiff interface forces and recovery states.

---

## 📂 Repository Structure

```text
├── src/
│   ├── controllers/
│   │   ├── base_controller.py       # Abstract base class for controllers
│   │   └── baseline_controller/     # Heuristic Expert (Finite State Machine)
│   ├── encoders/
│   │   ├── cnn_encoder.py           # Custom NatureCNN visual feature extractor
│   │   └── preprocessing.py         # Image normalization and preprocessing pipeline
│   ├── environments/
│   │   ├── gym_wrapper.py           # Robosuite to Gymnasium API environment wrapper
│   │   └── test_wrapper.py          # Wrapper verification and smoke test script
│   └── models/
│       ├── train_bc.py              # Behavior Cloning training pipeline
│       ├── evaluate_bc.py           # Evaluation script for BC policy
│       ├── train_rl.py              # Autonomous SAC agent training (Iterative Block Training)
│       └── test_rl.py               # Live 3D MuJoCo simulation and performance plotting
├── rl_logs/                         # Training logs, checkpoints, and evaluation plots
├── requirements.txt                 # Software dependencies and libraries
├── DEVELOPMENT.md                   # Implementation timeline and changelog
├── instructions.md                  # Installation and execution instructions
└── README.md                        # Project documentation
```

---

## ⚙️ Installation & Setup

### Option A: Windows (PowerShell)

```powershell
# Clone your fork
git clone https://github.com/nikospap21/Vision-Based-Contact-Rich-Robot-Manipulation.git
cd Vision-Based-Contact-Rich-Robot-Manipulation

# Setup virtual environment
python -m venv ic_env
.\ic_env\Scripts\Activate.ps1

# Install dependencies
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### Option B: Linux (Ubuntu / Debian)

```bash
# System dependencies for MuJoCo rendering
sudo apt-get update
sudo apt-get install -y libgl1-mesa-dev libglib2.0-0 libosmesa6-dev patchelf python3-pip python3-venv

# Clone and setup environment
git clone https://github.com/nikospap21/Vision-Based-Contact-Rich-Robot-Manipulation.git
cd Vision-Based-Contact-Rich-Robot-Manipulation

python3 -m venv ic_env
source ic_env/bin/activate

pip install --upgrade pip
pip install -r requirements.txt
```

---

## 🚀 Execution Pipeline

> **Note:** Ensure your virtual environment (`ic_env`) is activated before running any scripts.

### 1. Environment Verification
```bash
python -m src.environments.test_wrapper
```

### 2. Expert Demonstration Collection
```bash
python -m src.demos.collect_demos
```

### 3. Behavior Cloning (BC) Training & Evaluation
```bash
# Train BC policy
python -m src.models.train_bc

# Evaluate BC policy
python -m src.models.evaluate_bc
```

### 4. Soft Actor-Critic (SAC) Autonomous Learning
```bash
python -m src.models.train_rl
```

### 5. Live Simulation & Evaluation Plot
```bash
python -m src.models.test_rl
```

---

## ⚠️ Academic Scope, Challenges & Limitations

This project is developed as an academic exploration of end-to-end visual policies for contact-rich robotic tasks.

### Primary Engineering Challenges
- **Vision-Based Control:** Privileged ground-truth coordinates are strictly prohibited (`use_object_obs=False`). The policy must infer spatial geometry purely from raw $84 \times 84$ image tensors (`agentview_image`).
- **Contact Dynamics & Jamming:** Sub-millimeter misalignment at the peg-nut interface induces extreme contact forces, requiring active compliance and error recovery.
- **Sample Efficiency & Convergence:** Direct pixel-to-torque/action mapping with Soft Actor-Critic (SAC) demands substantial sample complexity under sparse reward signals.

### Future Research Directions
- Multimodal sensor fusion (incorporating wrist force-torque F/T sensing with vision).
- Pre-training visual representations via self-supervised contrastive learning.
- Domain randomization across lighting, friction, and camera angles for robust sim-to-real transfer.
