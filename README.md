# UnderactuatedPeopleFollowingRobotMastersProject
This repository contains documentation of an underactuated person following robot that utilizes UWB for a masters capstone project.

The software for project is still an ongoing endeavour, the source code is not included in this github repository at this time.


This robot was designed mostly during 2023 under both Dr. Liwen Shih (Capstone Advisor) and Dr. Harman (Mobile Robots Course CENG 5437) at UHCL. 

## Motivations

The motivation of this project is to develop an autonomous, person following robot that can carry objects while someone is performing activities in both an outdoor or indoor setting and on smooth or rough terrain.  The robot that was developed by the end of this project is only a prototype and isn't designed for manufacturing.  It's more of a robust frame that can hold enough sensors, and other internals to testbed different software solutions.  

## Design Overview
The robot was designed to be able to follow a person off-roads and in crowded environments.  To achieve this, a two-wheeled underactuated design was chosen to ambulate the vehicle and Ultra-Wideband (UWB) trackers (four DWM3001CDK modules) were used to locate a person to follow.  


Several hardware design issues that would need to be changed in a re-design of this chassis would include, but not be limited to the following:
  1. The use of a Phase-Difference of Arrival (PDOA) UWB tracking system.  This type of UWB tracking scheme is much more compact than Time-of-Flight (TOF) UWB tracking.  Unfortunately, commercial modules for PDOA tracking are an order of magnitude more expensive than TOF UWB tracking modules.
  2. The use of other manufauring materials and methods besides FDM 3D printing.  The sole use of FDM printing (due to a lack of alternatives, such as subtractive techniques) drove up the final materials cost significantly.
  3. The inclusion of an adjustable and/or smaller gearing/belt system.  The belt system included in this robot was designed to conform to the load and speed requirements during the conception of this project, but in retrospect the produced design is bulky and 
  
  


## Hardware Design








