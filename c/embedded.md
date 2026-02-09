# 📘 CURSOR PROMPTS – Lập trình nhúng (Embedded C/C++)

**Rules • Prompts • Debug cho Embedded Systems trong Cursor IDE**

> File này thuộc thư mục [c/](./README.md).

---

## Mục lục

- [1. Rules cho Embedded Projects](#1-rules-cho-embedded-projects)
- [2. Peripheral Driver Prompts](#2-peripheral-driver-prompts)
- [3. RTOS Prompts](#3-rtos-prompts)
- [4. Communication Protocol Prompts](#4-communication-protocol-prompts)
- [5. Sensor & Device Driver](#5-sensor--device-driver)
- [6. Build & Toolchain](#6-build--toolchain)
- [7. Debug Prompts](#7-debug-prompts)
- [8. Best Practices & Anti-patterns](#8-best-practices--anti-patterns)

---

## 1. Rules cho Embedded Projects

### 1.1 File `.cursor/rules/embedded.mdc`

```
---
description: Rules for embedded C/C++ files
globs: *.c, *.h, *.cpp, *.hpp
---

- NO dynamic memory allocation (no malloc/free) in bare-metal code
- Use volatile for hardware registers and ISR-shared variables
- Keep ISR handlers short (set flag, defer processing to main loop/task)
- Use fixed-width types: uint8_t, uint16_t, uint32_t (stdint.h)
- Use const for lookup tables and configuration (store in Flash)
- Initialize all variables explicitly
- Check all hardware return/status values
- Use #pragma once or include guards
- Document register addresses and bit fields
- Consider power consumption in design choices
- No floating-point unless hardware FPU available
- Use static for file-scope functions and variables
- Follow MISRA-C guidelines where applicable
- Target: [STM32 / AVR / ESP32 / PIC / NRF / RP2040]
```

### 1.2 Full `.cursorrules` cho Embedded project

```txt
You are an expert embedded systems engineer with deep knowledge of
microcontrollers, real-time systems, and low-level hardware programming.

# Embedded Rules
- Target MCU: [STM32 / AVR / ESP32 / RP2040 / NRF52 / PIC]
- NO dynamic memory allocation (bare-metal).
- Use stdint.h types (uint8_t, uint32_t, etc.).
- Use volatile for hardware registers and ISR-shared data.
- Keep ISRs short – set flag, process in main loop or task.
- Const for flash-stored data (lookup tables, strings).
- No floating-point unless FPU available.
- Power-aware code (sleep modes, peripheral clock gating).
- Follow MISRA-C guidelines where applicable.
- Document all register addresses and bit fields.
- Test on hardware or accurate simulator.
```

---

## 2. Peripheral Driver Prompts

### 2.1 GPIO Driver

```text
@codebase

Create GPIO driver module:

Requirements:
- Init function (pin, mode: input/output/alternate/analog, pull-up/down)
- Read/Write functions
- Toggle function
- Interrupt configuration (rising/falling/both edge)
- Callback registration for GPIO interrupt
- Pin abstraction (port + pin → single identifier)
- No dynamic allocation

Target: [STM32 HAL / bare-metal register / ESP-IDF / Arduino]
```

### 2.2 UART Driver

```text
@codebase

Create UART driver module:

Requirements:
- Init (baudrate, data bits, parity, stop bits)
- Blocking send/receive
- Non-blocking (interrupt-based) send/receive with ring buffer
- DMA transfer support (if available)
- Timeout handling
- Error detection (overrun, framing, parity errors)
- Printf-like formatted output function
- Configurable buffer sizes (compile-time #define)

Target: [MCU]
```

### 2.3 SPI Driver

```text
@codebase

Create SPI driver module:

Requirements:
- Master mode init (clock speed, CPOL, CPHA, bit order)
- CS pin management (auto or manual)
- Transfer function (send + receive simultaneously)
- Multi-byte transfer (buffer + length)
- DMA support (if available)
- Lock mechanism (if shared bus)
- Error handling

Target: [MCU]
```

### 2.4 I2C Driver

```text
@codebase

Create I2C driver module:

Requirements:
- Master mode init (clock speed: 100KHz/400KHz/1MHz)
- Write: address + register + data
- Read: address + register → data
- Multi-byte read/write
- ACK/NACK handling
- Timeout
- Bus recovery (clock stretching, stuck bus)
- Scanner function (detect all devices on bus)

Target: [MCU]
```

### 2.5 Timer / PWM Driver

```text
@codebase

Create Timer/PWM driver module:

Requirements:
- Timer init (prescaler, period, mode: normal/PWM/capture)
- PWM output (frequency, duty cycle)
- Input capture (frequency/period measurement)
- One-shot and periodic timer modes
- Callback on timer interrupt
- Microsecond delay function
- Multiple timer channel support

Target: [MCU]
```

### 2.6 ADC Driver

```text
@codebase

Create ADC driver module:

Requirements:
- Init (resolution, reference voltage, sampling time)
- Single conversion read
- Continuous/scan mode
- DMA for continuous conversion
- Multi-channel support
- Averaging / oversampling
- Conversion to voltage/engineering units
- Calibration support

Target: [MCU]
```

---

## 3. RTOS Prompts

### 3.1 FreeRTOS Task

```text
@codebase

Create FreeRTOS task for [mô tả chức năng]:

Requirements:
- Task function with proper infinite loop
- Stack size estimation (worst-case analysis)
- Priority assignment (document rationale)
- Inter-task communication: [queue / semaphore / mutex / event group / notification]
- Watchdog feeding
- Error/exception handling (vApplicationStackOverflowHook)
- Task statistics (runtime, stack high water mark)
- Graceful shutdown/restart capability
```

### 3.2 FreeRTOS Queue Communication

```text
@codebase

Create inter-task communication using FreeRTOS queues:

Requirements:
- Queue creation (item size, queue length)
- Producer task(s) – xQueueSend
- Consumer task(s) – xQueueReceive
- Timeout handling
- Queue full/empty handling
- Typed messages (struct with message type + payload)
- ISR-safe variants (xQueueSendFromISR) if needed
```

### 3.3 FreeRTOS Mutex / Semaphore

```text
@codebase

Create resource protection using FreeRTOS synchronization:

Requirements:
- Mutex for shared resource access (SPI bus, UART, shared buffer)
- Binary semaphore for event signaling (ISR → task)
- Counting semaphore for resource pool management
- Priority inheritance (mutex)
- Timeout to prevent deadlocks
- Document locking order to prevent deadlocks
```

### 3.4 Zephyr RTOS Task

```text
@codebase

Create Zephyr RTOS thread for [mô tả]:

Requirements:
- K_THREAD_DEFINE or k_thread_create
- Proper stack size (K_THREAD_STACK_DEFINE)
- Priority and scheduling
- Kernel objects: k_sem, k_mutex, k_msgq, k_fifo
- Devicetree integration for hardware access
- Power management hooks
- Logging with LOG_MODULE_REGISTER
```

---

## 4. Communication Protocol Prompts

### 4.1 Modbus RTU

```text
@codebase

Implement Modbus RTU [master / slave]:

Requirements:
- UART-based communication
- Function codes: 0x03 (Read Holding), 0x06 (Write Single), 0x10 (Write Multiple)
- CRC-16 calculation and verification
- Timeout handling (inter-character, response)
- Register map definition
- Error response handling
- Multi-device support (addressing)
```

### 4.2 CAN Bus

```text
@codebase

Create CAN bus communication module:

Requirements:
- CAN init (bitrate, filter configuration)
- Send/receive CAN frames
- Standard (11-bit) and Extended (29-bit) IDs
- Filter/mask configuration
- Interrupt-based reception
- TX mailbox management
- Error handling (bus-off, error passive)
- Higher-layer protocol support: [CANopen / J1939 / custom]
```

### 4.3 MQTT Client (for IoT)

```text
@codebase

Create lightweight MQTT client for [MCU]:

Requirements:
- Use [Eclipse Paho Embedded / Mongoose / lwMQTT / custom]
- Connect/disconnect with TLS (if supported)
- Publish messages with QoS 0/1/2
- Subscribe to topics with callback
- Last Will and Testament
- Keep-alive and reconnection
- Message queue for offline buffering
- JSON payload serialization
- Low memory footprint
```

### 4.4 Custom Serial Protocol

```text
@codebase

Create custom serial protocol for [mô tả: device communication, bootloader, data logging]:

Requirements:
- Frame format: [start byte | length | command | payload | CRC | end byte]
- CRC calculation (CRC-8/16/32)
- Frame parsing state machine
- Command handler registry
- ACK/NACK mechanism
- Timeout and retry logic
- Escape characters for binary data
- Error detection and recovery
```

---

## 5. Sensor & Device Driver

### 5.1 Temperature Sensor

```text
@codebase

Create driver for [sensor: DS18B20 / BME280 / LM75 / SHT30 / DHT22]:

Requirements:
- Communication: [I2C / SPI / 1-Wire / analog]
- Init with error detection (device present?)
- Read temperature (and humidity/pressure if applicable)
- Raw → engineering unit conversion
- Calibration offset support
- Configurable sample rate / resolution
- Error handling (communication failure, out-of-range)
- Example usage
```

### 5.2 IMU / Accelerometer

```text
@codebase

Create driver for [IMU: MPU6050 / BMI160 / LSM6DS3 / ICM-20948]:

Requirements:
- I2C/SPI communication
- Accelerometer + gyroscope reading
- Configurable range and sample rate
- Digital filter configuration
- FIFO reading (if available)
- Interrupt configuration (data ready, motion detection)
- Sensor fusion hint (complementary filter / Madgwick / Kalman)
- Raw → g / °/s conversion
```

### 5.3 Display Driver (LCD / OLED)

```text
@codebase

Create display driver for [display: SSD1306 OLED / ILI9341 TFT / HD44780 LCD / ST7789]:

Requirements:
- Communication: [I2C / SPI / parallel]
- Init sequence
- Pixel/character write
- Text rendering (built-in font)
- Drawing primitives (line, rect, circle, fill)
- Buffer management (framebuffer for OLED)
- Refresh/flush function
- Contrast/brightness control
- Power save mode
```

### 5.4 Motor Control

```text
@codebase

Create motor control module for [motor: DC / Stepper / Servo / BLDC]:

Requirements:
- PWM-based speed control
- Direction control (H-bridge / enable pins)
- Encoder feedback (if closed-loop)
- PID controller for speed/position
- Acceleration/deceleration ramp
- Current limiting / protection
- Emergency stop
- Position tracking (for stepper)
```

---

## 6. Build & Toolchain

### 6.1 CMake for Embedded

```text
@codebase

Create CMakeLists.txt for embedded project:

Requirements:
- Cross-compiler toolchain file (arm-none-eabi-gcc)
- Linker script specification
- Startup file inclusion
- Compiler flags: -mcpu, -mthumb, -mfloat-abi, -mfpu
- Binary outputs: .elf, .bin, .hex
- Size report (text/data/bss)
- Flash/upload target
- Debug configuration (OpenOCD / J-Link / ST-Link)

Target: [MCU family]
```

### 6.2 PlatformIO setup

```text
@codebase

Create PlatformIO project configuration:

Requirements:
- platformio.ini with board, framework, platform
- Build flags and defines
- Library dependencies
- Upload and debug settings
- Multiple environments (debug, release, test)
- Custom scripts if needed
```

---

## 7. Debug Prompts

### 7.1 Hard Fault / Crash

```text
@terminal
@file [filepath]

MCU hard fault / crash:
[paste: fault handler output, register dump, call stack]

Please:
- Decode fault registers (CFSR, HFSR, MMFAR, BFAR)
- Identify: null pointer, stack overflow, alignment fault, bus error
- Fix the root cause
- Add fault handler with debug output
```

### 7.2 Peripheral Not Working

```text
@codebase
@file [filepath]

Peripheral [UART / SPI / I2C / ADC / Timer] not working:
[mô tả: no output, wrong data, timeout]

Please:
- Check clock enable (RCC)
- Check pin alternate function and GPIO configuration
- Check peripheral register settings
- Check interrupt enable and priority
- Verify with datasheet / reference manual
- Suggest logic analyzer / oscilloscope checks
```

### 7.3 RTOS Bug

```text
@codebase

RTOS issue:
[mô tả: deadlock, stack overflow, priority inversion, missed deadline, memory corruption]

Please:
- Check task priorities and stack sizes
- Check mutex/semaphore usage (deadlock analysis)
- Check ISR-task interaction (ISR-safe APIs)
- Check shared resource protection
- Add runtime statistics monitoring
```

### 7.4 Timing Issue

```text
@codebase

Timing issue:
[mô tả: jitter, missed deadline, wrong frequency, drift]

Please:
- Check timer configuration (prescaler, period)
- Check interrupt latency
- Check clock source and PLL configuration
- Use oscilloscope/logic analyzer for verification
- Consider DMA for timing-critical transfers
```

---

## 8. Best Practices & Anti-patterns

### ✅ DO
- `volatile` cho hardware registers và ISR-shared variables
- `stdint.h` types (`uint8_t`, `uint32_t`) – không dùng `int`
- `const` cho lookup tables (lưu Flash, tiết kiệm RAM)
- `static` cho file-scope functions/variables
- Keep ISRs short (set flag, process later)
- Document register addresses và bit fields
- Use watchdog timer
- Test trên hardware thật (không chỉ simulator)

### ❌ DON'T
- Không `malloc`/`free` trong bare-metal (fragmentation)
- Không floating-point khi không có FPU
- Không busy-wait trong RTOS task (dùng delay/semaphore)
- Không quên enable clock cho peripheral
- Không quên volatile cho shared variables
- Không ignore interrupt priorities (nested interrupt issues)
- Không hardcode magic register values – dùng defines/macros

---

> 📌 Quay lại [C Core](./README.md) | [Handbook chính](../cursor-ide-handbook.md)
