# ADC

- A class that supports Analog-to-Digital Conversion (`ADC`) functionality.
- Generally, it is capable of converting analog voltage values to digital values.

## Constructor

---

### ADC.new( pin, *params )

- Generate an `ADC` object by specifying the physical pin indicated by "pin."
- Typically, "pin" is specified as an integer, but alternative methods (e.g., "B1" in PIC) are also acceptable.
- If the device has a switch between analog and digital modes for the pin, it will be switched to analog mode at this point.
- Generally, there is no need to specify additional parameters, such as "params." However, for certain models that require additional feature specifications like sampling speed, they can be specified here.

Example of use:

```ruby
adc1 = ADC.new( 1 )
```

---

## Instance Method

---

### read_voltage() -> Float

- Reads the value and returns the voltage value (V).

Example of use:

```ruby
v1 = adc1.read_voltage()
```

Note:

- VM must be compiled with Float enabled.
(MRBC_USE_FLOAT macro. Default ON)
- If not available, NotImplementedError exception is raised.

---

### read() -> Float

- Alias for read_voltage

---

### read_raw() -> Integer

- Read a value and return the raw value (the value before conversion to voltage).

Example of use:

```ruby
v1 = adc1.read_raw()
```

---

---

---

---

---

---
