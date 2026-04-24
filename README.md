# DC Motor Speed Control using Progressive Control Strategies (STM32 + MATLAB)

## 📌 Overview

This project implements a **real-time DC motor speed control system** using an STM32 microcontroller. The system progressively explores different control strategies, starting from basic methods and advancing toward a full **PID controller**.

The goal is to **understand, implement, and compare** the behavior of:

* Proportional (P) Control
* Integral (I) Control
* Proportional + Integral (PI) Control
* Feedforward Control
* Full PID Control

Additionally, **MATLAB is integrated** with the embedded system via UART to enable **real-time graphical visualization** of system performance.

---

## ⚙️ System Description

The system measures motor speed using **encoder pulses** and computes RPM in real time. A control algorithm then adjusts the PWM duty cycle to regulate the motor speed toward a desired **setpoint**.

Key implementation reference: 

### 🔁 Control Loop Flow

1. Encoder pulses are counted using interrupts
2. RPM is calculated at fixed time intervals
3. Error is computed:

   ```
   error = setpoint - measured_rpm
   ```
4. Control law is applied (depending on strategy)
5. PWM output is updated to drive the motor

---

## 🧠 Control Strategies Implemented

### 1. Proportional Control (P)

* Output is proportional to the error
* Fast response but may result in steady-state error

```
PWM = base_pwm + Kp * error
```

---

### 2. Integral Control (I)

* Eliminates steady-state error by accumulating past error
* Can introduce overshoot if not bounded

```
integral += error * dt
PWM = base_pwm + Ki * integral
```

---

### 3. Proportional + Integral Control (PI)

* Combines fast response and steady-state correction
* Widely used in practical systems

```
PWM = base_pwm + (Kp * error) + (Ki * integral)
```

---

### 4. Feedforward Control

* Provides a **baseline PWM (base_pwm)** based on expected system behavior
* Reduces controller effort and improves response time

```
PWM = base_pwm + control_action
```

---

### 5. Full PID Control

* Adds derivative action to predict system behavior
* Improves stability and reduces overshoot

```
PWM = base_pwm + (Kp * error) + (Ki * integral) + (Kd * derivative)
```

Implemented features include:

* **Derivative smoothing (low-pass filtering)**
* **Integral windup limiting**
* **PWM saturation handling**

---

## 🔧 Key Parameters

| Parameter      | Description         |
| -------------- | ------------------- |
| `Kp`           | Proportional gain   |
| `Ki`           | Integral gain       |
| `Kd`           | Derivative gain     |
| `base_pwm`     | Feedforward term    |
| `dt`           | Sampling time       |
| `setpoint_rpm` | Desired motor speed |

---

## 📡 MATLAB Integration

The STM32 transmits real-time data via **UART (115200 baud)** to MATLAB.

### Data Sent:

* RPM
* Integral value (and can be extended)

Example:

```
The RPM is : 950.25, The Integral is : 120.50
```

### MATLAB Role:

* Reads serial data
* Parses incoming values
* Plots **real-time graphs** of system response

This allows:

* Visual tuning of controller gains
* Observation of transient and steady-state behavior
* Performance comparison across control strategies

---

## 🧪 Features Implemented in Code

* Real-time RPM computation using encoder pulses
* Interrupt-based pulse counting
* PWM generation using TIM3
* Anti-windup for integral term
* Derivative filtering
* UART-based telemetry for visualization
* Configurable control gains

---

## 🚀 How to Run

1. Flash the code to STM32
2. Connect motor + encoder
3. Open MATLAB and start serial communication
4. Run MATLAB script for live plotting
5. Adjust gains (`Kp`, `Ki`, `Kd`) to observe behavior

---

## 📊 Learning Outcomes

This project demonstrates:

* Practical implementation of control theory
* Transition from simple to advanced controllers
* Real-time embedded system design
* Hardware-software co-design using MATLAB
* System tuning and performance analysis

---

## 🔮 Future Improvements

* Add **full MATLAB GUI dashboard**
* Implement **auto-tuning (Ziegler-Nichols / optimization)**
* Introduce **RTOS-based scheduling**
* Extend to **closed-loop position control**
* Add **data logging for offline analysis**

---

## 📁 File Structure

* `main.c` → Core embedded control logic
* MATLAB script → Real-time visualization (external)

---

## 🧑‍💻 Author

Embedded Systems & Control Engineering Project

---

## 📜 License

This project is provided as-is for educational and research purposes.

---
