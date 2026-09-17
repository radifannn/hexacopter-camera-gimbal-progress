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

1. Hexacopter Baseline

Before working on the camera and gimbal, the custom hexacopter was
already working in ArduPilot + Gazebo.

The vehicle uses a HEXA/X configuration:

FRAME_CLASS = 2
FRAME_TYPE  = 1

The hexacopter had already been verified with:

six functioning rotors
ArduPilot SITL
Gazebo
IMU
GPS
takeoff
stable hover
forward movement
reverse movement
landing

The six-rotor motor configuration is considered a known-good baseline.

Do not modify the motor configuration unnecessarily.

2. Step 1 — Camera + Gimbal
Status: COMPLETE

The camera and gimbal were added after the hexacopter itself had
already been validated.

The gimbal model used is:

model://gimbal_small_3d

The gimbal is attached to the hexacopter through:

<joint name="gimbal_joint" type="revolute">
  <parent>hexacopter::base_link</parent>
  <child>gimbal::base_link</child>

The main integration file is:

models/hexacopter_with_ardupilot/model.sdf

3. Camera Orientation

After adding the camera and gimbal, the initial camera orientation
was inspected.

The camera did not initially point in the desired direction for
ground-target tracking.

An attempt was made to modify the internal camera sensor pose.

That modification was later reverted.

The current configuration keeps the reference/default camera
orientation from the gimbal_small_3d model.

Therefore:

the camera is installed and operational
the gimbal is installed and operational
the camera has not been permanently forced into a downward-facing
orientation

The final camera orientation for target tracking should be decided
after the moving ground target exists and the geometry of the tracking
problem is established.

4. Gimbal Control

The gimbal has three degrees of freedom:

Roll
Pitch
Yaw

The ArduPilot control mapping is:

RC6 → Roll
RC7 → Pitch
RC8 → Yaw

The servo functions are:

SERVO9_FUNCTION  = 8
SERVO10_FUNCTION = 7
SERVO11_FUNCTION = 6

The gimbal limits are:

Roll:
  -30° to +30°

Pitch:
  -135° to +45°

Yaw:
  -160° to +160°

Gazebo command topics:

/gimbal/cmd_roll
/gimbal/cmd_pitch
/gimbal/cmd_yaw

Gazebo joints:

gimbal::roll_joint
gimbal::pitch_joint
gimbal::yaw_joint

Each gimbal axis has a Gazebo JointPositionController.

5. Gimbal Testing

All three gimbal axes were manually tested through MAVProxy.

Roll

Commands:

rc 6 1100
rc 6 1900
rc 6 1500

Observed:

1100 → roll left
1900 → roll right
1500 → return to center

Result:

ROLL = WORKING
Pitch

Commands:

rc 7 1100
rc 7 1900
rc 7 1500

Observed:

1100 → pitch down
1900 → pitch up
1500 → return to center

Result:

PITCH = WORKING
Yaw

Commands:

rc 8 1100
rc 8 1900
rc 8 1500

Observed:

1100 → yaw one direction
1900 → yaw opposite direction
1500 → return to center

Result:

YAW = WORKING

6. Important Debugging History

The first attempt to control the gimbal was incomplete.

Only part of the gimbal control was initially configured.

During testing, the gimbal behaved incorrectly and continued rotating
instead of behaving as a controlled position axis.

That configuration was rolled back.

The gimbal was then rebuilt using the complete three-axis control
structure based on the official ArduPilot Gazebo gimbal implementation.

The complete three-axis configuration was successfully tested.

The current configuration should therefore be treated as a known-good
baseline.

Before modifying it, create a backup/checkpoint.

7. Current Known-Good State
Custom HEXA/X hexacopter       ✅
ArduPilot SITL                 ✅
Gazebo                         ✅
Camera                         ✅
Gimbal                         ✅
Roll control                   ✅
Pitch control                  ✅
Yaw control                    ✅
Manual gimbal testing          ✅

The camera + gimbal subsystem is currently working.

8. Files in This Repository

The relevant files are:

models/
├── hexacopter_with_ardupilot/
│   ├── model.sdf
│   └── model.config
│
└── gimbal_small_3d/
    ├── model.sdf
    ├── model.config
    └── meshes/
        ├── base_plate.dae
        ├── yaw_arm.dae
        ├── roll_arm.dae
        └── camera_enclosure.dae

worlds/
└── hexacopter_runway.sdf

These files represent the current simulation context for the camera
and gimbal work.

9. Development Principles

The project is being developed incrementally.

Important rules:

Preserve working subsystems.
Create a checkpoint before modifying a working subsystem.
Make one major change at a time.
Validate SDF/XML before launching Gazebo.
Test subsystems independently.
Avoid unnecessary complexity during early development.
Do not rebuild components that have already been verified.

10. Next Step — Moving Ground Target
Status: NEXT

The next task is to implement a simple moving ground target vehicle.

The first target does not need to be highly realistic.

The purpose of this stage is to create a controlled moving object
that can be observed by the camera and later used for YOLOv8 and
position-estimation experiments.

The target should:

exist on the ground plane
move along a known path
have reproducible/scriptable motion
be visible from the camera
eventually provide its own GPS state

The target should remain simple during the first implementation.

Do not start YOLOv8 yet.

First establish:

Working Hexacopter
      +
Working Camera
      +
Working 3-Axis Gimbal
      +
Moving Ground Target

Only after that should YOLOv8 be integrated.

11. Development Sequence
STEP 1
Camera + Gimbal
      ✅ COMPLETE
          ↓
STEP 2
Moving Ground Target
      ← NEXT
          ↓
STEP 3
YOLOv8 Detection
          ↓
STEP 4
EKF Target Position Estimation
          ↓
STEP 5
Future MPC Integration

12. Handoff Summary

This is an already-working simulation.

The following are NOT tasks to rebuild:

HEXA/X motor configuration
camera installation
gimbal installation
gimbal roll control
gimbal pitch control
gimbal yaw control

The current task is:

STEP 2 — ADD A SIMPLE MOVING GROUND TARGET
