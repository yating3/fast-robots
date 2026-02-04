<link rel="stylesheet" href="../index.css" />

# Lab 1: Artemis and Bluetooth

## Lab 1A
In order to set up my computer for Lab 1, I installed the Arduino IDE and updated Python. Then, I installed necessary packages and activated the virtual environment. I then connected the Artemis Nano to my computer. I ensured that I was able to communicate with the board and that the components worked by running several example sketches.

### 1. Blink
The blue LED on the board flashes.

### 2. Serial
Message are echoed back in the Serial Monitor.

### 3. Analog Read
The temperature reading increases when I hold the board in my hand.

### 4. Microphone Output
Making noise raised the highest frequency of the microphone input.

## Lab 1B
This part of the lab focused on establishing a bluetooth connection between my computer and the Artemis board. This is done using Bluetooth Low Energy (BLE) which is analogous to a bulletin board with computers acting as community members reading the bulletin board. The Artemis board acts as the bulletin board which the computer reads from. This lab also uses a codebase that holds the functions that will be used. The file ble_arduino.ino contains the code that will run the Artemis board.

### Update Artemis MAC Address
I got the MAC address by burning the ble_arduino.ino sketch. I then updated the MAC address in connections.yaml.
```
Advertising BLE with MAC: c0:81:d5:22:a9:64
```
### Change BLEService UUID
I generated the UUID in jupyter. I then updated the UUID in ble_arduino.ino and connections.yaml.
```
UUID('54d98045-b89c-437c-b219-ad830cef9fff')
```

### Bluetooth Connection
After setting everything up, I was able to establish a bluetooth connection.


### Task 1


### Task 2


### Task 3


### Task 4


### Task 5


### Task 6


### Task 7


### Task 8

## Lab Tasks
Configurations: Show what the relevant configurations, anything that was specifically needed to address the tasks.
Include a brief explanation on what you did and results for each task.
Address all questions posed in the lab.
Include screenshots, screen recordings, pictures, or videos of relevant results (i.e. messages received in Jupyter Notebook, serial terminal print of messages received by Artemis).

## Discussion
Briefly describe what you’ve learned, challenges that you faced, and/or any unique solutions used to fix problems. It is important to keep these writeups succinct. You will not get extra points for writing more words if the content doesn’t contribute to communicating your understanding of the lab material.
