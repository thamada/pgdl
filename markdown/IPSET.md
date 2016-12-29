Time-stamp: <2016-12-30 05:08:14 hamada>

# /IPSET


------------------------------
## 書式

/IPSET *vname*, *iname*, <b>fix</b>,   *len*, *scale*;

/IPSET *vname*, *iname*, <b>ufix</b>,  *len*, *scale*, *offset*;

/IPSET *vname*, *iname*, <b>log</b>,   *len*, *mlen*,  *scale*;

/IPSET *vname*, *iname*, <b>float</b>, *len*, *mlen*,  *rmode*;

------------------------------
## 引数

- *vname*: データフロー記述部変数名
- *iname*: *vname*に対応するインタフェース引数
- <b>fix</b>, <b>ufix</b>, <b>log</b>, <b>float</b>: *vname*の型
- *len*: *vname*ビット長
- *mlen*: <b>log</b>,<b>float</b>型の仮数部ビット長
- *scale*: <b>fix</b>,<b>ufix</b>,<b>log</b>型に変換する時のスケーリング係数
- *offset*: <b>ufix</b>型に変換する時のオフセット値
- *rmode*: 丸めモード(省略可) FORCE1(force to one), TOEVEN(to the nearest even), TRUNC(truncation)が指定可能. 省略時はTOEVEN.

------------------------------
## 記述例

```
/IPSET xi[3], x[][], fix,   32,       (pow(2.0,32.0)/64.0);
/IPSET yi[3], x[][], ufix,  32,       (pow(2.0,32.0)/64.0), (32.0);
/IPSET hi,    h[],   log,   17,    8, (pow(2.0,60.0)/(1.0/1024.0));
/IPSET hi[3], h[][], float, 26,   16;
```

------------------------------
## 概要

/IPSETはIPSETからデータフローユニットへ供給されるデータのデータ型および
インターフェースプログラムの引数との関連付けを指定します.
<br><br>

記述例1行目では, データフロー部配列変数xi[3]がi粒子レジスタから供給
されるものであることを示しています.  さらに, xi[3]は32ビット固定小数点
(<b>fix</b>)型で, ホストプログラムからはインタフェース引数にx[][]によって値が渡さ
れることも指定しています.
fix変換の詳細をC言語であらわすと次のようになります.

```
/IPSET xi[3], x[][], fix, 32, (pow(2.0,32.0)/64.0);

xi[0] = ((int) (x[i][0] * (pow (2.0, 32.0) / 64.0) + 0.5)) & 0xFFFFFFFF;
xi[1] = ((int) (x[i][1] * (pow (2.0, 32.0) / 64.0) + 0.5)) & 0xFFFFFFFF;
xi[2] = ((int) (x[i][2] * (pow (2.0, 32.0) / 64.0) + 0.5)) & 0xFFFFFFFF;
```


同じく, 記述例2行目は, 配列yi[3]がi粒子レジスタから供給されるもので
あることを示しています.  1行目との違いはyi[3]の型が32ビット符号無し固
定小数点(<b>ufix</b>)型であるということです. インタフェース引数x[][]を型変換する際
にオフセット値(32.0)を加えて符号無し固定小数点型yi[3]に変換します.
ufix変換の詳細をC言語であらわすと次のようになります.

```
/IPSET yi[3], x[][], ufix, 32, (pow(2.0,32.0)/64.0), (32.0);

yi[0] = ((unsigned int) ((x[i][0] + (32.0)) * (pow (2.0, 32.0) / 64.0) + 0.5)) & 0xFFFFFFFF;
yi[1] = ((unsigned int) ((x[i][1] + (32.0)) * (pow (2.0, 32.0) / 64.0) + 0.5)) & 0xFFFFFFFF;
yi[2] = ((unsigned int) ((x[i][2] + (32.0)) * (pow (2.0, 32.0) / 64.0) + 0.5)) & 0xFFFFFFFF;
```

記述例3行目は, 対数(<b>log</b>)型変数hiをi粒子レジスタに対応づける例で
す.  同様にlog変換の詳細をC言語であらわすと次のようになります.

```
/IPSET hi, h[], log, 17, 8, (pow(2.0,60.0)/(1.0/1024.0));

if (h[i] == 0.0)
    hi = 0;
else if(h[i] > 0.0)
    hi = (((int)(pow(2.0, 8.0) * log(h[i] * (pow(2.0, 60.0) / (1.0 / 1024.0))) / log(2.0))) & 0x7FFF) | 0x8000;
else
    hi = (((int)(pow(2.0, 8.0) * log(-h[i] * (pow(2.0, 60.0) / (1.0 / 1024.0))) / log(2.0))) & 0x7FFF) | 0x18000;
```

log変換では上のようにif文による場合分けが生じます. これは(PGPGシステム
が使用する)log型ではゼロを特別なビット(非ゼロビット)を用いてあらわすた
め -- そして当然ですが数値の正負を符号ビットを用いてあらわすため -- で
す. 上記の17ビットlog型変換の例ではMSBが符号ビット, MSBの次が非ゼロビッ
トになっていることがわかります.
<br><br>

記述例4行目は, 浮動小数点(<b>float</b>)型のIPSETの例です. float変換の
詳細をC言語であらわすと次のようになります.

```
/IPSET hi, h[], float, 26,   16;
      

if (h[i] == 0.0)
    hi = 0;
else {
    unsigned long long int b = *((unsigned long long int *) (&h[i]));
    hi = ((0x8000000000000000ULL & b) >> 38)
        | (0x1000000ULL)
        | 0xffffffULL &  ( (0x3fffffffffffffffULL & (b-0x3ff0000000000000ULL)) >> 36 );
}
```

上記のfloat変換は*rmode*を省略しているため丸め操作はデフォルトの
TRUNC(Truncation: 切り捨て) になっています.



------------------------------
## 参照

------------------------------
## Change Log

- 2004.08.17 : the first edition by T. Hamada
