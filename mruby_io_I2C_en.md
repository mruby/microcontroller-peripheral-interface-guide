# I2C

- A class that supports I2C bus.
- This specification supports only master devices with 7-bit addresses.
- Define two types of methods: convenience methods, which are easy to use, and low-level methods, which provide detailed control.
- The low-level methods should be implemented on an as-needed basis, since it is unlikely that they will be needed in most cases.

---

## Constructor


### I2C.new( id=nil, *params )

- Generates an I2C object by specifying the physical unit indicated by "id."
- "id" can be omitted when there is only one physical unit.

Optional Parameters

| param | type | description |
| --- | --- | --- |
| frequency | Integer | Frequency (default 100kHz) |
| freq | (Same as above) |  |
| scl_pin | --- | SCL pin specification |
| sda_pin | --- | SDA pin specification |

Example of use:

```ruby
# Generate an I2C object with the default settings.
i2c = I2C.new()

# Use an I2C device of unit 1 with a frequency of 400kHz.
i2c = I2C.new(1, frequency:400_000 )   # 400kHz
```

Device-specific

- Not all parameters may be supported.

---

## Instance Methods


### read( i2c_adrs_7, read_bytes, *param ) -> String

- Reads data of read_bytes bytes from the device with the address i2c_adrs_7.
- If the device returns NAK in the middle, a String of shorter length than read_bytes may be returned.
- If data is specified in the param, it will be output before the repeated start, and then reading will start.
- Output specifications are the same as for write().

Example of use:

```ruby
s = i2c.read( 0x45, 3 )                 # case 1
s = i2c.read( 0x45, 3, 0xf3, 0x2d )     # case 2
```

I2C bus sequence

```
(case 1) S -- adrs(45) R A -- data1 A -- ... -- data3 A|N -- P
(case 2) S -- adrs(45) W A -- out1(f3) A ... out2(2d) A -- Sr -- adrs(45) R A -- data1 A -- ... data3 N -- P
    S : Start condition
    P : Stop condition
    Sr: Repeated start condition
    A : Ack
    N : Nack
    R : Read bit
    W : Write bit
```

---

### write( i2c_adrs_7 , *outputs ) -> Integer

- Writes data specified in outputs to the device with the address i2c_adrs_7.
- The number of bytes successfully written is returned as the return value.
- outputs can be specified as an Integer, Array<Integer>, or String.

Example of use:

```ruby
i2c.write( 0x45, 0x30, 0xa2 )
i2c.write( 0x50, 0x00, 0x80, data_string )  # useful for EEPROM
i2c.write( 0x11, [0x30, 0xa2] )
```

I2C Sequence

```
S -- adrs W A -- data_1 A -- ... -- data_n N -- P
    S : Start condition
    P : Stop condition
    A : Ack
    N : Nack
    W : Write bit
```

---

### send_start()

- Low level method.
- Outputs a StartCondition to the I2C bus.

Example of use:

```ruby
i2c.send_start
```

---

### send_restart()

- Low level method.
- Outputs a Restart (RepeatedStart) Condition to the I2C bus.

Example of use:

```ruby
i2c.send_restart
```

---

### send_stop()

- Low level method.
- Outputs a StopCondition to the I2C bus.

Example of use:

```ruby
i2c.send_stop
```

---

### raw_read( read_bytes, ack_nack = false ) -> String

- Low level method.
- Reads read_bytes bytes from the I2C bus and returns them.
- ack_nack = true outputs ACK at the last byte reading, and false outputs NACK.

Example of use:

```ruby
str = i2c.raw_read( 20 )
```

---

### raw_write( *outputs ) -> Integer

- Low level method.
- Writes data specified in outputs to the I2C bus.
- The number of bytes successfully written is returned as the return value.
- outputs can be specified as an Integer, Array<Integer>, or String.

Example of use:

```ruby
i2c.raw_write( 0x45, 0x30, 0xa2 )
```
