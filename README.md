# logicool

The `logicool` ROS package provides a simple launch file to integrate Logitech USB webcams with ROS Noetic. This package relies on the `usb_cam` ROS package to interface with the webcam and stream its feed to the ROS ecosystem.

## Dependencies

Before using the `logicool` package, ensure that the following packages are installed:

1. **usb_cam** - ROS package to interface with USB cameras.
2. **camera_calibration** - ROS package for camera calibration.

## Installation

### Step 1: Install `usb_cam` and `camera_calibration`

You can install the necessary packages using the following commands:

```bash
sudo apt-get update
sudo apt-get install ros-noetic-usb-cam
sudo apt-get install ros-noetic-camera-calibration
```

### Step 2: Clone and Build the `logicool` Package

1. Navigate to your ROS workspace's `src` directory:

    ```bash
    cd ~/catkin_ws/src
    ```

2. Clone the `logicool` repository:

    ```bash
    git clone https://github.com/bastien-muraccioli/logicool.git
    ```

3. Navigate to the workspace root and build the package:

    ```bash
    cd ~/catkin_ws
    catkin_make
    ```

4. Source the setup file:

    ```bash
    source devel/setup.bash
    ```

## Usage

### Launching the Camera

To start streaming from your Logitech USB webcam, use the provided launch file:

```bash
roslaunch logicool usb_cam.launch
```

You can view the camera feed using the `rqt_image_view` tool (optional but recommended for verification):

```bash
rosrun rqt_image_view rqt_image_view
```

## Camera Calibration Tutorial

To calibrate your camera, follow these steps:

### Step 1: Launch the Camera

Open a terminal and execute the following commands to start the camera:

```bash
cd ~/catkin_ws
source devel/setup.bash
roslaunch logicool usb_cam.launch
```

### Step 2: Open a Second Terminal for Calibration

In a new terminal, start the calibration tool:

```bash
cd ~/catkin_ws
source devel/setup.bash
rosrun camera_calibration cameracalibrator.py --size 8x6 --square 0.025 image:=/usb_cam/image_raw camera:=/usb_cam
```

Follow the instructions to move the calibration pattern in front of the camera in various directions to capture sufficient data.

### Step 3: Save and Apply Calibration Data

After completing the calibration process:

1. Create a directory to store the calibration data:

    ```bash
    mkdir calibration
    cd calibration
    ```

2. Move and extract the calibration data file:

    ```bash
    mv /tmp/calibrationdata.tar.gz .
    tar -xzf calibrationdata.tar.gz
    ```

3. Update the calibration file:

    ```bash
    sed -i 's/narrow_stereo/usb_cam/g' ost.yaml
    cp ost.yaml ~/.ros/camera_info/usb_cam.yaml
    ```

4. Clean up:

    ```bash
    cd ..
    rm -r calibration
    ```

You should now have a calibrated camera ready for use in your ROS Noetic environment.
