# Hexacopter Camera & Gimbal Development Progress

This repository is a development handoff for the current camera and
gimbal integration of a custom ArduPilot + Gazebo HEXA/X hexacopter.

The purpose of this repository is to preserve the current working
state and provide Claude with the context needed to continue the
project without rebuilding or unnecessarily modifying components that
have already been verified.

---

## Overall Project

The broader project is a moving ground-target tracking and position
estimation system using a hexacopter.

The planned development sequence is:

```text
Step 1 — Camera + Gimbal
        ↓
Step 2 — Moving Ground Target
        ↓
Step 3 — YOLOv8 Detection
        ↓
Step 4 — Target Position Estimation using EKF
        ↓
Step 5 — Future MPC Integration"

current progress:
Step 1 — Camera + Gimbal         COMPLETE
Step 2 — Moving Ground Target    NEXT
Step 3 — YOLOv8                  NOT STARTED
Step 4 — EKF                     NOT STARTED
Step 5 — MPC                     FUTURE
