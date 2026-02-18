````markdown
# One-Axis Face Tracking Camera Lab

Welcome to the Face Tracking Workshop!

In this lab you will build a simple vision-controlled servo system using:

- Raspberry Pi Pico
- SG90 Servo Motor
- Laptop Webcam
- Python + OpenCV

By the end of this lab, your servo will rotate to follow your face horizontally.

---

# Lab Structure

This lab has three major parts:

1. Install & Configure Hardware
2. Validate the Pico with LED + Servo Tests
3. Build the Face Detection + Tracking System

Work step-by-step. Do not skip checkpoints.

---

# PART 1 — Install and Configure

## Step 1 — Install Thonny

1. Go to: https://thonny.org
2. Download the Windows installer
3. Install and open Thonny

If installation fails, notify the instructor.

---

## Step 2 — Connect the Pico

1. Plug in the Raspberry Pi Pico via USB.
2. In Thonny, click the bottom-right interpreter selector.
3. Choose:

    MicroPython (Raspberry Pi Pico)

4. If prompted, install MicroPython firmware.

Wait until installation completes.

---

## Checkpoint 1

In the Thonny shell, type:

```python
import machine
```

If there is no error, your Pico is ready.

If you see:

```
ModuleNotFoundError: machine
```

You are not running MicroPython.

---

# PART 2 — Hardware Validation

We test the board before building the full system.

---

## Step 3 — LED Blink Test

Copy and run:

```python
from machine import Pin
import time

led = Pin("LED", Pin.OUT)

while True:
    led.toggle()
    time.sleep(0.5)
```

The onboard LED should blink.

If it does not:

* Check interpreter selection
* Reinstall MicroPython
* Ask instructor

---

## Step 4 — Servo Wiring

Connect your SG90 servo:

* Brown → GND
* Red → VSYS (5V)
* Orange → GP15

---

## Step 5 — Servo Test

Replace code with:

```python
from machine import Pin, PWM
import time

servo = PWM(Pin(15))
servo.freq(50)

servo.duty_u16(5000)
```

The servo should move to center.

---

## Checkpoint 2

If servo moves, hardware setup is complete.

Do not continue until this works.

---

# PART 3 — Face Detection (Laptop Side)

From this point forward, code runs on your laptop (not the Pico).

---

## Step 6 — Install Required Packages

Open Command Prompt and run:

```
pip install opencv-python pyserial
```

If installation fails, notify the instructor.

---

## Step 7 — Download Skeleton Code

Download the skeleton project from:

👉 [Code Skeleton - Getting Started](https://github.com/asaadsleman/Face-Tracking-with-Raspberry-Pi-Pico)

Download ZIP → Extract → Open folder.

You will edit:

* laptop/face_tracking.py
* pico/main.py

---

# Face Detection — Phase A

Open:

```
laptop/face_tracking.py
```

Run the file.

A green box should appear around your face.

If it does not:

* Increase lighting
* Move closer to camera
* Check webcam permissions

---

## Checkpoint 3

Face detection must work before continuing.

---

# Face Tracking — Phase B

Your goal now:

1. Find the center X position of the detected face.
2. Map that value from:

   * 0–640 pixels
   * to 0–180 degrees
3. Send the angle to the Pico via serial.
4. Move the servo accordingly.

Follow the TODO comments inside the file.

---

## Mapping Formula Reminder

```python
def map_value(x, in_min, in_max, out_min, out_max):
    return (x - in_min) * (out_max - out_min) / (in_max - in_min) + out_min
```

---

## Final Result

When you move your face left and right:

* The servo should rotate to follow you.
* Center position ≈ 90°
* Left ≈ 0°
* Right ≈ 180°

---

# Troubleshooting

## Error: No module named 'machine'

You are running Pico code on your laptop.
Switch Thonny interpreter to MicroPython.

---

## Serial Port Busy

Close Thonny before running the laptop script.
Only one program can use the serial port at a time.

---

## Servo Jitter

* Check ground connection
* Improve lighting
* Add smoothing in code

---

# Reflection Questions

1. Why does the Pico use MicroPython instead of regular Python?
2. Why do we map pixel position to angle?
3. Why might the servo shake?
4. How could we make the motion smoother?

---

# Extensions (Optional)

* Add smoothing factor
* Add dead-zone near center
* Reverse servo direction
* Add second servo (pan + tilt)

---

# End of Lab

If everything works:

Congratulations — you have built a simple real-time computer vision control system.

```
