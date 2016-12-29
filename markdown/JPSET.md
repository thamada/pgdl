Time-stamp: <2016-12-30 05:12:27 hamada>

# /JPSET

------------------------------
## 書式

/JPSET *vname*, *iname*, <b>fix</b>,   *len*, *scale*;

/JPSET *vname*, *iname*, <b>ufix</b>,  *len*, *scale*, *offset*;

/JPSET *vname*, *iname*, <b>log</b>,   *len*, *mlen*,  *scale*;

/JPSET *vname*, *iname*, <b>float</b>, *len*, *mlen*,  *rmode*;

------------------------------
## 引数

- *vname*: データフロー記述部変数名
- *iname*: *vname*に対応するインタフェース引数
- <b>fix</b>, <b>ufix</b>, <b>log</b>, <b>float</b>: *vname*の型
- *len*: *vname*ビット長
- *mlen*: log,float型の仮数部ビット長
- *scale*: fix,ufix,log型に変換する時のスケーリング係数
- *offset*: ufix型に変換する時のオフセット値
- *rmode*: 丸めモード(省略可) FORCE1(force to one), TOEVEN(to the nearest even), TRUNC(truncation)が指定可能. 省略時はTOEVEN.

------------------------------
## 記述例

```
/JPSET xj[3], x[][], fix,   32,       (pow(2.0,32.0)/64.0);
/JPSET yj[3], x[][], ufix,  32,       (pow(2.0,32.0)/64.0), (32.0);
/JPSET hj,    h[],   log,   17,    8, (pow(2.0,60.0)/(1.0/1024.0));
/JPSET hj[3], h[][], float, 26,   16;
```

------------------------------
## 概要

/JPSETはj粒子メモリからデータフローユニットへ供給されるデータのデータ型
およびインターフェースプログラムの引数との関連付けを指定します.
<br><br>

記述例1行目では, データフロー部配列変数xj[3]がj粒子メモリから供給される
ものであることを示しています.  さらに, xj[3]は32ビット固定小数点
(<b>fix</b>)型で, ホストプログラムからはインタフェース引数にx[][]によっ
て値が渡されることも指定しています.  fix変換の詳細をC言語であらわすと次
のようになります.

```
      /JPSET xj[3], x[][], fix, 32, (pow(2.0,32.0)/64.0);

      xj[0] = ((int) (x[j][0] * (pow (2.0, 32.0) / 64.0) + 0.5)) & 0xFFFFFFFF;
      xj[1] = ((int) (x[j][1] * (pow (2.0, 32.0) / 64.0) + 0.5)) & 0xFFFFFFFF;
      xj[2] = ((int) (x[j][2] * (pow (2.0, 32.0) / 64.0) + 0.5)) & 0xFFFFFFFF;
```


同じく, 記述例2行目は, 配列yj[3]がj粒子メモリから供給されるものであるこ
とを示しています.  1行目との違いはyj[3]の型が32ビット符号無し固定小数点
(<b>ufix</b>)型であるということです. インタフェース引数x[][]を型変換す
る際にオフセット値(32.0)を加えて符号無し固定小数点型xj[3]に変換します.
ufix変換の詳細をC言語であらわすと次のようになります.

```
      /JPSET yj[3], x[][], ufix, 32, (pow(2.0,32.0)/64.0), (32.0);

      yj[0] = ((unsigned int) ((x[j][0] + (32.0)) * (pow (2.0, 32.0) / 64.0) + 0.5)) & 0xFFFFFFFF;
      yj[1] = ((unsigned int) ((x[j][1] + (32.0)) * (pow (2.0, 32.0) / 64.0) + 0.5)) & 0xFFFFFFFF;
      yj[2] = ((unsigned int) ((x[j][2] + (32.0)) * (pow (2.0, 32.0) / 64.0) + 0.5)) & 0xFFFFFFFF;
```

記述例3行目は, mjを対数(<b>log</b>)型変数をj粒子メモリに指定する例です.
同様にlog変換の詳細をC言語であらわすと次のようになります.

```
      /JPSET hj, h[], log, 17, 8, (pow(2.0,60.0)/(1.0/1024.0));

     if (h[j] == 0.0)
       hj = 0;
     else if(h[j] > 0.0)
       hj = (((int)(pow(2.0, 8.0) * log(h[j] * (pow(2.0, 60.0) / (1.0 / 1024.0))) / log(2.0))) & 0x7FFF) | 0x8000;
     else
       hj = (((int)(pow(2.0, 8.0) * log(-h[j] * (pow(2.0, 60.0) / (1.0 / 1024.0))) / log(2.0))) & 0x7FFF) | 0x18000;
```

log変換では上のようにif文による場合分けが生じます. これは(PGPGシステム
が使用する)log型ではゼロを特別なビット(非ゼロビット)を用いてあらわすた
め -- そして当然ですが数値の正負を符号ビットを用いてあらわすため -- で
す. 上記の17ビットlog型変換の例ではMSBが符号ビット, MSBの次が非ゼロビッ
トになっていることがわかります.
<br><br>

記述例4行目は, 浮動小数点(<b>float</b>)型のJPSETの例です.  float変換の
詳細をC言語であらわすと次のようになります.

```
      /JPSET hj, h[], float, 26,   16;

      if (h[j] == 0.0)
          hj = 0;
      else
        {
          unsigned long long int b = *((unsigned long long int *) (&h[j]));
          hj = ((0x8000000000000000ULL & b) >> 38)
            | (0x1000000ULL)
            | 0xffffffULL &  ( (0x3fffffffffffffffULL & (b-0x3ff0000000000000ULL)) >> 36 );
        }
```

上記のfloat変換は*rmode*を省略しているため丸め操作はデフォルトの
TRUNC(Truncation: 切り捨て) になっています.

------------------------------
## 参照
- j粒子メモリ
- データフローユニット
- インタフェース引数
- fix型
- ufix型
- log型
- float型


------------------------------
## Change Log

- 2004.08.17 : the first edition by T. Hamada
