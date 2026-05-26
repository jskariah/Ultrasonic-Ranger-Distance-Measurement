# TM4C Ultrasonic Distance Meter

## Overview

This project is an embedded distance measurement system built for the TM4C/Tiva C LaunchPad. It uses a Grove ultrasonic ranger sensor to measure distance by sending a trigger pulse and capturing the returned echo pulse using a hardware timer interrupt.

## Features

- Ultrasonic ranger distance measurement
- Event-driven embedded system structure
- Periodic ranger triggering every 0.5 seconds
- Wide Timer input capture
- Timer interrupt service routine
- Finite state machine for ranger pulse detection
- Designed to support distance display in millimeters or inches
- Built for the TM4C123GH6PM microcontroller using Code Composer Studio

## Hardware Used

- TM4C/Tiva C LaunchPad
- Grove Base BoosterPack
- Grove ultrasonic ranger sensor
- 4-digit 7-segment display

## Hardware Connections

| Component | Connection |
|---|---|
| Ultrasonic Ranger | Grove J6 |
| Ranger Signal Pin | GPIO PD1 |
| Timer Capture Pin | WT2CCP1 |
| Microcontroller | TM4C123GH6PM |

## Project Structure

```text
TM4C-Ultrasonic-Distance-Meter/
├── lab8_main.c
├── ranger.c
├── ranger.h
├── seg7.h
├── tm4c123gh6pm_startup_ccs.c
├── tm4c123gh6pm.cmd
└── README.md
```

## File Descriptions

| File | Description |
|---|---|
| `lab8_main.c` | Main program file that initializes the system and periodically triggers ranger readings |
| `ranger.c` | Ultrasonic ranger driver, timer configuration, trigger pulse generation, and ISR state machine |
| `ranger.h` | Header file containing the ranger interface |
| `seg7.h` | Header file for the 4-digit 7-segment display interface |
| `tm4c123gh6pm_startup_ccs.c` | Startup code for the TM4C123GH6PM microcontroller |
| `tm4c123gh6pm.cmd` | Linker command file for Code Composer Studio |

## How It Works

The ultrasonic ranger measures distance using pulse timing.

The basic process is:

1. The microcontroller sends a short trigger pulse to the ranger.
2. The ranger sends back an echo pulse.
3. The width of the echo pulse represents the distance to the object.
4. A wide timer captures the pulse edges.
5. The interrupt service routine tracks the pulse using a finite state machine.
6. The measured pulse width can be converted into distance.

## Ranger State Machine

The ranger interrupt service routine uses a software finite state machine to process pulse events.

The states are:

| State | Purpose |
|---|---|
| `IDLE` | Ranger is not currently measuring |
| `EXPECT_START_EVENT_1` | Waiting for the rising edge of the start pulse |
| `EXPECT_START_EVENT_2` | Waiting for the falling edge of the start pulse |
| `EXPECT_DATA_EVENT_1` | Waiting for the rising edge of the echo/data pulse |
| `EXPECT_DATA_EVENT_2` | Waiting for the falling edge of the echo/data pulse |

This structure allows the program to separate the outgoing trigger pulse from the incoming echo pulse.

## Event-Driven Design

The program uses an event-based loop instead of continuously polling everything manually.

In `lab8_main.c`, the system:

1. Initializes the LaunchPad
2. Initializes the ultrasonic ranger
3. Creates a scheduled event for ranger readings
4. Triggers a ranger measurement every 0.5 seconds
5. Waits for interrupts using `wfi`
6. Executes scheduled callbacks through the event system

## Timer Configuration

The ranger driver uses Wide Timer 2B with GPIO PD1 configured as the timer capture pin. The timer is used to generate the ranger trigger pulse and process capture events from the returned pulse.

## Display Support

The project includes a 7-segment display interface and a system state for switching between millimeter and inch display modes.

The uploaded code is structured for distance display support, but the current version mainly shows the ranger triggering and timer-capture framework.

## How to Build and Run

This project is intended to be built and flashed using Texas Instruments Code Composer Studio.

General steps:

1. Open Code Composer Studio.
2. Import the project into the workspace.
3. Connect the TM4C/Tiva C LaunchPad.
4. Build the project.
5. Flash the program to the board.
6. Connect the ultrasonic ranger sensor.
7. Run the program and observe ranger trigger behavior.

## What I Learned

Through this lab, I practiced:

- Embedded C programming
- Ultrasonic sensor interfacing
- Timer configuration on the TM4C microcontroller
- Wide timer input capture
- Interrupt-driven programming
- Event scheduling
- Finite state machine design
- Hardware/software integration
- Working with Code Composer Studio
- Understanding how pulse width can be used to calculate distance

## Future Improvements

Potential future improvements include:

- Completing distance calculation from captured pulse width
- Displaying live distance readings on the 7-segment display
- Adding a button to switch between millimeters and inches
- Adding filtering or averaging to stabilize measurements
- Adding range limits and error handling
- Adding a wiring diagram
- Adding photos or demo video of the working setup

## Author

Created by Justin Skariah.

Developed for ECE 266
