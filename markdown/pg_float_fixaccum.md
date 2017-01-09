Time-stamp: <2017-01-10 00:04:20 hamada>

# pg_float_fixaccum : 型変換付き(浮動小数点⇒固定小数点) 固定小数点型 積算

## 書式

pg_float_fixaccum(*x*, *z*, *NFLO*, *NMAN*, *NFIX*, *NACC*, *NST*, *RM*);<br>

## 引数

- *x*:  入力値
- *z*:  演算結果 : xの積算
- *NFLO*: 浮動小数点 *x* ワードビット幅
- *NAN*: 浮動小数点数 *x* 仮数部ビット幅
- *NFIX*: 変換後固定小数点(符号付絶対値表現) ワードビット幅
- *NACC*: 出力(および積算) 固定小数点(2補数表現) *z* ワードビット幅
- *NST*:  パイプラインステージ数 (2以上が必要)
- *RM*:  変換丸めモード : 省略可. 省略時はTRUNC.

## 記述例

```
pg_float_fixaccum(x, z, 18,  8, 49, 57, 2);
pg_float_fixaccum(x, z, 26, 16, 57, 64, 4);
```

## 概要
 
入力の積算
```
     z += x;
```
を計算する.
浮動小数点型の入力(NFLOビット)を受け取り, 固定小数点型(NFIXビット)へ変換し
NACCビットで積算する.

内部は単純に<b>pg_conv_floattofix</b>と<b>pg_fix_accum</b>を使用しているだけです.

## 参照

- <a href="pg_conv_floattofix.md">pg_conv_floattofix</a>
- <a href="pg_fix_accum.md">pg_fix_accum</a>


## Change Log

- 2004.10.30 : the first edition by T. Hamada
