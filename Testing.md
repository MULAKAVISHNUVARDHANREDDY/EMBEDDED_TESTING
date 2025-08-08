# Testing
-------------------
1. What is Embedded Testing?
- Embedded Testing is the process of validating and verifying the functionality, performance, timing, and safety of embedded hardware-software systems. These systems are often part of real-time environments such as:

        - Automotive ECUs
        - Medical devices
        - Consumer electronics
        - IoT and wearable devices

- Embedded systems have strict constraints like timing, power usage, memory limitations, and interface precision, which makes testing a crucial and complex task.

2. Why is Embedded Testing Important?

       - Helps detect bugs early in development
       - Ensures hardware-software integration is reliable
       - Validates real-time behavior and timing requirements
       - Reduces the risk of field failures
# Black Box testing
---------------------
1.What is Black Box Testing?

  - Black Box Testing is a testing technique where the internal structure, design, or implementation of the system is not known to the tester. It validates the system’s behavior solely based on input and output.
  In embedded systems, it is used to ensure that the system works as expected from the user or external interface perspective — without needing access to the source code or internal logic.
  
2.Why is Black Box Testing Important?

    - Does not require source code access
    - Tests the complete system functionality
    - Closely mimics real-world usage
    - Useful for validating final firmware, hardware integration, and system behavior
    - Essential for user acceptance and regulatory testing

    
| Use Case             | Description                                                              |
| -------------------- | ------------------------------------------------------------------------ |
| System-level Testing | Verifies entire embedded system behavior (e.g., Button → LED ON)         |
| Protocol Testing     | Sends and receives data via UART, SPI, I2C and checks for correct output |
| Acceptance Testing   | Ensures the product meets customer and functional requirements           |
| Firmware Validation  | Tests compiled firmware behavior without reviewing the source code       |


### 1️⃣ System-Level Testing

 1.Description:
     - This involves testing the entire embedded system as a whole
       hardware, firmware, sensors, user interface, and communication — from an external point of view.
