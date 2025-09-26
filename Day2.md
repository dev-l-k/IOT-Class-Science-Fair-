# Connecting to Arduino and Defining Components in Python with pyfirmata

Once you have uploaded **StandardFirmata** to your Arduino and installed the necessary Python libraries, you're ready to connect your board and define the components (like LEDs, sensors, servos, etc.) you want to control.

---

## 1. Connecting to the Arduino Board

To make your Python program talk to the Arduino, you must create a connection using the `Arduino()` class from pyfirmata. You need to specify the port your Arduino is connected to.

**Example:**

```python
import pyfirmata

# Replace '/dev/ttyUSB1' with your port (e.g., 'COM3' on Windows)
board = pyfirmata.Arduino('/dev/ttyUSB1')
```

- On Windows, the port is usually like `'COM3'` or `'COM4'`.
- On Linux, it’s usually like `'/dev/ttyUSB0'` or `'/dev/ttyACM0'`.
- On Mac, it may look like `'/dev/tty.usbmodemXXXX'`.

**Tip:**  
If you are unsure about your port, unplug your Arduino, check the available ports, then plug it back in and see which new port appears.

---

## 2. Defining Pins for Various Components

After connecting to the board, you need to tell pyfirmata which pins your components are attached to. This is done using the `get_pin()` function.

**Syntax:**  
```python
pin = board.get_pin('type:pin:mode')
```
- **type:** `'d'` for digital, `'a'` for analog
- **pin:** pin number on your Arduino
- **mode:** `'o'` for output (e.g., LEDs), `'i'` for input (e.g., buttons, sensors), `'p'` for PWM (e.g., dimmable LEDs, motors), `'s'` for servo

### Common Component Pin Definitions

| Component    | Pin String Example      | Description                               |
|--------------|------------------------|-------------------------------------------|
| LED (on/off) | `'d:2:o'`              | Digital output pin 2                      |
| Button       | `'d:4:i'`              | Digital input pin 4                       |
| PWM LED      | `'d:3:p'`              | PWM output pin 3 (for brightness control) |
| Servo Motor  | `'d:9:s'`              | Servo on digital pin 9                    |
| Analog Sensor| `'a:0:i'`              | Analog input pin A0                       |
| IR Sensor    | `'d:5:i'`              | Digital input pin 5                       |
| LDR (Light)  | `'a:1:i'`              | Analog input pin A1                       |

**Example Code:**

```python
# LEDs
light = board.get_pin('d:2:o')    # LED on pin 2 (digital output)
led2 = board.get_pin('d:3:p')     # PWM LED on pin 3 (PWM output)

# Sensors
button = board.get_pin('d:4:i')   # Button on pin 4 (digital input)
ir = board.get_pin('d:5:i')       # IR sensor on pin 5 (digital input)
ldr = board.get_pin('a:1:i')      # LDR on analog pin A1 (analog input)

# Servo
servo = board.get_pin('d:9:s')    # Servo on pin 9 (servo mode)
```

---

## 3. Using the Defined Components

You can control or read from the pins as follows:

```python
light.write(1)        # Turn ON LED
led2.write(0.5)       # Set PWM LED to 50% brightness
servo.write(90)       # Rotate servo to 90 degrees

button_state = button.read()  # Read button (1 = pressed, 0 = not pressed)
ir_state = ir.read()          # Read IR sensor
ldr_value = ldr.read()        # Read LDR (value between 0 and 1)
```

**Full Example:**

```python
import pyfirmata
import time

board = pyfirmata.Arduino('/dev/ttyUSB1')

light = board.get_pin('d:2:o')    # LED
led2 = board.get_pin('d:3:p')     # PWM LED
button = board.get_pin('d:4:i')   # Button
servo = board.get_pin('d:9:s')    # Servo
ldr = board.get_pin('a:0:i')      # LDR

light.write(1)
led2.write(0.7)
servo.write(180)

time.sleep(1)

light.write(0)
led2.write(0)
servo.write(0)

# Reading inputs
button_state = button.read()
ldr_value = ldr.read()
print("Button:", button_state, "LDR:", ldr_value)
```

---

## 4. More About Pin Modes

- Use `'d:pin:o'` for outputs (LEDs, buzzers, relays).
- Use `'d:pin:i'` for digital inputs (buttons, switches, digital sensors).
- Use `'a:pin:i'` for analog inputs (sensors like LDR, potentiometers).
- Use `'d:pin:p'` for PWM outputs (dimmable LEDs, motor speed control).
- Use `'d:pin:s'` for servos (servo motors).

---

## 5. Tips

- Always check your wiring and pin numbers.
- Make sure StandardFirmata is still uploaded to your Arduino.
- Close your Python program cleanly to avoid port errors.

---

**Now you know how to connect to your Arduino and define all kinds of components in Python using pyfirmata!**
