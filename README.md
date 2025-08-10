## Chick-Embryo-Heart-Code

Short Arduino sketch that uses an Adafruit VL6180X Time‑of‑Flight (ToF) and ambient light sensor to:

- Read ambient light level (lux)
- Measure distance (range) in millimeters
- Stream readings and sensor status messages over Serial at 115200 baud
- Drive a relay/actuator on pin `D10` using PWM based on the measured distance

### What the code does
- Initializes the VL6180X over I2C and starts the Serial port.
- Continuously reads:
  - Ambient light in lux: `vl.readLux(VL6180X_ALS_GAIN_5)`
  - Range (distance) in mm: `vl.readRange()`
- Prints lux, range (when valid), and any sensor error status to the Serial Monitor.
- Controls `RELAY_PIN` (pin `10`) using PWM according to the range threshold:
  - If range < 42.9 mm → `analogWrite(10, 0)` (off)
  - If range > 42.9 mm → `analogWrite(10, 25)` (~10% duty cycle)

Note: The pin is named `RELAY_PIN`, but the code uses PWM (`analogWrite`). Use a suitable driver (transistor/MOSFET/SSR) if you are switching a load. Mechanical relays generally should not be PWM‑driven.

### Hardware
- Arduino‑compatible board
- Adafruit VL6180X ToF & ALS sensor breakout (I2C)
- Relay/SSR/transistor driver connected to Arduino pin `D10` (as required by your load)

### Wiring (typical)
- VL6180X → Arduino
  - VIN → 5V (Adafruit breakout tolerates 5V)
  - GND → GND
  - SCL → SCL
  - SDA → SDA
- Driver/Relay input → `D10` (RELAY_PIN)
- Provide separate power/ground to your load as required

### Dependencies
- Arduino IDE
- Libraries (install via Library Manager):
  - `Adafruit VL6180X`
  - `Wire` (bundled with Arduino)

### Usage
1. Open `vl6180x.ino` in the Arduino IDE.
2. Install the required library (`Adafruit VL6180X`).
3. Select your board and port, then upload.
4. Open the Serial Monitor at 115200 baud to view lux, range, and status messages.

### Configuration
- Change the relay/driver output pin by editing `RELAY_PIN` in `vl6180x.ino`.
- Adjust the distance threshold (`42.9`) and PWM values (`0` and `25`) inside the `loop()` to suit your application.

This sketch was created for a masters project (ex vivo culture of an embryonic chick heart), but the code itself is general‑purpose for VL6180X distance/light sensing with simple threshold‑based actuator control.
