# UART

- A class that supports asynchronous serial communication.

---

## Constructor


### UART.new( id=nil, *params )

- Generates a `UART` object by specifying the physical unit indicated by "id."
- The default parameter values follow the settings below if possible.
    - Baud rate: 9600
    - Data bits: 8 bits
    - Stop bits: 1 bit
    - Parity: None
    - Flow control: None

Optional Parameters

| param | type | description |
| --- | --- | --- |
| baudrate | Integer | Baud rate (default 9600) |
| baud | (Same as above) |  |
| data_bits | Integer | Data bits (default 8) |
| stop_bits | Integer | Stop bits (default 1) |
| parity | Const | Parity bit (default NONE) |
| flow_control | Const | Flow control (default NONE) |
| unit | --- | Specifies the unit (same as parameter "id") |
| txd_pin | --- | Specifies the TxD pin |
| rxd_pin | --- | Specifies the RxD pin |
| rts_pin | --- | Specifies the RTS pin |
| cts_pin | --- | Specifies the CTS pin |

Parameter Constants

```ruby
UART::NONE
UART::EVEN
UART::ODD
UART::RTSCTS
```

Example of use:

```ruby
# Use UART1 with all default parameters.
uart1 = UART.new( 1 )

# Use the serial device with the specified device node at 19200 bps, even parity.
uart2 = UART.new("/dev/cu.usbserial1", baud:19200, parity:UART::EVEN )
```

Device-specific

- Not all parameters may be supported.

---

## Instance Methods


### setmode( *params )

- Changes the mode (parameters) of `UART`.
- The parameter specification follows that of the constructor.

Example of use:

```ruby
uart1.setmode( bardrate:38400 )
```

---

### read( read_bytes ) -> String

- Reads data of the specified number of bytes, `read_bytes`.
- If the specified number of bytes has not arrived, it will block until they arrive.

Example of use:

```ruby
val = uart1.read( 10 )
```

Note

- If you do not want to block, you can check the number of bytes that can be read in advance using the `bytes_available` method.

---

### write( string ) -> Integer

- Sends data.
- Returns the number of bytes sent.

Example of use:

```ruby
uart1.write("Output string\\r\\n")
```

Device-specific

- Depending on the low-level library or hardware FIFO used, the method may return before all data has been sent.
- If there is a transmission buffer, and all data can be stored in the buffer, it probably won't block.

---

### gets() -> String

- Reads a line of string. Internally, it returns the byte sequence until "\n" in the read buffer.
- If a complete line of data has not arrived, it will block until it arrives.

Example of use:

```ruby
val = uart1.gets()
```

Note

- If you do not want to block, you can check if reading a line is possible in advance using the `can_read_line` method.
- The maximum character length depends on the size of the read buffer. If the buffer becomes full with data that does not contain a newline character, it will not be possible to read using `gets`.

---

### puts( string ) -> nil

- Sends one line and sends a newline code at the end of the argument `string`.
- The newline code is LF only by default.

Example of use:

```ruby
uart1.puts("Output string")
```

---

### bytes_available() -> Integer

- Returns the number of readable bytes in the read buffer.

Example of use:

```ruby
len = uart1.bytes_available()
```

---

### bytes_to_write() -> Integer

- Returns the number of bytes of data in the transmission buffer that have not been actually sent.

Example of use:

```ruby
bytes = uart1.bytes_to_write()
```

Note

- For devices without a buffer, this function always returns 0.

---

### can_read_line() -> bool

- Returns true if reading a line of data is possible.

Example of use:

```ruby
flag = uart1.can_read_line()
```

---

### flush()

- Returns true if reading a line of data is possible.

Example of use:

```ruby
uart1.flush()
```

---

### clear_rx_buffer()

- Clears the receive buffer.

Example of use:

```ruby
uart1.clear_rx_buffer()
```

---

### clear_tx_buffer()

- Clears the transmission buffer.

Example of use:

```ruby
uart1.clear_tx_buffer()
```

---

### send_break( time )

- Sends a break signal.
- The `time` is optional and specified in seconds.

Example of use:

```ruby
uart1.send_break( 0.1 )
```

Device-specific

- Depending on the hardware, `time` may be fixed (e.g., 12 bits) and cannot be changed.
