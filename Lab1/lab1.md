<link rel="stylesheet" href="../index.css" />

# Lab 1: Artemis and Bluetooth

## Lab 1A
In order to set up my computer for Lab 1, I installed the Arduino IDE and updated Python. Then, I installed necessary packages and activated the virtual environment. I then connected the Artemis Nano to my computer. I ensured that I was able to communicate with the board and that the components worked by running several example sketches.

### 1. Blink
The blue LED on the board flashes.
<video width="480" height="310" controls loop="" muted="" autoplay="">
    <source src="https://github.com/yating3/fast-robots/raw/refs/heads/main/Lab1/lab1_blink.mov" />
</video>

### 2. Serial
Message are echoed back in the Serial Monitor.

<img src="lab1_serial.png" width="600" class="left">

### 3. Analog Read
The temperature reading increases when I hold the board in my hand.
<video width="480" height="310" controls loop="" muted="" autoplay="">
    <source src="https://github.com/yating3/fast-robots/raw/refs/heads/main/Lab1/lab1_temp.mov" />
</video>

### 4. Microphone Output
Making noise raises the frequency of the microphone input.
<video width="480" height="310" controls loop="" muted="" autoplay="">
    <source src="https://github.com/yating3/fast-robots/raw/refs/heads/main/Lab1/lab1_freq.mov" />
</video>

## Lab 1B
This part of the lab focused on establishing a bluetooth connection between my computer and the Artemis board. This is done using Bluetooth Low Energy (BLE) which is analogous to a bulletin board with computers acting as community members reading the bulletin board. The Artemis board acts as the bulletin board which the computer reads from. This lab also uses a codebase that holds the functions that will be used. The file ble_arduino.ino contains the code that will run the Artemis board.

### Update Artemis MAC Address
I got the MAC address by burning the ble_arduino.ino sketch. I then updated the MAC address in connections.yaml.

Arduino IDE Serial monitor output:
```
Advertising BLE with MAC: c0:81:d5:22:a9:64
```
### Change BLEService UUID
I generated the UUID in jupyter. I then updated the UUID in ble_arduino.ino and connections.yaml.

Jupyter lab output:
```
UUID('54d98045-b89c-437c-b219-ad830cef9fff')
```

### Bluetooth Connection
After setting everything up, I was able to establish a bluetooth connection.

<img src="lab1_bluetooth.png" width="600" class="left">

### Task 1
Send a string from the computer to the Artemis board using the ECHO command.

```
case ECHO:

    char char_arr[MAX_MSG_SIZE];

    // Extract the next value from the command string as a character array
    success = robot_cmd.get_next_value(char_arr);
    if (!success)
        return;

    tx_estring_value.clear();
    tx_estring_value.append("Robot says: ");
    tx_estring_value.append(char_arr);
    tx_estring_value.append(" :)");
    tx_characteristic_string.writeValue(tx_estring_value.c_str());
    
    break;
```
Output:
<img src="lab1_t1.png" width="600" class="left">

### Task 2
Send 3 floats to the Artemis board using the SEND_THREE_FLOATS command.

```
case SEND_THREE_FLOATS:
    float float_a, float_b, float_c;

    // Extract the next value from the command string as an integer
    success = robot_cmd.get_next_value(float_a);
    if (!success)
        return;

    // Extract the next value from the command string as an integer
    success = robot_cmd.get_next_value(float_b);
    if (!success)
        return;

    // Extract the next value from the command string as an integer
    success = robot_cmd.get_next_value(float_c);
    if (!success)
        return;

    Serial.print("Three Floats: ");
    Serial.print(float_a);
    Serial.print(", ");
    Serial.print(float_b);
    Serial.print(", ");
    Serial.println(float_c);

    break;
```
<img src="lab1_t2.png" width="600" class="left">
<img src="lab1_t2_out.png" width="400" class="left">

### Task 3
Write a GET_TIME_MILLIS command that returns a string with the time in milliseconds.
```
case GET_TIME_MILLIS:

    tx_estring_value.clear();
    tx_estring_value.append("T: ");
    tx_estring_value.append((int) millis());
    tx_characteristic_string.writeValue(tx_estring_value.c_str());

    break;
```
<img src="lab1_t3.png" width="600" class="left">

### Task 4
Set up a notification handler in Python that receives strings from the Artemis board and extracts the time.
<img src="lab1_t4.png" width="600" class="left">

### Task 5
Use a loop to generate and send the current time in milliseconds to my laptop for it to be processed by the notification handler.
<img src="lab1_t5.png" width="600" class="left">

### Task 6
Create a global array to store time stamps. Create SEND_TIME_DATA which loops through the array sends data to my laptop to be processed. 
```
case SEND_TIME_DATA:   
    for (int i=0; i<size; i++){
        timestamp_array[i] = (int) millis();
    }
    
    for (int i=0; i<size; i++){
        tx_estring_value.clear();
        tx_estring_value.append("T: ");
        tx_estring_value.append(timestamp_array[i]);
        tx_characteristic_string.writeValue(tx_estring_value.c_str());             
    }

    break;
```
<img src="lab1_t6.png" width="600" class="left">

### Task 7
Add an array to store temperature readings corresponding with time. Create GET_TEMP_READINGS which loops through both arrays and sends each temperature reading with a time stamp.
```
case GET_TEMP_READINGS:   
    for (int i=0; i<size; i++){
        timestamp_array[i] = (int) millis();
    }

    for (int i=0; i<size; i++){
        temperature_array[i] = (int) getTempDegF();
    }
    
    for (int i=0; i<size; i++){
        tx_estring_value.clear();
        tx_estring_value.append("T: ");
        tx_estring_value.append(timestamp_array[i]);
        tx_estring_value.append(" Temp: ");
        tx_estring_value.append(temperature_array[i]);
        tx_characteristic_string.writeValue(tx_estring_value.c_str());             
    }

    break;
```
<img src="lab1_t7.png" width="600" class="left">

### Task 8
Discuss the differences between these two methods, the advantages and disadvantages of both and the potential scenarios that you might choose one method over the other. How “quickly” can the second method record data? The Artemis board has 384 kB of RAM. Approximately how much data can you store to send without running out of memory?

## Lab Tasks
Configurations: Show what the relevant configurations, anything that was specifically needed to address the tasks.
Include a brief explanation on what you did and results for each task.
Address all questions posed in the lab.
Include screenshots, screen recordings, pictures, or videos of relevant results (i.e. messages received in Jupyter Notebook, serial terminal print of messages received by Artemis).

## Discussion
Briefly describe what you’ve learned, challenges that you faced, and/or any unique solutions used to fix problems. It is important to keep these writeups succinct. You will not get extra points for writing more words if the content doesn’t contribute to communicating your understanding of the lab material.
