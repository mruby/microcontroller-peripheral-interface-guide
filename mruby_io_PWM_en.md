# PWM

- A class that supports Pulse Width Modulation (`PWM`) functionality.

---

## **Constructor**

---

### PWM.new( pin, *params )

- Generate a PWM object by specifying the physical pin indicated by "pin."
- Typically, "pin" is specified as an integer, but alternative methods (e.g., "B1" in PIC) are also acceptable.
- When specifying the parameter **`frequency`**, the output starts with a duty cycle of 50%.
- If you want to start the output with a duty cycle other than 50%, specify the parameter **`duty`** simultaneously.

Optional Parameters

| param | type | description |
| --- | --- | --- |
| frequency | Integer,Float | Specifies the frequency. |
| freq | (Same as above) |  |
| duty | Integer,Float | Specifies the duty cycle. |

Example of use:

```ruby
# Generate an object for PWM output on pin 1. Output is not started yet.
pwm1 = PWM.new( 1 )

# Generate an object for PWM output on pin 1 and start the output with a frequency of 440Hz and a duty cycle of 30%.
pwm1 = PWM.new( 1, frequency:440, duty:30 )
```

Device-specific

- Additional parameters such as unit names may be required.

---

## **Instance Methods**

---

### frequency( freq )

- Starts the output or changes the frequency by specifying the frequency.
- freq is specified as an Integer or Float.
- Specifying 0 stops the output.
- Changing the frequency does not affect the duty cycle.

Example of use:

```ruby
pwm1.frequency( 440 )   #  Output at 440Hz
```

Device-specific

- The maximum and minimum frequencies and resolutions depend on the device.
- When stopping the output, it is desirable to stop it at LowLevel.
- Whether to stop at the middle of a cycle (become LowLevel) or after the output of that cycle is complete when stopping the output is device-specific.

---

### period_us( micro_sec )

- Starts the output or changes the period by specifying the time of one cycle in microseconds.
- micro_sec is specified as an Integer.
- Specifying 0 stops the output.
- Changing the period does not affect the duty cycle.

Example of use:

```ruby
pwm1.period_us( 2273 )   # 1/2273us = 440Hz
```

---

### duty( percent )

- Changes the duty cycle by specifying a percentage from 0 to 100.
- percent is specified as an Integer or Float.

Example of use:

```ruby
pwm1.duty( 50 )
```

---

### pulse_width_us( micro_sec )

- Specifies the time in microseconds for which the output stays ON in one cycle.
- micro_sec is specified as an Integer.
- If a time longer than one cycle is specified, it will be set to the maximum value that can be ON without an error.

Example of use:

```ruby
pwm1.pulse_width_us( 1000 )
```
