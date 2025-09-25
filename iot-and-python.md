# IoT and Python

Python is one of the most popular programming languages for IoT projects because:

- It is easy to learn and read.
- Supports many libraries for hardware control (like Raspberry Pi, Arduino, MicroPython).
- Useful for data analysis and visualization.
- Works well on small, low-powered devices.

**Example:**  
Controlling an LED on Raspberry Pi using Python.

```python
import RPi.GPIO as GPIO
import time

GPIO.setmode(GPIO.BCM)
GPIO.setup(18, GPIO.OUT)

GPIO.output(18, True)
time.sleep(1)
GPIO.output(18, False)

GPIO.cleanup()
```