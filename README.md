# UnderactuatedPeopleFollowingRobotMastersProject
This repository contains documentation of an underactuated person following robot that utilizes UWB for a masters capstone project.

The software for project is still an ongoing endeavour, the source code is not included in this github repository at this time.


This robot was designed between 2022 and 2023 under both Dr. Liwen Shih (Capstone Advisor) and Dr. Harman (Mobile Robots Course CENG 5437) at UHCL. 

## Motivations

The motivation of this project is to develop an autonomous, person following robot that can carry objects while someone is performing activities in both an outdoor or indoor setting and on smooth or rough terrain.  The robot that was developed by the end of this project is only a prototype and isn't designed for manufacturing.  It's more of a robust frame that can hold enough sensors, and other internal modules to testbed different software solutions.  

## Design Overview
The robot was designed to be able to follow a person off-roads and in crowded environments.  To achieve this, a two-wheeled underactuated design was chosen to ambulate the vehicle and Ultra-Wideband (UWB) trackers (four DWM3001CDK modules) were used to locate a person to follow using 3D trilateration.  The robot is currently utilizing a raspberry pi with ARM powered peripherals and uses a mixture of UART and USB for intra-robot communication and WiFi, Bluetooth, and UWB for communication with computers and peripherals outside the robot.  PID and linearized state equation solutions were investigated for controlling robot movement.  Sensor fusion solutions utilizing complementary and kallman filters were investigated to provide data to control solutions.  A PID solution was used to control two BLDC motors to power the wheels and used feedback from rotary encoders.  ROS was used to organize the code for the robot and for simulation.  FEA in Autodesk Inventor was used to iteratively test small changes in chassis frame parts to test for deformation under load.  OpenSCAD was used to programmatically generate pulley designs for use in the robots pulley system.  White PLA was used for the chassis and black flexible TPU was used for the wheels.  An OAK-D Series 2 camera system was used to implement SLAM for navigation and obstacle avoidance using stereo depth perception.  Both Python, C++, and C were used to program the robot and its peripherals.  Docker was used to containerize and test ROS modules in a desktop environment and allowed for the easy transfer of modules from desktop to robot environments.

