# Project Triskelion: DeepRL for Autonomous Low-Gravity Robotics

*(Note: The core source code for this project is maintained in a private repository for intellectual property protection. This repository serves as a public demonstration hub for academic and professional evaluation. Technical inquiries or access requests can be directed to quantum_quality@bk.ru).*

## 📖 Overview

Project Triskelion features a 3-legged hopping robot trained via Deep Reinforcement Learning to navigate low-gravity terrain[cite: 3]. Built completely end-to-end—from custom CAD to URDF pipelines and PPO controllers—the system is designed for fault-tolerant planetary rover applications[cite: 3]. The architecture is designed to handle varying gravitational environments, scaling from Mars (0.3g) down to the Moon (0.1g) and Ceres (0.029g)[cite: 3]. 

This project extends prior discrete-jump reinforcement learning research (such as ETH Zürich's SpaceHopper) by achieving **goal-directed hopping navigation** under low-gravity constraints[cite: 3].

---

<video src="phaseD_demo_static_fwd3m.mp4" width="100%" controls autoplay loop muted></video>


## 📺 Simulation Demo: Phase D (Goal-Directed Navigation)

<video src="phaseD_demo_static_fwd3m.mp4" width="100%" controls autoplay loop muted></video>

The following demonstration showcases the final Phase D policy. The robot successfully hops from a random spawn point to an arbitrary target located 2 to 5 meters away[cite: 2]. 

* **Environment:** Moon gravity (−0.981 m/s²)[cite: 2].
* **Control:** The 120°-symmetric body is completely omnidirectional; the robot does not need to face the target to navigate effectively[cite: 2].

<!-- Ensure your video file is named correctly and uploaded to the root of this new public repo -->
https://github.com/Kairat11/triskelion-simulation-demo/blob/main/phaseD_demo_static_fwd3m.mp4

---

## 📊 Performance Metrics & Navigation Results

The final policy (`checkpoints/phaseD_v22_latest_79.9M.pt`) was trained using a CleanRL-style PPO architecture within the Newton physics engine[cite: 2]. The reward mechanism successfully drives navigation via distance-reduction (`pos_delta`) rather than strict velocity-tracking[cite: 2].

**Omnidirectional navigation is fully solved:** The policy reaches within 0.5 m of arbitrary 2–5 m targets with exceptional reliability and zero falls[cite: 2].

### Batched Evaluation (128 Parallel Environments)
*Evaluated over 500 steps per environment at Moon gravity.*[cite: 2]

| Target Direction | Arrival Rate (< 0.5m) | Fall Rate | Median Closest Approach |
| :--- | :--- | :--- | :--- |
| **Forward** (3 m, +x) | 96.1%[cite: 2] | 0.0%[cite: 2] | 0.17 m[cite: 2] |
| **Diagonal** (2, 2) | 98.4%[cite: 2] | 0.0%[cite: 2] | 0.15 m[cite: 2] |
| **Right** (3 m, +y) | 100.0%[cite: 2] | 0.0%[cite: 2] | 0.16 m[cite: 2] |
| **Left** (3 m, -y) | 99.2%[cite: 2] | 0.0%[cite: 2] | 0.14 m[cite: 2] |
| **Back** (3 m, -x) | 97.7%[cite: 2] | 0.0%[cite: 2] | 0.16 m[cite: 2] |
| **Near** (1 m, +x) | 100.0%[cite: 2] | 0.0%[cite: 2] | 0.10 m[cite: 2] |

*Note on Directional Bias: Lateral directions (left/right/back) were specifically tested to ensure the 120°-symmetric gait did not suffer from a weak axis; the data confirms no directional bias exists[cite: 2].*

---

## 🚀 Technical Architecture 

The simulation and training pipeline requires no subtask decomposition, utilizing a single policy to handle locomotion, hopping, and procedural terrain traversal[cite: 3].

* **Physics & Kinematics:** 
  * Direct OnShape CAD to URDF preprocessing, ensuring visual geometry matches physics colliders[cite: 3].
  * Simulated utilizing the Newton physics engine for high-fidelity rigid-body contact dynamics[cite: 3].
* **Reinforcement Learning:**
  * CleanRL-style PPO (256/256 MLP actor and critic)[cite: 3].
  * 48-dimensional observation space mapping to 9-dimensional joint offsets[cite: 3].
* **Procedural Environment:** 
  * Features a 12-block heightmap curriculum mixing wave and grid difficulties[cite: 3].
  * Supports multi-gravity validation across regimes spanning a 10× factor[cite: 3].

## 🔬 System Limitations & Next Steps
While the navigation policy is highly robust, the current gait operates as a continuous hopper (airborne approximately 92% of the time)[cite: 2]. The agent reliably reaches the target spatial location, but success is currently defined by arrival rather than executing a full multi-foot parked stance, which the current hopping physics actively fight against[cite: 2]. 

Future development phases include executing gravity sweeps for asteroid environments (Ceres, -0.28 m/s²) and implementing stage 2.5 air-righting for cat-twist flight recovery[cite: 3].
