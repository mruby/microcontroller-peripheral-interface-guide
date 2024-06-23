# I2C

- I2C バスをサポートするクラス。
- この仕様書ではマスターデバイス、7ビットアドレスのみをサポートしている。
- 容易に使う事ができるコンビニエンスメソッドと、細かな制御ができる低レベルメソッドを定義する。
- 低レベルメソッドは、ほとんどの場合必要ないと思われるので、必要に応じて実装すれば良い。

---

## コンストラクタ


### I2C.new( id=nil, *params )

- id で示す物理ユニットを指定して、I2C オブジェクトを生成する。
- 物理ユニットが１つしか無い場合などは、idは省略可能とする。

オプションパラメータ

| param | type | description |
| --- | --- | --- |
| frequency | Integer | 周波数 (デフォルト 100kHz) |
| freq | 同上 |  |
| scl_pin | --- | クロックピンの指定 |
| sda_pin | --- | データピンの指定 |

使用例

```ruby
# デフォルトの設定で、i2cオブジェクトを生成する。
i2c = I2C.new()

# ユニット1 の I2C デバイスを、周波数 400kHz で使う。
i2c = I2C.new(1, frequency:400_000 )   # 400kHz
```

機種依存

- 全てのパラメータがサポートされているとは限らない。

---

## インスタンスメソッド


### read( i2c_adrs_7, read_bytes, *param ) -> String

- i2c_adrs_7 アドレスのデバイスから、read_bytes バイトのデータを読み込む。
- デバイスが途中で NAK を返す場合は、read_bytes より短い長さの String が返る可能性がある。
- param にデータが指定されていれば、それを出力してからリピーテッドスタートを挟み読み込みを始める。
- 出力の仕様は、write() を参照。

使用例

```ruby
s = i2c.read( 0x45, 3 )                 # case 1
s = i2c.read( 0x45, 3, 0xf3, 0x2d )     # case 2
```

I2C bus sequence

```
(case 1) S -- adrs(45) R A -- data1 A -- data2 A -- data3 A|N -- P
(case 2) S -- adrs(45) W A -- out1(f3) A -- out2(2d) A -- Sr -- adrs(45) R A -- data1 A -- data2 A -- data3 N -- P
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

- i2c_adrs_7 アドレスのデバイスに、outputs で指定したデータを書き込む。
- 書き込みできたバイト数が戻り値として返る。
- outputsは、Integer, Array\<Integer\> および String で指定する。

使用例

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
- I2Cバスに StartCondition を出力する。

使用例

```ruby
i2c.send_start
```

---

### send_restart()

- Low level method.
- I2Cバスに Restart (RepeatedStart) Condition を出力する。

使用例

```ruby
i2c.send_restart
```

---

### send_stop()

- Low level method.
- I2Cバスに StopCondition を出力する。

使用例

```ruby
i2c.send_stop
```

---

### raw_read( read_bytes, ack_nack = false ) -> String

- Low level method.
- I2Cバスから read_bytes バイト読み込んで返す。
- ack_nack = true で最後のバイト読み込み時に ACK を、false で NACK 出力する。

使用例

```ruby
str = i2c.raw_read( 20 )
```

---

### raw_write( *outputs ) -> Integer

- Low level method.
- I2Cバスへ outputs で指定したデータを書き込む。
- 書き込みできたバイト数が戻り値として返る。
- outputsは、Integer, Array\<Integer\> および String で指定する。

使用例

```ruby
i2c.raw_write( 0x45, 0x30, 0xa2 )
```