Several renders are shown below.
Internal Cutaway | External Cutaway
--- | ---
![Internal Cutaway](https://github.com/user-attachments/assets/2fab7f4c-2b42-42bb-a447-30158af5807a "Internal Cutaway") | ![External Cutaway](https://github.com/user-attachments/assets/7cf34015-fd88-4f54-b5d8-54b4906661b9 "External Cutaway")




Several design issues that would need to be changed in a re-design of this chassis would include, but not be limited to the following:
  1. The use of a Phase-Difference of Arrival (PDOA) UWB tracking system.  This type of UWB tracking scheme is much more compact than Time-of-Flight (TOF) UWB tracking.  Unfortunately, commercial modules for PDOA tracking are an order of magnitude more expensive than TOF UWB tracking modules.
  2. The use of other manufacturing materials and methods besides FDM 3D printing.  The sole use of FDM printing (due to a lack of alternatives, such as subtractive techniques) drove up the final materials cost significantly.
  3. The inclusion of an adjustable and/or smaller volume gearing/belt system.  The belt system included in this robot was designed to conform to the load and speed requirements during the conception of this project, but in retrospect the produced design is bulky and cannot be changed if a different motor choice is desired.
  4. While robust, the wheels on this robot are very heavy (50% infill).  This necessitates slow acceleration of the robot.  Additionally, too fast of an acceleration will cause the chassis to spin instead of the wheels when the robot begins to move from rest.  The solution to this problem is to reduce the mass of the wheels.  The reason this issue was not caught during the initial design of the chassis was that the free body diagrams used to model the robot considered the chassis to be a non-moving entity.  The design issue was caught in simulation, but by then the wheels were already printed.
  5. A more powerful internal processor should be considered.  The raspberry pi was not adequate for performing SLAM using ROS as well as other control tasks using the current software.
  6. The O-Drive used to power the BLDC motors is bulky and takes up lots of space.  Additionally, the hardware interface uses GPIO pins that can shake loose during robot operation.  A custom solution would be more adequate for a finalized design of this robot.
  7. Flexible, 3D printable TPU was used to line the wheels, but due to low friction led to slippage and led to errors in odometry measurements.  A cast solution using 3D printed moulds and a small vacuum  chamber would be a better choice of material for wheel linings.
  8. The OAK-D series 2 stereo cameras have a low resolution (1MP, 1280x800, global shutter) and while the framerate is adequate (120 FPS for stereo cameras) it is not very good at detecting fast moving obstacles far away from the robot (people running at the robot, cars, etc.).  While the OAK-D series 2 module is an economical solution, the cost is prohibitive to place modules on each side (4 sides) of the robot (200$ per module).  Examples of a higher-performing and economical solution would be to use new distance measurement methods using high resolution, low cost, rolling shutter camera modules as well as low-cost LIDAR modules (that are not available for individual commercial use). 
  
## Part/Design Selection
A trade off analysis was performed when choosing the motors, drive scheme, tracking methods, distance measurement sensors, cameras for visual SLAM, encoders, small computers, and encoders in to assist in the implement of this design.

### Motor Selection
After considering different motor types, a BDLC motor was chosen to power the robot due to its efficiency, weight, and noise.  Due to low torque from a BLDC, a gearing or belt system would need to be designed.  Closed loop feedback can easily be performed using an encoder attached to the output shaft.  If backlash is big enough problem, a dual feedback scheme can be used where a high speed magnetic encoder, or similar high-speed encoder, can be attached to the BLDC motor (input shaft) in addition to the encoder attached to the output shaft.  
The finished design did have significant backlash from loose connections between the wheel and wheel attachment point.  This was fixed by printing a tighter fitting connector.
A custom-made axial flux motor solution would be an ideal choice for this robot (higher power density, lower volume), but would greatly increase development time.

### Drive Scheme Selection
Different steering mechanisms were considered, but a differential, two-wheel self-balancing robot with a low center of gravity was chosen because of its simple odometry (differential drive), interesting design, and good turning characteristics.  In addition to this, there has been, historically, a large number of people who have also constructed this kind of robot (an inverted pendulum model) for people follower robots, enabling this robot to potentially draw from a large number of readily available technical resources.  This design is also well suited for outdoor environments due to its high ground clearance and large wheels.

### Tracking Methods
Several UWB sensors were chosen to be used to track a user due to their simplicity and readily availability at a low price.  Other close contenders were Bluetooth and WLAN methods, but each of these methods were not purpose built specifically for tracking and would be harder to implement effectively.  Other methods investigated were divided into camera and non-camera based methods.  Camera methods, like those used in the Gita series of people following robots, include Feature Based Tracking, Feature Based learning, and Deep Learning.  Non-camera based methods investigated included IMU based tracking, GPS, RFID, WiFi, Bluetooth, and UWB.

### Distance Measurement Sensors
A stereo/monocular based solution with paired IMU was chosen due to its ready availability (compared to 3D LIDAR sensors), low comparative cost to methods of comparable effectiveness (LIDAR/RADAR), longer detection range compared to SONAR, and greater effectiveness in sunlight compared to IR sensors.

### Camera Selection
After much consideration, the Oak-d s2 was chosen due to its higher resolution stereo cameras (1280x800 px) at a high framerate (120 fps) at a reasonable price (200$), which was cheaper than purchasing two better individual camera modules that had both global shutters and sync pins.  It’s also noted that there are several cheaper options for comparable stereo camera modules, but there were less examples on how to use these cameras (less documentation), so the camera module with a higher amount of support was chosen.

### Encoder Selection
The CUI AMT10E, an incremental 5120 pulse per revolution encoder, was chosen since it was relatively cheap (~25$ each) compared to similarly performing encoders and was easy to implement, unlike a magnetic encoder which would need it’s own custom PCB to implement.  Other designs, such as an optical encoder or low pulse-per-revolution quadrature encoder, were not considered due to being more complex and expensive for the former, and too inaccurate for the later.  Additionally, the CUI encoder could also operate at the maximum output shaft output RPM.

### IMU Selection
Inertial Measurement Units (IMUs) were chosen by calculating the effective number of bits (ENOB) of their accelerometer and gyroscope outputs as well as their Zero-G output, and noise.  A low Zero-G output is needed since it defines the smallest accelerometer and gyroscopic values the IMU can measure.  A low noise value determines how much filtering is needed for the output values.  Lower noise values also mean that smaller samples can be taken from the sensor output.  The ENOB can be estimated using the sensors THD and SNR values.  A higher ENOB value is useful for distinguishing the sensors signal value from noise during filtering.  A BMI 270 module was chosen due to its low cost, decent ENOB, low Zero-G output, and low noise.

## Hardware Design

The robot was designed as a knee-height differential drive robot with a 40cm wheel radius and able to follow a person jogging up to about 8 mph.  The chassis and wheels are to be printed by a 3D-printer in PLA, while the wheels were designed to be printed in TPU, a flexible 3D-printable filament.  







