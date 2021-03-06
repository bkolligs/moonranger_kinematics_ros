# Moonranger Kinematics Test Node

The purpose of this ROS node is to offer a simple way to swap between kinematic models for testing on Morphin. 

## Running Tests

Tests were written in the `unittest` framework for ease of use with ROS. Run `catkin_make run_tests` in the workspace folder to perform them. 

## Integration with _autonomy_ repo

The main change that needs to happen here is just the subscription and publisher topic names, which I have created as configuration parameters in the (config)[config/kinematics.yaml] file. The main functions of this node are to: 
1. **Navigation Kinematics:** Obtaining body velocity from wheel rates
2. **Actuation Kinematics:** Obtaining wheel rates from body velocity

The node will need subscribe to `/navigator/drive_arc` with message type `RASM_DRIVE_ARC_MSG`. It will need to publish to `/joy` of message type `Joy`. This message type doesn't natively handle four wheel velocities so that'll need to be changed. 

## Parameter setting

There are several parameters that are tunable in order to change performance of the model. Most important are: 
* The length of the vehicle from the center point = 0.2222
* The width of the vehicle from the center point = 0.32195
* The vertical translation (height) of the wheel hubs from the center point = 0.0745 (to the belly) **OR** it is 0.18217 (to the interface plate)
* The wheel radius = 0.0955

Additionally when adjusting slip there are two parameters we can tune: 
1. The wheel radius
2. The vehicle contact frame constraints `vc`

Number [2]	needs to be determined experimentally, as it will describe the movement of the body frame in global coordinates that is caused by the wheel slip. 

## Velocity to motor command
Note that the velocity command coming out of this node is going to be in radians per second, which can be converted into revolutions per minute in order to get the actual speed sent to the motor itself. This still needs to be implemented
