<link rel="stylesheet" href="../index.css" />

# Lab 9: Mapping

The objective of this lab is to build a map of a room by collecting ToF recordings as the robot spins 360 degrees on axis. I decided to collect this data using PID orientation control. 

## Implementation

I chose to use PID orientation control because this would allow me to get more accurate orientation positioning and ToF readings. The robot will be taking readings when the car has settled at the target orientation and is stationary. This yields more reliable distance readings.

In Arduino, I created a new command START_MAPPING that would set a flag to begin rotations and data collection in the loop. I collected data in 15 degree rotation increments. I allowed about 2 seconds of time between measurements to allow the orientation PID to settle and get to the correct angle orientation. 

In my Jupyter notebook, I first sent a orientation PIP command to set it in position 0 before I started. This is helpful as the initial movement was often most unpredictable due to the potentially large angle change. I then sent the mapping command. Once I finished mapping, I sent a command to get the recorded time, ToF, yaw, and setpoint data. I used a notification handler to collect this data then saved it in a CSV file. I was able to get measurements within 1 degree of my setpoint increments. The robot behavior is reliable enough to assume that the readings are spaced equally in angular space.

There were various parameters that I had to tune in order to get the car to turn smoothly, spin on axis, and collect useful data. I manually tuned these paramters by sending  commands with different inputs in Jupyter and observed how the car responded.

Inputs tuned:
- Kp: Since the car would be making small angle changes, I lowered Kp in order to reduce oscillation and overshooting. I settled on a Kp of .
- Minimum PWM for PID: The PWM had to be high enough to be the car to turn. It also needed to be adjusted slightly based on the battery level. I tried to run it on a close to full battery as much as possible for consistency. I chose a minimum PWM of .
- Calibration factor: I needed to multiply my right motor by a calibration factor in order for it to spin at a similar rate to the left motor. This allowed the car to rotate on axis and significantly reduced drift. I chose a value of .
- Time between turns: The time between turns was determined through experimentation. I slowly increased it until the car was settling. The ideal time was 2 seconds.
- Angle between turns: I chose 15 degrees as this would give me a accurate scan without taking too much time. I had to balance resolution, battery capacity, and measurement time.

Loop code:
```

```

Car turning on axis:

## Data Collection

I collected data at 

Mapping area:
[Picture of world]

## Polar Coordinates

I used polar coordinate plots to make sure that the readings matched my expectations. 


## Transformations

I then used transformation matrices to convert my ToF readings to the intertial reference frame of the room. 

```

```



