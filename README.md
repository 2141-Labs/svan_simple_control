# svan_simple_control

## Setup Instructions

### Ubuntu Setup
```bash
# Install ROS dependencies (if not already installed)
sudo apt update
sudo apt install -y python3-catkin-tools python3-rosdep ros-noetic-catkin

# Create and initialize workspace (if you don't have one already)
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws
catkin init

# Clone the repositories
cd ~/catkin_ws/src
git clone https://github.com/orionop/svan_simple_control.git
# Note: If you encounter issues with the repository URLs, check with the maintainer for the correct URLs

# Install dependencies
cd ~/catkin_ws
rosdep update
rosdep install --from-paths src --ignore-src -r -y

# Build the packages
catkin build svan_simple_control_msgs
source devel/setup.bash
catkin build svan_simple_control
source devel/setup.bash

# Optional: Add to your .bashrc for convenience
echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

### Setup (simulation)
```bash
# cd into workspace
cd ~/xMo/src

# clone packages
git clone https://github.com/orionop/svan_simple_control.git
# For the messages package, contact repo maintainer for correct URL

# add to build script
cd ~/xMo
echo "catkin build svan_simple_control_msgs" >> svan_build.sh
echo "source devel/setup.bash" >> svan_build.sh
echo "catkin build svan_simple_control" >> svan_build.sh
echo "source devel/setup.bash" >> svan_build.sh

# build packages
catkin build svan_simple_control_msgs
catkin build svan_simple_control
source devel/setup.bash
```

### Setup (hardware)
```bash
# cd into workspace
cd ~/dev/xMo/src

# clone packages
git clone https://github.com/orionop/svan_simple_control.git
git clone https://github.com/kry0sc0pic/svan_simple_control_msgs.git

# add to build script
cd ~/dev/xMo
echo "catkin build svan_simple_control_msgs" >> svan_build.sh
echo "source devel/setup.bash" >> svan_build.sh
echo "catkin build svan_simple_control" >> svan_build.sh
echo "source devel/setup.bash" >> svan_build.sh

# build packages
catkin build svan_simple_control_msgs
catkin build svan_simple_control
source devel/setup.bash
```

## Running (simulation)
after starting the gazebo simulation launch file and the mcp.py file. run the following in a third terminal

```bash
cd ~/xMo
source devel/setup.bash
rosrun svan_simple_control simulation.py
```

_now open 4th terminal are run your script. replace `<script>` with the filename of the example you want to run._
```bash
cd ~/xMo
source devel/setup.bash
python3 src/svan_simple_control/examples/<script>.py
```

### Running (hardware)
after completing all the startup steps (joystick calibration, sleep calibration, moetus interface launch and starting the `mcp.py`). run the following commands in two ssh sessions on the SVAN.

_in the first ssh session_
```bash
cd ~/dev/xMo
source devel/setup.bash
rosrun svan_simple_control hardware.py
```

_in the second ssh session_
```bash
cd ~/dev/xMo
source devel/setup.bash
python3 src/svan_simple_control/examples/<script>.py
``` 

## Message definitions
these are the following variables you can give as part of the `SvanCommand` message. You can view the source by contacting the repo maintainer.

0. `command_type` (`uint8`) - what aspect you want to control

    | value | aspect |
    | ---- | --- |
    | 0 | OPERATION MODE (trot, push, up, etc.) |
    | 1 | LINEAR MOVEMENT |
    | 2 | ROLL ANGLE |
    | 3 | PITCH ANGLE | 
    | 4 | YAW VELOCITY |
    | 5 | HEIGHT |

1. `operation_mode` (`uint8`) - operation mode to switch to. used when `command_type` is `0`.

    | value | mode |
    | --- | --- |
    | 1 | STOP |
    | 2 | TWIRL |
    | 3 | PUSHUP |
    | 4 | TROT |
    | 5 | SLEEP |






2. `height` (`uint8`) - height profile to set. used when `command_type` is `5`.

    | value | height |
    | --- | --- |
    | 1 | UP (MAX) |
    | 2 | DOWN (MIN) |

    _granular height control is being actively developled_

3. `vel_x` (`float32`) (`-1.0` - `1.0`) - normalised value of velocity in x-axis. postive x-axis is the right of the robot.

4. `vel_y` (`float32`) (`-1.0` - `1.0`) - normalised value of velcity in y-axis. positive y-axis is the front of the robot.

5. `roll` (`float32`) - normalised roll angle. used when `command_type` is `2`.

    | value | roll angle |
    | --- | --- |
    | -1 | LEFT |
    | 0 | CENTER |
    | 1 | RIGHT |

6. `pitch` (`float32`) - normalised pitch angle. used when command_type is `3`.

    | value | roll angle |
    | --- | --- |
    | 1 | FRONT |
    | 0 | CENTER |
    | -1 | BACK |

7. `yaw` (`uint8`) - yaw direction. used when command_type is `4`.

    | value | direction |
    | --- | --- |
    | 0 | LEFT |
    | 1 | RIGHT |
    | 2 | NONE |


//TODO: update examples

## examples

### sine wave circle

moves in a circular path while varying height.

source: `examples/sine_wave_circle.py`

### hand gesture movement control

author: [Atharv Nawale]()

move and stop the svan using hand gestures. powered by mediapipe.

source: `examples/hand_control.py`

### hand gesture height control

author: [Dhruv Shah]()

control the svan's height using gestures. powered by mediapipe.

source: `examples/height_control.py`