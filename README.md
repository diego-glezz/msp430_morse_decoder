# Morse Code Decoder (MSP430FR6989)

This repository contains the C source code for an embedded system capable of decoding physical Morse code inputs in real-time. Developed specifically for the Texas Instruments MSP430FR6989 LaunchPad evaluation board.

The project demonstrates direct low-level peripheral control, static memory management in constrained environments, and asynchronous event handling via hardware interrupts.

## Hardware Requirements
* Texas Instruments MSP430FR6989 LaunchPad board.
* USB connection for power and serial communication (Application UART).

## Key Technical Features

* **Hardware Interrupts (ISR):** Replacement of blocking loops (polling) with interrupt service routines on GPIO pins and data transmission, freeing the CPU for concurrent operations.
* **Timer Control:** Use of the Timer_A module and the VLO oscillator (10 kHz) to accurately measure pulse duration (differentiating between dots and dashes) and inactivity times (letter and word spacing).
* **Hardware Debouncing:** Implementation of controlled delays in physical button reads to mitigate electrical noise and prevent multiple registrations or accidental deletions.
* **Asynchronous UART Communication:** Configuration of the eUSCI_A1 module at 9600 baud to transmit the complete Morse dictionary and error messages to the PC via interrupts (`UCTXIE`), operating in the background.
* **Safe Memory Management:** Strict use of fixed-size arrays and static memory to avoid Heap fragmentation and prevent critical failures (Hard Faults) common in dynamic memory allocation within embedded systems.
* **Visual Interface:** Direct control of the integrated LCD peripheral registers to display the decoded text.

## Usage Instructions

1. Compile and flash the code to the MSP430FR6989 board using Code Composer Studio (CCS) or another compatible IDE.
2. Open a serial terminal on the computer (e.g., PuTTY or Tera Term) connected to the "Application UART" COM port at **9600 baud, 8N1**.
3. Upon startup, the board will automatically transmit the reference Morse dictionary to the console.
4. **Button 1 (P1.1):** * Short press (< 0.4s): Registers a dot (`.`).
   * Long press (> 0.4s): Registers a dash (`-`).
5. **Button 2 (P1.2):** Deletes the last decoded character.
6. The board will evaluate the input after 0.6 seconds of inactivity. If the Morse code matches a letter or number, it will appear on the LCD panel. If the sequence is invalid, the red LED (P1.0) will turn on, indicating a non-blocking error.
