<link rel="stylesheet" href="../index.css" />

# Lab 6: Orientation Control

## Programming Implementation

In order to better organize my code, I created new classes with a header file and cpp file. I created a class for motors and a class for PID. They each have their own variables and functions. This made my ble_arduino.ino file much more concise and easy to navigate. Additionally, I added code in my loop that would cause the car to brake if BLE disconnected. In the previous lab I had the issue of not being able to send a brake command because BLE had disconnected. 

I also ensured that I was still able to send processing bluetooth commands while my controller is running by making my loops non-blocking. This allows me to tune my PID gains and change the setpoint while my robot is running. I implemented new Arduino commands SET_GAINS (sets kp, ki, and kd), SET_ANGLE (sets target angle), SET_CAL (sets calibration factor), and SET_PWM_LIM (sets min and max PWM). This means that I don't have to reupload my code every time I want to change these values. It makes debugging and tuning much easier. This is also helpful for the future because I may need to change the setpoint as my robot is running in response to various obstacles that may necessitate longer or shorter stopping distances. 

I used bluetooth to send and receive data similar to lab 5. I stored information in arrays as I was running PID. After PID finished running, I called a command that would send it. I received and stored the data using a notification handler in Jupyter lab.

## PID Input Signal

You should integrate your gyroscope to get an estimate for the orientation of the robot.
Are there any problems that digital integration might lead to over time? Are there ways to minimize these problems?
Does your sensor have any bias, and are there ways to fix this? How fast does your error grow as a result of this bias? Consider using the onboard digital motion processor (DMP) built into your IMU to minimize yaw drift.
Are there limitations on the sensor itself to be aware of? What is the maximum rotational velocity that the gyroscope can read (look at spec sheets and code documentation on github). Is this sufficient for our applications, and is there was to configure this parameter?

## P Controller

## PI Controller


P/I/D discussion (Kp/Ki/Kd values chosen, why you chose a combination of controllers, etc.)
Range/Sampling time discussion
