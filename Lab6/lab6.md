<link rel="stylesheet" href="../index.css" />

# Lab 6: Orientation Control

## Programming Implementation

In order to better organize my code, I created new classes with a header file and cpp file. I created a class for motors and a class for PID. They each have their own variables and functions. This made my ble_arduino.ino file much more concise and easy to navigate. Additionally, I added code in my loop that would cause the car to brake if BLE disconnected. In the previous lab I had the issue of not being able to send a brake command because BLE had disconnected. 

I also ensured that I was still able to send processing bluetooth commands while my controller is running by making my loops non-blocking. This allows me to tune my PID gains and change the setpoint while my robot is running. I implemented new Arduino commands SET_GAINS (sets kp, ki, and kd), SET_ANGLE (sets target angle), SET_CAL (sets calibration factor), and SET_PWM_LIM (sets min and max PWM). This means that I don't have to reupload my code every time I want to change these values. It makes debugging and tuning much easier. This is also helpful for the future because I may need to change the setpoint as my robot is running in response to various obstacles that may necessitate longer or shorter stopping distances. 

New Arduino commands:
```
        case SET_GAINS:
          success = robot_cmd.get_next_value(o_pid.kp);
          if (!success)
              return;
          success = robot_cmd.get_next_value(o_pid.ki);
          if (!success)
              return;
          break;

        case SET_ANGLE:
          success = robot_cmd.get_next_value(o_pid.SETPOINT_ANGLE);
          if (!success)
              return;
          break;

        case SET_CAL:
          success = robot_cmd.get_next_value(car_motor.MOTOR_CAL);
          if (!success)
              return;
          break;

        case SET_PWM_LIM:
          success = robot_cmd.get_next_value(o_pid.MIN_PWM);
          if (!success)
              return;
          success = robot_cmd.get_next_value(o_pid.MAX_PWM);
          if (!success)
              return;
          break;
```

I used bluetooth to send and receive data similar to lab 5. I stored information in arrays as I was running PID. After PID finished running, I called a command that would send it. There are arrays for time, PWM, angle, and error. I received and stored the data using a notification handler in Jupyter lab.

## PID Input Signal

I used the gyroscope to get the yaw angle of the robot. In lab 2, I found that the IMU gyroscope had significant drift, but less noise than the accelerometer. As a result of this bias, errors can start to grow. One way that drift can be minimized is using a complimentary filter that combines both accelerometer and gyroscope data to get a better approximations of position. Another option is to use the onboard DMP. The DMP uses sensor fusion to fuse data fraom the gyroscope, accelerometer, and magnetometer. It's a low power calibration method that doesn't sacrifice performance. A simple way to minimize it is by calculating the bias and subtracting it. This is the method I tried first.

Code snippet from setup:
```
    for (int i = 0; i < 200; i++) {
      if (myICM.dataReady()) {
        myICM.getAGMT();
        gyro_bias += myICM.gyrZ();
      }
      delay(5);
    }
    gyro_bias /= 200.0;
```

The ICM-20948 IMU sensor has limitations such as a maximum rotational velocity of +/- 2000 dps according to the data sheet. I believe that this is sufficient for my application because the robot won't need to rotate this quickly to reach any setpoint. The maximum rotational velocity can be configured to 250, 500, 1000, or 2000 dps. The IMU sensor has advantages like a much higher sampling rate.

## P Controller

I started by creating just a P controller. My implementation was similar to the one for lab 5 except I'm using the IMU to measure angle instead of distance and sending PWM values to my turn turn functions instead of my drive_straight function.

I had to increase my minimum PWM for orientation PID because turning requires more power to overcome static friction as found in lab 4. I set the minimum to a PWM value of 120. For my kp value, I started at 0.1 and slowly increased it until the car responded quickly without overshooting. I decided not to tune ki and kd because I was satisfied with the results I got with just kp. I didn't see any significant oscillations that needed to be dampened and it was getting close to the desired position without a lot of error.

Code snippet from loop:
```
      if (o_pid.pid_started) {
        if (myICM.dataReady()) {
          myICM.getAGMT();  

          float curr_time = millis();
          float dt = (curr_time - prev_time_angle) / 1000.0;
          prev_time_angle = curr_time;

          float new_gyr = myICM.gyrZ() - gyro_bias;

          angle += new_gyr * dt;
        }
          o_pid.run_opid(car_motor, angle);
      }
```


