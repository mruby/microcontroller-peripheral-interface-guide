# SPI

- A class that supports `SPI` bus.
- This specification defines only master devices and transfers in 8-bit units.
- The Chip Select (CS/SS) will be managed using `GPIO` functionality and is not defined within the `SPI` class.

---

## **Constructor**

---

### SPI.new( id=nil, *params )

- Generates an `SPI` object by specifying the physical unit indicated by "id."
- "id" can be omitted when there is only one physical unit.

Optional Parameters

| param | type | description |
| --- | --- | --- |
| unit | --- | Specification of the SPI unit |
| frequency | Integer | Frequency (default 1MHz) |
| mode | Integer | 0 to 3 (default 0) |
| first_bit | Constant | SPI::MSB_FIRST | SPI::LSB_FIRST (default MSB_FIRST) |

Example of use:

```ruby
# Generate an SPI object with the default settings.
spi = SPI.new()

# Use sn SPI device of unit 1 with a frequency of 10MHz.
spi = SPI.new( unit:1, frequency:10_000_000 )
```

Device-specific

- Not all parameters may be supported.

---

## **Instance Methods**

---

### setmode( *params )

- Changes the operating mode (parameters) of the `SPI`.
- The parameters are specified according to the constructor.

Example of use:

```ruby
spi.setmode( mode:3 )
```

---

### read( read_bytes ) -> String

- Reads data of **`read_bytes`** bytes from the SPI bus.
- At the same time, data will be output as 0.

Example of use:

```ruby
data = spi.read( 32 )
```

---

### write( *outputs ) -> nil

- Outputs data specified in **`outputs`** to the SPI bus.
- **`outputs`** can be specified as an Integer, Array<Integer>, or String.

Example of use:

```ruby
spi.write( 0x30, 0xa2 )
spi.write( "\\x30\\xa2" )
i2c.write( 0x02, 0xee, 0xad, 0x00, data_string )  # useful for EEPROM
```

---

### transfer( outputs, additional_read_bytes = 0 ) -> String

- Outputs data specified in **`outputs`** to the SPI bus while simultaneously reading data (General-purpose transfer).
- **`outputs`** can be specified as an Integer, Array<Integer>, or String.
- If **`additional_read_bytes`** is specified, it will output 0x00 after the **`outputs`**.

Example of use:

```ruby
s = spi.transfer( 0b0000_0101, 1 )  # s will return a 2-byte String
```
