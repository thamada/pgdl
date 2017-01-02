Time-stamp: <2017-01-03 03:28:28 hamada>

# /VALSET


------------------------------
## 書式

/VALSET *vname*, *hex_value*,  *nbit_lsb*, *nbit_msb*;



------------------------------
## 引数

- *vname*: データフロー記述部変数名
- *hex_value*: 定数(C言語16進リテラル)
- *nbit_lsb*: VHDL signalでの(nbit_msb downto <b>nbit_lsb</b>)のnbit_lsb
- *nbit_msb*: VHDL signalでの(<b>nbit_msb</b> downto nbit_lsb)のnbit_msb


------------------------------
## 記述例

```
/VALSET mmax ,0x9700, 0, 16;
```

------------------------------
## 概要

/VALSETはデータフロー記述中でビット列定数を指定するためのものである.
/VASETはHDLに精通したプログラマ用にあるもので, 
通常は/CONST_FLOATもしくは /CONST_LOGを使用すれば事足りる.

記述例のmmaxは, HDL中では,
```
  constant mmax : std_logic_vector(16 downto 0) := "01001011100000000"; -- 0x9700
```
という定数信号に変換される.


------------------------------
## 参照

------------------------------
## Change Log

- 2004.08.16 : the first edition
