# ROS2 Autonomous Navigation with VLA (Qwen2.5-VL)

This repository contains a **ROS2 Humble** node that enables a **TIAGo Lite** robot to navigate autonomously in a **Webots** simulation environment using real-time Vision-Language-Action (VLA) reasoning.

## 🤖 System Overview

The node integrates a Large Multimodal Model (Qwen2.5-VL) directly into the robot's control loop. It performs high-level visual reasoning to detect obstacles (e.g., trees, furniture) and generates velocity commands (`Twist` messages) to reach a specified goal (e.g., a door).

### Key Components:
* **Perception:** Real-time image acquisition from the `/Tiago_Lite/Astra_rgb/image_color` topic.
* **VLA Reasoning:** Cloud-based inference via HuggingFace for zero-shot obstacle avoidance.
* **Control:** Dynamic mapping of AI decisions (TURN, FORWARD, GOAL) to ROS2 `geometry_msgs/Twist`.

## 🛠️ Tech Stack
* **Robotics Middleware:** ROS2 Humble
* **Simulation:** Webots R2023b
* **Vision Model:** Qwen2.5-VL-7B-Instruct
* **Language:** Python 3.10
* **CV Bridge:** OpenCV for ROS image transformation

## 🚀 Setup & Execution

1. **Source ROS2 Environment:**
   ```bash
   source /opt/ros/humble/setup.bash
   ```

2. **Launch Webots Simulation**
Start the TIAGo simulation environment:
   ```bash
   ros2 launch webots_ros2_tiago robot_launch.py
   ```
   
3. Start Visualization (Optional)
To monitor the robot's sensors and TF tree via Foxglove:
   ```bash
   ros2 run foxglove_bridge foxglove_bridge
   ```
Then, open the Foxglove Studio and connect to localhost.

4. Run the AI Navigator Node
Execute the main Python script to start the VLA-based control loop:
   ```bash
   python3 tiago_ai_navigator.py
   ```


🎥 Logic Flow & Decision Making
The node operates in a continuous loop:

Subscribe: Captures raw RGB frames from the TIAGo Astra camera.

Inference: Sends the image + prompt to the VLA model to classify the scene.

Actuate: - TURN: If an obstacle (e.g., tree) is detected (Angular Velocity = 1.0 rad/s).

FORWARD: If the path is clear (Linear Velocity = 0.2 m/s).

GOAL: Mission complete.
