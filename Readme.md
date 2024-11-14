
---

# STM32 LED Contention Demo Using FreeRTOS

This project demonstrates a contention scenario between two tasks in an STM32 microcontroller environment using FreeRTOS. The tasks simulate accessing a shared resource and illustrate contention by managing three LEDs (Green, Red, and Blue) on the STM32 board.

## Overview

This program has two primary tasks, each attempting to access a shared resource:
- **Green LED Task**: Simulates a low-priority task with access to the shared resource, illuminating the Green LED while it "holds" the resource.
- **Red LED Task**: Simulates a high-priority task that accesses the same resource, illuminating the Red LED while it has access.

Both tasks are designed to occasionally attempt access to the shared resource at the same time, creating a **contention condition**. When contention occurs, the Blue LED illuminates, indicating that both tasks attempted to use the shared resource simultaneously.

## Project Structure

- **`main.c`**: The main program file that contains the task implementations and shared resource access control.
- **FreeRTOS configuration**: Manages the task priorities and delays to allow task scheduling and handling contention.

## System Description

This project uses:
- **Green LED Task**: Low-priority task with a 500 ms delay between attempts to access the shared resource.
- **Red LED Task**: High-priority task with a 100 ms delay between access attempts.
- **Blue LED**: Indicates a contention problem, which occurs when both tasks attempt to access the shared resource simultaneously.

### Simulating Access to the Shared Resource

The shared resource is controlled by a simple access indicator:
- **StartFlag**: A binary flag representing the state of the shared resource. When `StartFlag` is `Up`, the resource is available. When `StartFlag` is `Down`, the resource is in use by one of the tasks.

The access pattern is as follows:
1. A task checks if `StartFlag` is `Up`.
2. If `StartFlag` is `Up`, it sets it to `Down` and accesses the resource.
3. If `StartFlag` is already `Down`, the task identifies contention by turning on the Blue LED.
4. After simulating access (a 500 ms delay), the task sets `StartFlag` back to `Up` and releases the resource.

## Code Structure

### Task Implementations

- **GreenLed Task**:
  - Turn on Green LED.
  - Access the shared data.
  - Turn off Green LED.
  - Delay for 500 ms.

- **RedLed Task**:
  - Turn on Red LED.
  - Access the shared data.
  - Turn off Red LED.
  - Delay for 100 ms.

The Red LED task has a higher priority, so it can preempt the Green LED task, which leads to occasional contention.

### Access Shared Data Function

The `AccessSharedData` function simulates contention:
```c
void AccessSharedData(void) {
    if (StartFlag == 1) {
        StartFlag = 0;  // Set StartFlag to Down
    } else {
        HAL_GPIO_WritePin(GPIOC, GPIO_PIN_13, GPIO_PIN_SET);  // Turn on Blue LED for contention
    }

    HAL_Delay(500);  // Simulate read/write delay

    StartFlag = 1;  // Set StartFlag back to Up
    HAL_GPIO_WritePin(GPIOC, GPIO_PIN_13, GPIO_PIN_RESET);  // Turn off Blue LED if it was turned on
}
```

## Getting Started

1. Clone this repository and load the project into your STM32 development environment.
2. Configure the FreeRTOS and HAL libraries.
3. Flash the code to your STM32 microcontroller and run the program.

When the program runs, the Blue LED should occasionally light up, indicating contention when the Red and Green tasks both try to access the shared resource.

## Requirements

- **STM32 Microcontroller** (configured with LED pins and FreeRTOS).
- **FreeRTOS**: Used for task scheduling.
- **STM32 HAL Library**: For GPIO and timing functions.

## License

This project is licensed under the terms in the LICENSE file provided with the software.

## Acknowledgments

This project was developed as part of an exercise in resource sharing and contention detection on real-time operating systems for embedded systems.

---

## Video Demo Exercise 5
https://github.com/thamyis100/contention-problems/pull/3#issue-2657479652
