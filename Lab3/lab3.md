<link rel="stylesheet" href="../index.css" />

# Lab 3: ToF


## Prelab
I chose to use the longer wires for the ToF sensors so that I would have more flexibility with the placement. I used the shorter wires for the Artemis and IMU. I plan to place one ToF on the front of the car and another on the side. This will help me detect obstacles from different sides. 


- Note the I2C sensor address

- Briefly discuss the approach to using 2 ToF sensors

- Briefly discuss placement of sensors on robot and scenarios where you will miss obstacles

- Sketch of wiring diagram (with brief explanation if you want)

## Lab Tasks

### Set up
In order to power the Artemis, I soldered a 750 mAh battery to the Artemis using a JST connector. I then put heat shrink over the exposed wire to isolate it in order to avoid shorting it. 

3. Connect the QWIIC break-out board to the Artemis

(Picture of your ToF sensor connected to your QWIIC breakout board)

4. Connect the first ToF sensor to the QWIIC breakout board.
- You will have to cut one end of a QWIIC cable and solder the other to your sensor. You have two long cables and two short ones, choose wisely.
- Think about which color attaches to SDA/SCL?
- The photo below is an example of a sensor with a cable. Note that the protective film has not yet been removed.

5. Scan the I2C channel to find the sensor
- Go to File->Examples->Apollo3->Wire and open Example05_wire_I2C
- Browse through the code to see how to use i2c commands.
- Run the code. Does the address match what you expected? If not, explain why.

(Screenshot of Artemis scanning for I2C device (and discussion on I2C address))

6. The ToF sensor has three modes (Short, Medium, and Long) that optimize the ranging performance given the maximum expected range. Discuss the pros/cons of each mode, and think about which one could work on the final robot. (Note: medium mode is only available with the Polulu VL53L1X Library).

<pre> .setDistanceModeShort(); //1.3m .setDistanceModeMedium(); //3m .setDistanceModeLong(); //4m, Default </pre>

(Discussion and pictures of sensor data with chosen mode)

7. Test your chosen mode
- Use the “..\Arduino\libraries\SparkFun_VL53L1X_4m_Laser_Distance_Sensor\examples\Example1_ReadDistance” example
- Document your ToF sensor range, accuracy, repeatability, and ranging time
- The figure below is an example from 2020, when students measured the accuracy and repeatability in different lighting conditions, and timing for various code setups (these are not all required tasks for this year), however, we highly recommend generating your plots in the Jupyter notebook to gain more familiarity with the environment, e.g. using matplotlib.

8. Using notes from the pre-lab, hook up both ToF sensors simultaneously and demonstrate that both work.
- Don’t use the Example05_wire code to do this, it works poorly when multiple sensors are attached.

(2 ToF sensors and the IMU: Discussion and screenshot/video of sensors working in parallel)

9. In future labs, it is essential that the code executes quickly, therefore you cannot let your code hang while it waits for the sensor to finish a measurement. Write a piece of code that prints the Artemis clock to the Serial as fast as possible, continuously, and prints new ToF sensor data from both sensors only when available.
- The distanceSensor.checkForDataReady() routine can be called to check when new data is available.
- How fast does your loop execute, and what is the current limiting factor?

(Tof sensor speed: Discussion on speed and limiting factor; include code snippet of how you do this)

10. Finally, edit your work from Lab 1, such that you can record time-stamped ToF data and IMU data for a set period of time, and then send it over Bluetooth to your computer.

11. Include a plot of the ToF data against time.
(Time v Distance: Include graph of data sent over bluetooth (2 sensors))

12. Include a plot of the IMU data against time.
(Time v Angle: Include graph of data sent over bluetooth)
