# GPIO

- A class that supports general-purpose digital input/output functionality.
- Generally, it is intended for controlling the input or output of a single physical pin.

---

## Class method


### GPIO.setmode( pin, params ) -> nil

- Specify the physical pin indicated by "pin" and change the mode of the GPIO.
- Refer to the constructor for other information.

Example of use:

```ruby
# Set pin number 1 output.
GPIO.setmode( 1, GPIO::OUT )

# Set B1 pin to input, with internal pull-up. (for PIC, etc.)
GPIO.setmode( "B1", GPIO::IN|GPIO::PULL_UP )
```

---

### read_at( pin ) -> Integer

- Returns the value read from the specified pin as either 0 or 1.
- When reading from the pin set for output, if the hardware permits, the actual value should be read and returned instead of the set value.

Example of use:

```ruby
GPIO.setmode( 1, GPIO::IN )
v1 = GPIO.read_at( 1 )          # read from pin 1.
```

---

### high_at?( pin ) -> bool

- Return true If the value read from the specified pin is high (==1)

Example of use:

```ruby
if GPIO.high_at?( 1 )
```

---

### low_at?( pin ) -> bool

- If the value loaded from the specified pin is low-level (== 0), return true.

Example of use:

```ruby
if GPIO.low_at?( 1 )
```

---

### write_at( pin, integer_data )

- Output a value to the specified pin.
- The value must be specified as either 0 or 1.
- If the data is out of range (i.e. not 0 or 1), a RangeError will occur.

Example of use:

```ruby
GPIO.setmode( 1, GPIO::OUT )
GPIO.write_at( 1, 0 )      # output zero to pin 1.
```

---

## Constructor


### GPIO.new( pin, params )

- Specify the physical pin indicated by the pin and generate a GPIO object.
- At the same time, specify a param to indicate the mode, such as input/output direction.
- The pin is typically specified as an integer, but other methods (such as "B1" in PIC) may be used.
- Although one bit is the basic unit, pin specification that combines multiple bits may be necessary depending on the device.
- Use the following constants for param and specify them connected by `|`.
- IN, OUT, or HIGH_Z instruction is mandatory, and an ArgumentError will occur if it is absent.

Constants:

```ruby
GPIO::IN            # Set as input
GPIO::OUT           # Set as output
GPIO::HIGH_Z        # Set as high impedance
GPIO::PULL_UP       # Enable internal pull-up
GPIO::PULL_DOWN     # Enable internal pull-down
GPIO::OPEN_DRAIN    # Set to open-drain mode
```

Usage Example

```ruby
# Set GPIO pin 1 as output.
gpio1 = GPIO.new(1, GPIO::OUT)

# Set B1 pin as input with internal pull-up (for PIC, etc.).
gpio1 = GPIO.new("B1", GPIO::IN|GPIO::PULL_UP)
```

---

## Instance methods


### read() -> Integer

- Return the loaded value as 0 or 1.
- It can be any integer only when implementing multiple bit processing.
- If reading from the pin set for output, the hardware should read and return the actual value if possible, not the set value.

Example of use:

```ruby
v1 = gpio1.read()
```

---

### high?() -> bool

- If the loaded value is high level (==1), it returns true.

Example of use:

```ruby
if gpio1.high?()
```

---

### low?() -> bool

If the loaded value is low-level (==0), return true.

Example of use:

```ruby
if gpio1.low?()
```

---

### write( integer_data )

- Specify the value to output to the pin as either 0 or 1.
- Only in the case of implementation that handles multiple bits collectively, indicate with any integer value.

Example of use:

```ruby
gpio1.write( 1 )
```

---

### setmode( param ) -> nil

- Change the GPIO mode at any timing.
- When IN, OUT, or HIGH_Z is specified while PULL_UP or other settings have already been set, the previous settings will be invalidated.

Example of use:

```ruby
# Enable internal pullup.
gpio1.setmode( GPIO::PULL_UP )

# Switch to input and enable internal pullup.
gpio1.setmode( GPIO::IN|GPIO::PULL_UP )
```
