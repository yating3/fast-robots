<link rel="stylesheet" href="../index.css" />

# Lab 11: Localization (real)

The goal of this lab is to perform localization with a Bayes filter on the real robot. To implement this, I collected distance data by doing a 360 degree scan at 4 different positions. 

## Simulation

I first tested the given localization implementation by running the simulation code. The behavior was expected. The belief (blue) was close to the ground truth (green). 

Plotted results:

<img src="lab11_simplot.png" width="600" class="left">

## Implementation

Code for performing observation loop on the real robot and recording ToF sensor measurements

```
    async def perform_observation_loop(self, rot_vel=120):
        """Perform the observation loop behavior on the real robot, where the robot does  
        a 360 degree turn in place while collecting equidistant (in the angular space) sensor
        readings, with the first sensor reading taken at the robot's current heading. 
        The number of sensor readings depends on "observations_count"(=18) defined in world.yaml.
        
        Keyword arguments:
            rot_vel -- (Optional) Angular Velocity for loop (degrees/second)
                        Do not remove this parameter from the function definition, even if you don't use it.
        Returns:
            sensor_ranges   -- A column numpy array of the range values (meters)
            sensor_bearings -- A column numpy array of the bearings at which the sensor readings were taken (degrees)
                               The bearing values are not used in the Localization module, so you may return a empty numpy array
        """
        reset_arrays()
        print("Mapping")
        ble.send_command(CMD.SET_PWM_LIM, "122|220")
        ble.send_command(CMD.SET_CAL, "1.15")
        ble.send_command(CMD.START_MAPPING, "0.0025|1200|20")

        await asyncio.sleep(40)

        ble.send_command(CMD.STOP_MAPPING, "")
        print("Sending Data")
        ble.send_command(CMD.SEND_MAP_DATA, "")

        while (len(dist_arr) < 18):
            await asyncio.sleep(5)
        
        sensor_ranges = (np.array(dist_arr)/1000).reshape(-1,1)
        sensor_bearings = (np.array(yaw_arr)).reshape(-1,1)
        
        return sensor_ranges, sensor_bearings
```

## Results 

### (-3,-2)

<img src="lab11_data1.png" width="600" class="left">
<img src="lab11_plot1.png" width="600" class="left">

### (5,3)

<img src="lab11_data2.png" width="600" class="left">
<img src="lab11_plot2.png" width="600" class="left">

### (0,3)

<img src="lab11_data3.png" width="600" class="left">
<img src="lab11_plot3.png" width="600" class="left">

### (5,-3)

<img src="lab11_data4.png" width="600" class="left">
<img src="lab11_plot4.png" width="600" class="left">