```c
  Example:
      - In a smart home device, pressing a physical button should activate a motor and turn on an indicator LED.
      - The tester verifies the result by pressing the button and checking the motor response and LED state without
      - knowing the internal logic or how the signal was processed.
```
 2.What is validated:
       - Complete signal flow from input (button) to output (motor/LED)
       - Hardware connections and debounce logic
       - Real-time responsiveness (e.g., output within 100ms.

### 2️⃣ Protocol Testing

  1.Description:
      - Used to verify that communication over serial protocols (UART, SPI, I2C, CAN, etc.)
        works correctly and complies with the expected message structure and timing.
```c  
   Example:
       - Send a command like AT+TEMP? over UART to a microcontroller
       - Check if the response is OK:25.3 or ERROR
       - The tester monitors protocol lines and logs to evaluate behavior
```
   2.What is validated:
        - Correct command parsing
        - Valid response formatting
        - Timing requirements (e.g., response time < 50 ms)
        - CRC/error checking, framing, and retries.

### 3️⃣ Acceptance Testing

   1.Description:
       - Performed by the QA team or client to ensure that the final product meets business,functional and user requirements.
         This is the final validation before release.
 ```c
     Example:
         - In a GPS tracking device, verify that the location is updated every 30 seconds in the mobile app.
         - Tester ensures end-to-end system (hardware → firmware → server → app) works correctly
```
   2.What is validated:
         - All system features function as specified
         - Edge cases and real-world usage (e.g., cold start, lost GPS signal, battery depletion)
         - Interface compatibility (e.g., BLE with different phones)
                
### 4️⃣ Firmware Validation
              
   1. Description:
         - Validates the final compiled and flashed firmware (binary file),without reviewing or modifying the source code.
           This confirms that the device behaves correctly with production firmware.
```c
     Example:
         - Flash a wearable device with final firmware
         - Test user interactions: tapping the screen should open menus, notifications should vibrate the device
```
   2.What is validated:
         - Firmware stability and responsiveness
         - Bootloader functionality
         - Power-on self-tests and startup timing
         - Response to interrupts, button events, sensors.
# White Box testing
-------------------
- White Box Testing focuses on validating the internal logic, decision-making, and edge cases of your embedded code. 
It is done with access to the source code, typically using unit testing frameworks, debugging tools, and code coverage tools.

### 1️⃣ Unit Testing

1.Description:

   - Tests individual functions or modules in isolation to ensure they work as expected.
     This is the most granular level of testing.

```c
  Example:
      - You have a function that converts ADC sensor values to temperature:
        float adc_to_temp(uint16_t adc_value)
         {
            return (adc_value * 0.1f);
         }
```
  - Test case might check:
          - Does adc_to_temp(1000) return 100.0?
          - What happens when adc_value = 0?
    
2.What is validated:

   - Return values under normal and boundary inputs.
   - Proper use of arithmetic/logical expressions.
   - No side effects or unexpected changes to global state.

### 2️⃣ Code Path Testing
   1.Description:
       - Verifies that all possible paths, branches, and conditional logic are executed
         at least once during testing.
```c                
 Example:
        int get_motor_state(bool enabled, int rpm)
        {
            if (!enabled) return 0;
            if (rpm > 1000) return 2;
            return 1;
        }
```
You need test cases that cover:
 - enabled = false → returns 0
 - enabled = true, rpm = 1500 → returns 2
 - enabled = true, rpm = 500 → returns 1

2.What is validated:

 - All if/else conditions are exercised.
 - Decision coverage (True/False paths for each boolean).
 - Code does not crash or behave unpredictably in any path.

### 3️⃣ Boundary Testing
1.Description:

   - Focuses on testing edge-case input values that might cause buffer overflows, underflows, or rounding errors.
```c
Example:
For an array with size 10:
int buffer[10];
```

Test:
- Writing at index 0 ✅
- Writing at index 9 ✅
- Writing at index 10 ❌ (should fail or be protected)
2.What is validated:
     - Proper bounds checking.
     - Array indexing, pointer arithmetic.
     - Input range enforcement (min, max, overflow).
  
### 4️⃣ Driver Testing

1.Description:

   - Tests low-level hardware drivers like GPIO, I2C, SPI, ADC, and PWM by inspecting code behavior directly and simulating hardware events via mocks.
```c
Example:
Testing I2C temperature sensor driver:
```
- Mock sensor to return 0x7FFF.
- Ensure function interprets this as +127.9°C.
- Simulate NACK from device and verify retry logic.
  
2.What is validated:
   - Register reads/writes, timing delays
   - Handling of hardware faults (e.g., NACK, busy)
   - Proper interrupt or DMA behavior if applicable
# Levels in Testing
-------------------
**L0 – Developer-Level / Component Testing**
-------------------------------------------
1.Scope:
        - Done at the lowest level, by developers, during coding.
          Focused on verifying the correctness of individual functions or modules in isolation.

        Includes:
        - Unit Testing (logic, boundaries, error handling)
        - Static Code Analysis (MISRA rules, memory safety)
        - Code Coverage Measurement (statement, branch, MC/DC)
```c
Example:
Testing read_sensor() function to ensure it returns correct values for various ADC inputs.
```
2.Goal:
        - Catch and fix bugs before integration.

**L1 – Module/Integration-Level Testing**
----------------------------------------
1.Scope:
        - Combines two or more software modules or hardware drivers and verifies their interaction.
          Often run on development boards or hardware simulators.
        
        Includes:
        - Integration Testing (driver + algorithm + application logic)
        - Hardware Abstraction Layer (HAL) Testing
        - Interface Protocol Testing (UART, SPI, I2C)
```c
       Example:
        - Test sensor driver + I2C communication + filtering algorithm to confirm correct end-to-end reading.
```
Goal:
Ensure components communicate and exchange data correctly.

L2 – System-Level Testing
Scope:

Validates the entire embedded system (hardware + firmware) under real or simulated operational conditions.

Black Box approach is often used here.

Includes:

System Testing (full device functionality)

Regression Testing (re-run after changes)

Performance Testing (speed, memory, power usage)

Example:
Test a wearable device’s complete workflow: user taps → sensor reads → display updates → data sent over BLE.

Goal:
Ensure the integrated system meets specifications and functions correctly in various scenarios.

L3 – Validation / Acceptance Testing
Scope:

Final testing before release to customer or production.

Ensures compliance with user requirements and regulatory standards.

Includes:

User Acceptance Testing (UAT)

Compliance Testing (ISO, IEC, DO-178C, etc.)

Field/Production Testing (factory line verification)

Example:
In an automotive ECU: test acceleration sensor feedback under actual driving conditions, following ISO 26262 standards.

Goal:
Prove product readiness for deployment.

📊 Summary Table
Level	Name	Scope / Purpose	Example Test
L0	Developer / Component Testing	Unit, static analysis, code coverage	Test motor_start() function
L1	Module / Integration Testing	Verify module interactions, HAL APIs	Sensor driver + protocol check
L2	System-Level Testing	Full system validation in real conditions	Smart meter end-to-end test
L3	Validation / Acceptance Testing	Regulatory and user requirement compliance	Client UAT for wearable tracker
