<link rel="stylesheet" href="../index.css" />

# Lab 12: Inverted Pendulum

In this lab, I implemented inverted pendulum control on my robot. I started the car from an upright position then used closed loop control to stabilize the car in that position.

## Implementation

My approach to implementing the inverted pendulum was very similar to what I did for linear and orientation PID in labs 5 and 6. I used the DMP yaw data to get the angle of my robot. I used the angle that I held it at initially as the setpoint since the DMP was inconsistent and didn't have a universal position. Once this was set, I had it run my modified version of orientation PID. 

PID code:
```
  if (ip_started) {
    while (!ip_yaw_init) {
      if (get_yaw()) {
        o_pid.setpoint_angle = angle;
        ip_yaw_init = true;
      } else { delay(5); }
    }

    if (get_yaw()) {
      //Serial.println(angle);
      //Serial.println(o_pid.setpoint_angle);
      o_pid.run_opid(car_motor, angle);
    }
  }
```

I used my orientation pid function as a starting point. The most signifcant change I made was in how I moved the car based on the u value. If the car was falling backwards, I would have it drive backwards. If it was falling forwards, I would have it drive forwards. 

```
  if (u > 0) {
    car.drive_straight(pwm, 1); 
    // Serial.print("Backward ");
    // Serial.println(pwm);
  }
  else {
    car.drive_straight(pwm, 0);
    // Serial.print("Forward ");
    // Serial.println(pwm);
  }
```

## Testing

I wasn't able to get it to stabilize for a long period of time. I tried changing different variables such as minimum/maximum PWM, Kp, and calibration factor. It was easy for me to manually tune it by sending different inputs through the commands in jupyter lab. The combination that worked the best was Kp = 3.5, calibration = 1.05, minimum PWM = 22, maximum PWM = 50.

<video width="480" height="310" controls loop="" muted="" autoplay="">
    <source src="https://github.com/yating3/fast-robots/raw/refs/heads/main/Lab12/lab12_video1.mov" />
</video>

A more aggressive model:

<video width="480" height="310" controls loop="" muted="" autoplay="">
    <source src="https://github.com/yating3/fast-robots/raw/refs/heads/main/Lab12/lab12_video2.mov" />
</video>

I think I could get it to work better if I added a Kd and Ki factor to my controller. I also noticed that the car would often fall over before it could react. This is likely due to DMP yaw readings not being frequent enough. I tried adjusting my kp values, but it would still under or over react. 
