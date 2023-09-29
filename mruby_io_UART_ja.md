# UART

- 非同期シリアル通信をサポートするクラス。

---

## コンストラクタ


### UART.new( id=nil, *params )

- id で示す物理ユニットを指定して、UART オブジェクトを生成する。
- パラメータデフォルト値は、可能であれば以下の設定に従う。
    - ボーレート 9600
    - データ8ビット
    - ストップビット1ビット
    - パリティー無し
    - フロー制御無し

オプションパラメータ

| param | type | description |
| --- | --- | --- |
| baudrate | Integer | ボーレート (default 9600) |
| baud | 同上 |  |
| data_bits | Integer | データビット (default 8) |
| stop_bits | Integer | ストップビット (default 1) |
| parity | Const | パリティービット (default NONE) |
| flow_control | Const | フロー制御 (default NONE) |
| unit | --- | ユニットの指定（パラメータidと同じ） |
| txd_pin | --- | TxDピンの指定 |
| rxd_pin | --- | RxDピンの指定 |
| rts_pin | --- | RTSピンの指定 |
| cts_pin | --- | CTSピンの指定 |

パラメータ用定数

```ruby
UART::NONE
UART::EVEN
UART::ODD
UART::RTSCTS
```

実行例

```ruby
# UART1を、全てデフォルトパラメータで使う。
uart1 = UART.new( 1 )

# 指定したデバイスノードを持つシリアルデバイスを、19200bps 偶数パリティーで使う。
uart2 = UART.new("/dev/cu.usbserial1", baud:19200, parity:UART::EVEN )
```

機種依存

- 全てのパラメータがサポートされているとは限らない。

---

## インスタンスメソッド


### setmode( *params )

- UARTのモード（パラメータ）を変更する。
- パラメータの指定は、コンストラクタに準拠する。

実行例

```ruby
uart1.setmode( baudrate:38400 )
```

---

### read( read_bytes ) -> String

- read_bytes で指定されたバイト数のデータを読み込む。
- 指定されたバイト数のデータが到着していない場合は、到着までブロックする。

実行例

```ruby
val = uart1.read( 10 )
```

ノート

- ブロックさせたくない場合は、あらかじめ `bytes_available` メソッドで読み込むことができるバイト数を確認できる。

---

### write( string ) -> Integer

- データを送信する。
- 送信したバイト数を返す。

実行例

```ruby
uart1.write("Output string\r\n")
```

機種依存

- 利用する低レイヤーライブラリやハードウェアFIFOなどにより、全データの送信完了を待たずに戻る場合がある。
- 送信バッファがあり、全データがバッファへ格納できる場合は、ブロックしないだろう。

---

### gets() -> String

- 文字列を一行読み込む。内部的にはリードバッファ内の "\n" までのバイト列を返す。
- 1行のデータが到着していない場合は、到着までブロックする。

実行例

```ruby
val = uart1.gets()
```

ノート

- ブロックさせたくない場合は、あらかじめ `can_read_line` メソッドで行読み込みが可能かを確認できる。
- 最大文字長はリードバッファのサイズに左右され、改行文字が入らないデータでバッファフルになった場合、gets で読み込むことはできないだろう。

---

### puts( string ) -> nil

- 1行送信し、引数 string が改行で終わっていない場合は改行コードを送信する。
- 改行コードは、デフォルトでは LF のみ。

実行例

```ruby
uart1.puts("Output string")
```

---

### bytes_available() -> Integer

- リードバッファに到着している読み込み可能バイト数を返す。

実行例

```ruby
len = uart1.bytes_available()
```

---

### bytes_to_write() -> Integer

- 送信バッファに溜まっており、実際に出力されていないデータのバイト数を返す。

実行例

```ruby
bytes = uart1.bytes_to_write()
```

ノート

- バッファの無いデバイスの場合は、この関数は常に0を返す。

---

### can_read_line() -> bool

- 1行のデータを読み込むことができる場合は、true を返す。

実行例

```ruby
flag = uart1.can_read_line()
```

---

### flush()

- 送信バッファに溜まったデータの送信完了までブロックする。

実行例

```ruby
uart1.flush()
```

---

### clear_rx_buffer()

- 受信バッファをクリアする。

実行例

```ruby
uart1.clear_rx_buffer()
```

---

### clear_tx_buffer()

- 送信バッファをクリアする。

実行例

```ruby
uart1.clear_tx_buffer()
```

---

### send_break( time )

- break 信号を送信する。
- time はオプションで、秒で指定する。

実行例

```ruby
uart1.send_break( 0.1 )
```

機種依存

- ハードウェアによっては、timeは一律（e.g. 12bit）で、変更できない。
