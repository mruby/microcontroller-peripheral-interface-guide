# GPIO

- 汎用デジタル入出力機能をサポートするクラス。
- 一般には、一つの物理ピンの入力もしくは出力の制御を想定している。

---

## クラスメソッド


### GPIO.setmode( pin, params ) -> nil

- pin で示す物理ピンを指定して、GPIO のモードを変更する。
- その他は、コンストラクタを参照。

使用例

```
# ピン番号1を出力に設定する。
GPIO.setmode( 1, GPIO::OUT )

# B1ピンを入力、内部プルアップに設定する。（PIC等）
GPIO.setmode( "B1", GPIO::IN|GPIO::PULL_UP )
```

---

### read_at( pin ) -> Integer

- 指定したピンから読み込んだ値を、0または1で返す。
- 出力に設定したピンに対して read する場合は、ハードウェアが許せば、設定値ではなく実際の値を読んで返すべき。

使用例

```
GPIO.setmode( 1, GPIO::IN )
v1 = GPIO.read_at( 1 )          # read from pin 1.
```

---

### high_at?( pin ) -> bool

- 指定したピンから読み込んだ値が、ハイレベル (==1) であれば、true を返す。

使用例

```
if GPIO.high_at?( 1 )
```

---

### low_at?( pin ) -> bool

- 指定したピンから読み込んだ値が、ローレベル (==0) であれば、true を返す。

使用例

```
if GPIO.low_at?( 1 )
```

---

### write_at( pin, integer_data )

- 指定したピンへ値を出力する。
- 値は0もしくは1で指定する。
- データが範囲外の場合（通常 0と1以外）、RangeErrorになる。

使用例

```
GPIO.setmode( 1, GPIO::OUT )
GPIO.write_at( 1, 0 )      # output zero to pin 1.
```

---

## コンストラクタ


### GPIO.new( pin, params )

- pin で示す物理ピンを指定して、GPIO オブジェクトを生成する。
- 同時に param を指定して、入出力方向などのモードを指示する。
- pin は標準的には整数で指定するが、別な方法（例えばPICでは"B1"等）があっても良い。
- 1ビットを基本とするが、機器によっては必要に応じて複数ビットまとめたピン指定もありうる。
- param は以下の定数を使い、`|`で接続して指定する。
- IN, OUT もしくは HIGH_Z の指示は必須とし、無き場合は ArgumentError が発生する。

定数

```
GPIO::IN            # 入力に設定する
GPIO::OUT           # 出力に設定する
GPIO::HIGH_Z        # ハイインピーダンスに設定する
GPIO::PULL_UP       # 内部プルアップを有効にする
GPIO::PULL_DOWN     # 内部プルダウンを有効にする
GPIO::OPEN_DRAIN    # オープンドレインモードに設定する
```

使用例

```
# ピン番号1を出力に設定する。
gpio1 = GPIO.new( 1, GPIO::OUT )

# B1ピンを入力、内部プルアップに設定する。（PIC等）
gpio1 = GPIO.new( "B1", GPIO::IN|GPIO::PULL_UP )
```

---

## インスタンスメソッド


### read() -> Integer

- 読み込んだ値を、0または1で返す。
- 複数ビットまとめて扱う実装の場合のみ、任意の整数になりうる。
- 出力に設定したピンに対して read する場合は、ハードウェアが許せば、設定値ではなく実際の値を読んで返すべき。

使用例

```
v1 = gpio1.read()
```

---

### high?() -> bool

- 読み込んだ値が、ハイレベル (==1) であれば、true を返す。

使用例

```
if gpio1.high?()
```

---

### low?() -> bool

- 読み込んだ値が、ローレベル (==0) であれば、true を返す。

使用例

```
if gpio1.low?()
```

---

### write( integer_data )

- ピンへ出力する値を0もしくは1で指定する。
- 複数ビットまとめて扱う実装の場合のみ、任意の整数で指示する。

使用例

```
gpio1.write( 1 )
```

---

### setmode( param ) -> nil

- GPIOのモードを任意のタイミングで変更する。
- 既に PULL_UP 等が設定されている時に IN,OUT もしくは HIGH_Z が指定された場合は、PULL_UP 等の設定は無効化される。

使用例

```ruby
# 内部プルアップを有効にする
gpio1.setmode( GPIO::PULL_UP )

# 入力に切り替え、内部プルアップを有効にする。
gpio1.setmode( GPIO::IN|GPIO::PULL_UP )
```
