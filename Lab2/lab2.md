<link rel="stylesheet" href="../index.css" />

# Lab 2: IMU
In this lab I configured the intertial measurement unit 

## Set up the IMU

To set up the IMU, I installed the SparkFun IMU Arduino Library and connected the IMU to the Artemis board using QWIIC connectors. 

### Example Code
I tested the IMU using an Arduino example sketch. AD0_VAL is the last bit of the I2C address. It represents whether the ADR jumper is closed. It's 0 when the jumper is closed and 1 when it isn't. It should be 1 for this lab because the jumper isn't soldered close.

Running the Example1_Basics sketch:
(video of example)

The Serial Monitor shows acceleration, gyroscope, and temperature data. The orientation of the axes is indicaated on the IMU. As I accelerate the sensor in the positive x, y, and z direction relative to gravity, the respective acceleration value increases. As I accelerate in the negative direction, it decreases. As I rotate the sensor in the positive x, y, and z direction, the respective gyroscope value increases. As I rotate in the negative direction, it decreases.


  (Picture of your Artemis IMU connections)
  
### Blink LED on Start-up
I added a loop that blinks the LED three times slowly on start-up as a visual indication that the board is running.
```
for (int i=0; i<3; i++){
    digitalWrite(LED_BUILTIN, HIGH);  // turn the LED on (HIGH is the voltage level)
    delay(1000);                      // wait for a second
    digitalWrite(LED_BUILTIN, LOW);   // turn the LED off by making the voltage LOW
    delay(1000);                      // wait for a second
}
```

## Accelerometer
  Image of output at {-90, 0, 90} degrees for pitch and roll (include equations)

(picture of equation)

0 degree pitch and roll
-90 degree pitch
(pic)
90 degree pitch
pic
-90 degree roll
pci
90 degree roll
pic

```
if (imu_i < imu_size) {
    float acc_pitch = atan2(myICM.accX(), myICM.accZ()) * 180 / M_PI;
    pitch_array[imu_i] = atan2(myICM.accX(), myICM.accZ()) * 180 / M_PI;

    float acc_roll = atan2(myICM.accY(), myICM.accZ()) * 180 / M_PI;
    roll_array[imu_i] = atan2(myICM.accY(), myICM.accZ()) * 180 / M_PI;
    imu_i++;
}
```


```
void printPitchRoll() {
    float acc_pitch = atan2(myICM.accX(), myICM.accZ()) * 180 / M_PI;
    SERIAL_PORT.print("Pitch: ");
    SERIAL_PORT.print(acc_pitch);
    SERIAL_PORT.print("  |  ");

    float acc_roll = atan2(myICM.accY(), myICM.accZ()) * 180 / M_PI;
    SERIAL_PORT.print("Roll: ");
    SERIAL_PORT.print(acc_roll);
    SERIAL_PORT.println();
}

```


Video 
  Accelerometer accuracy discussion
  Noise in the frequency spectrum analysis
    Include graphs for your fourier transform
    Discuss the results

## Gyroscope
  Include documentation for pitch, roll, and yaw with images of the results of different IMU positions
  Demonstrate the accuracy and range of the complementary filter, and discuss any design choices

## Sample Data
  Speed of sampling discussion
  Demonstrate collected and stored time-stamped IMU data in arrays
  Demonstrate 5s of IMU data sent over Bluetooth

## Stunt


The car was pretty responsive to the remote and took a few seconds to get to full speed. The forward/backward and left/right controls can be used together to do different turns. It can also spin in place without much displacement. To flip it, I drove it forward very quickly and changed direction. 
