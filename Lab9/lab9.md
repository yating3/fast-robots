<link rel="stylesheet" href="../index.css" />

# Lab 9: Mapping

The objective of this lab is to build a map of a room by collecting ToF recordings as the robot spins 360 degrees on axis. I decided to collect this data using PID orientation control. 

## Implementation

I created a new command START_MAPPING that would set a flag to begin rotations and data collection in the loop.
