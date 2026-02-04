<link rel="stylesheet" href="../index.css" />

# Lab 2: IMU

Lab Tasks

## Set up the IMU

To set up the IMU, I installed the SparkFun IMU Arduino Library and connected the IMU to the Artemis board using QWIIC connectors. I tested the IMU using an Arduino example sketch. 

Running the Example1_Basics sketch:
(video of example)

AD0_VAL is the last bit of the I2C address. It represents whether the ADR jumper is closed. It's 0 when the jumper is closed and 1 when it isn't. It should be 1 for this lab because the jumper isn't soldered close.

  Picture of your Artemis IMU connections
  Show that the IMU example code works
  
  Acceleration and gyroscope data discussion (pictures recommended) Check out the change in sensor values as you rotate, flip, and accelerate the board. Explain what you see in both acceleration and gyroscope data.

I added a loop that blinks the LED three times slowly on start-up as a visual indication that the board is running.
```
for (int i=0; i<3; i++){
    digitalWrite(LED_BUILTIN, HIGH);  // turn the LED on (HIGH is the voltage level)
    delay(1000);                      // wait for a second
    digitalWrite(LED_BUILTIN, LOW);   // turn the LED off by making the voltage LOW
    delay(1000);                      // wait for a second
}
```

Accelerometer
  Image of output at {-90, 0, 90} degrees for pitch and roll (include equations)
  Accelerometer accuracy discussion
  Noise in the frequency spectrum analysis
    Include graphs for your fourier transform
    Discuss the results

Gyroscope
  Include documentation for pitch, roll, and yaw with images of the results of different IMU positions
  Demonstrate the accuracy and range of the complementary filter, and discuss any design choices

Sample Data
  Speed of sampling discussion
  Demonstrate collected and stored time-stamped IMU data in arrays
  Demonstrate 5s of IMU data sent over Bluetooth

Record a Stunt
  Include a video (or some videos) of you playing with the car and discuss your observations
