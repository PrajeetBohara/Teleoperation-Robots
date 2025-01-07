# Teleoperation-Robots
A comprehensive manual for operating teleoperation robots in windows environment.


create fresh conda environment = conda create -y -n lerobot python=3.10
activate conda with lerobot dependencies = conda activate lerobot
navigate to lerobot cloned folder = cd C:\Users\praje\~\lerobot (for my case)
install lerobot with dependencies for motors = pip install -e ".[feetech]"

AVOID \ while using the commands in command prompt

caliberate follower arm (with camera) = python lerobot/scripts/control_robot.py calibrate --robot-path lerobot/configs/robot/so100.yaml --robot-overrides '~cameras' --arms main_follower
caliberate follower arm (without camera) = python lerobot/scripts/control_robot.py calibrate --robot-path lerobot/configs/robot/so100.yaml --arms main_follower


caliberate leader arm (with camera) = python lerobot/scripts/control_robot.py calibrate --robot-path lerobot/configs/robot/so100.yaml --robot-overrides '~cameras' --arms main_leader
caliberate leader arm (without camera) = python lerobot/scripts/control_robot.py calibrate --robot-path lerobot/configs/robot/so100.yaml --arms main_leader

teleoperate (without camera)
python lerobot/scripts/control_robot.py teleoperate --robot-path lerobot/configs/robot/so100.yaml

connecting camera
1.) Using the laptop's web camera using OpenCV
Get the index of the cameras (If only web camera then by default it is 0 for most cases)
Run following script to get the index
python lerobot/common/robot_devices/cameras/opencv.py --images-dir outputs/images_from_opencv_cameras
This will save the photos taken from webcamera of each frame in the outputs folder. The saved images will have name like "camera_01_frame_000088.png" where camera 01 refers to index of camera is 1.

Now, save the following code in a code editor :
from lerobot.common.robot_devices.cameras.opencv import OpenCVCamera

camera = OpenCVCamera(camera_index=0)
camera.connect()
color_image = camera.read()

print(color_image.shape)
print(color_image.dtype)

eg, lets say save it as camera.py. Now run this command from command prompt
python C:\Users\praje\~\lerobot\my_tests\camera.py --->directory of saved script

output will be like--> (480, 640, 30) uint8

2.) Using a phone as a secondary web camera
-Install droicam in your phone
-Install OBS Studio in laptop
- Install DroidCam OBS plugin in both OBS Studio and the phone
- In OBS Studio, head over to the sources section at the buttom
- Click add, select use WIFI, enter wifi IP displayed in phone (make sure the laptop and phone should be on the same network)
- Enter the resolution to 640 x 480
- Then activate

Teleoperate with camera:
python lerobot/scripts/control_robot.py teleoperate --robot-path lerobot/configs/robot/so100.yaml 
#In case it dont work try removing the camera part in the so100.yaml file.


