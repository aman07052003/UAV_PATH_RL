# UAV Path Optimization using Reinforcement Learning in 3D Non-Terrestrial Networks

## Overview
This project addresses the problem of **optimal path planning** for a UAV navigating in a 3D environment with **communication and sensing constraints**. We use **Deep Q-Networks (DQNs)** to train an agent to navigate toward a goal while maximizing communication signal strength, avoiding obstacles, and conserving energy.

---

## Phase 1: Environment Setup and Data Generation

### Objective:
- Simulate a **3D environment** for UAV movement within a Non-Terrestrial Network (NTN).
- Generate synthetic user locations, obstacles, and SNR maps.

### Files Involved:
- `env/uav_env.py`
- `scripts/generate_data.py`

### Key Features:
- **3D Grid**: Defined as `(20, 20, 5)` for X, Y, and Z coordinates.
- **Obstacles**: Cuboidal regions the UAV must avoid.
- **Users**: Randomly placed users on the ground.
- **SNR Map**: Simulates signal strength from randomly placed sources.

### Output:
- `users.npy`, `obstacles.npy`, `channel_map.npy` in the `data/` folder.

---

## Phase 2: UAV Custom Environment (Gym-Compatible)

### Objective:
- Create a **custom Gym environment** compatible with reinforcement learning algorithms.

### Files Involved:
- `env/uav_env.py`
- `main.py`

### Features:
- **Action Space**: 7 actions (e.g., move up, down, diagonal, stay).
- **Observation Space**: Contains position, battery, SNR, coverage, obstacle risk, and distance to goal.
- **Reward Function**: Tunable weights on SNR threshold, coverage, energy, and collisions.
- **Collision Tracking**: UAV trajectory and invalid moves are tracked and visualized.

### Visualizations:
- `render_2d()` and `render(use_3d=True)` for top-down and 3D plots.

### Output:
- Validates if UAV can reach the goal in simulation and visualize trajectory.

---

## Phase 3: Deep Q-Network (DQN) Based Training

### Objective:
- Train a Deep Q-Learning agent (DQNN) to navigate the UAV effectively.
- Support Dueling DQN, Double DQN, and Prioritized Experience Replay.

### Files Involved:
- `train/dqnn_agent.py`
- `train/q_networks.py`
- `train/train_dqn.py`
- `train/prioritized_replay.py`
- `main.py`

### Network Architecture:
- Input: Observation state vector.
- Hidden Layers: 2–3 fully connected layers (ReLU), size 128 and 64.
- Output: Q-values for each action.

### DQN Features:
- **Dueling DQN**: Separates value and advantage streams.
- **Double DQN**: Uses two networks to reduce overestimation bias.
- **Prioritized Experience Replay (PER)**: Sample important transitions more often.
- **Target Network Update**: Stabilizes training using fixed target net.

### Training Metrics Logged:
- Episode Reward
- Epsilon Decay
- Steps per Episode
- Success (Goal Reached)
- Collisions per Episode
- Coverage Ratio

### Output:
- Trained model saved as `dqnn_uav_model.pth`
- Metrics plotted in `training_metrics.png`

---

## Run Instructions

### 1. Clone and Setup
```bash
git clone <your-repo-url>
cd uav_path_rl
pip install -r requirements.txt
```

### 2. Run Full Pipeline
```bash
python main.py
```

### 3. Evaluate Trained Agent
```bash
python train/test_env.py
```

---

## Project Structure

```
uav_path_rl/
├── data/                   # Stores users, obstacles, SNR maps
├── env/                   # Custom UAVEnv (Gym Environment)
│   └── uav_env.py
├── scripts/               # Data generators
│   └── generate_data.py
├── train/                 # RL Agent + Q-Networks + Training
│   ├── dqnn_agent.py
│   ├── q_networks.py
│   ├── train_dqn.py
│   └── prioritized_replay.py
├── main.py                # Full simulation and training pipeline
└── train/test_env.py      # Inference from saved model
```

---

## Acknowledgements
- Implemented using Python, NumPy, PyTorch, Matplotlib, and OpenAI Gym.
- Inspired by research on UAV path optimization under communication constraints, conducted under the guidance of Dr. Anirudh Agarwal.
