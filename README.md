# Project 2: Vision-Based Contact-Rich Robot Manipulation

This repository contains the complete implementation for Project 2 of the **Intelligent Control** course (Academic Year 2025-2026).  
Developed by **Team 2**:

| Name                    | Student ID |
| Mpantekas Nikolaos      |   1092562  |
| Papoutsopoulos Nikolaos |   1101000  |
| Stathopoulos Stavros    |   1101069  |

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
│       ├── train_bc.py              # Behavior Cloning training pipeline[cite: 1]
│       ├── evaluate_bc.py           # Evaluation script for BC policy[cite: 1]
│       ├── train_rl.py              # Autonomous SAC agent training (Iterative Block Training)[cite: 1]
│       └── test_rl.py               # Live 3D MuJoCo simulation and performance plotting[cite: 1]
├── rl_logs/                         # Training logs, checkpoints, and evaluation plots[cite: 1]
├── requirements.txt                 # Software dependencies and libraries[cite: 1]
├── DEVELOPMENT.md                   # Implementation timeline and changelog[cite: 1]
├── instructions.md                  # Installation and execution instructions[cite: 1]
└── README.md                        # Project documentation[cite: 1]
```

⚙️ Installation & SetupOption A: Windows (PowerShell)PowerShell# Clone your fork
git clone [https://github.com/nikospap21/Vision-Based-Contact-Rich-Robot-Manipulation.git](https://github.com/nikospap21/Vision-Based-Contact-Rich-Robot-Manipulation.git)
cd Vision-Based-Contact-Rich-Robot-Manipulation

# Setup virtual environment[cite: 1]
python -m venv ic_env[cite: 1]
.\ic_env\Scripts\Activate.ps1[cite: 1]

# Install dependencies[cite: 1]
python -m pip install --upgrade pip[cite: 1]
pip install -r requirements.txt[cite: 1]
Option B: Linux (Ubuntu / Debian)Bash# System dependencies for MuJoCo rendering[cite: 1]
sudo apt-get update[cite: 1]
sudo apt-get install -y libgl1-mesa-dev libglib2.0-0 libosmesa6-dev patchelf python3-pip python3-venv[cite: 1]

# Clone and setup environment
git clone [https://github.com/nikospap21/Vision-Based-Contact-Rich-Robot-Manipulation.git](https://github.com/nikospap21/Vision-Based-Contact-Rich-Robot-Manipulation.git)
cd Vision-Based-Contact-Rich-Robot-Manipulation

python3 -m venv ic_env[cite: 1]
source ic_env/bin/activate[cite: 1]

pip install --upgrade pip[cite: 1]
pip install -r requirements.txt[cite: 1]
🚀 Execution PipelineNote: Ensure your virtual environment (ic_env) is activated before running any scripts[cite: 1].Environment Verification:[cite: 1]Bashpython -m src.environments.test_wrapper[cite: 1]
Expert Demonstration Collection:[cite: 1]Bashpython -m src.demos.collect_demos[cite: 1]
Behavior Cloning (BC) Training & Evaluation:[cite: 1]Bashpython -m src.models.train_bc[cite: 1]
python -m src.models.evaluate_bc[cite: 1]
Soft Actor-Critic (SAC) Autonomous Learning:[cite: 1]Bashpython -m src.models.train_rl[cite: 1]
Live Simulation & Evaluation Plot:[cite: 1]Bashpython -m src.models.test_rl[cite: 1]
⚠️ Academic Scope, Challenges & LimitationsThis project is developed as an academic exploration of end-to-end visual policies for contact-rich robotic tasks.Primary Engineering Challenges:Vision-Based Control: Privileged ground-truth coordinates are strictly prohibited (use_object_obs=False). The policy must infer spatial geometry purely from raw $84 \times 84$ image tensors (agentview_image).Contact Dynamics & Jamming: Sub-millimeter misalignment at the peg-nut interface induces extreme contact forces, requiring active compliance and error recovery.Sample Efficiency & Convergence: Direct pixel-to-torque/action mapping with Soft Actor-Critic (SAC) demands substantial sample complexity under sparse reward signals.Future Research Directions:Multimodal sensor fusion (incorporating wrist force-torque F/T sensing with vision).Pre-training visual representations via self-supervised contrastive learning.Domain randomization across lighting, friction, and camera angles for robust sim-to-real transfer.
