Time-stamp: <2016-12-29 02:11:47 hamada>

# /FOSET

------------------------------
## 書式

/FOSET *vname*, *iname*, <b>fix</b>,   *len*, *scale*;

/FOSET *vname*, *iname*, <b>ufix</b>,  *len*, *scale*, *offset*;

/FOSET *vname*, *iname*, <b>log</b>,   *len*, *mlen*,  *scale*;

/FOSET *vname*, *iname*, <b>float</b>, *len*, *mlen*,  *rmode*;

------------------------------
## 引数

- *vname: データフロー記述部変数名
- *iname: *vname*に対応するインタフェース引数
- **fix**, **ufix**, **log**, **float**: *vname*の型
- *len*: *vname*ビット長
- *mlen*: **log**, **float**型の仮数部ビット長
- *scale*: **fix**, **ufix**, **log**型に変換する時のスケーリング係数
- *offset*: ufix型に変換する時のオフセット値
- *rmode*: 丸めモード(省略可) FORCE1(force to one), TOEVEN(to the nearest even), TRUNC(truncation)が指定可能. 省略時はTOEVEN.

------------------------------
## 記述例

```
/FOSET xi[3], x[][], fix,   32,       (pow(2.0,32.0)/64.0);
/FOSET yi[3], x[][], ufix,  32,       (pow(2.0,32.0)/64.0), (32.0);
/FOSET hi,    h[],   log,   17,    8, (pow(2.0,60.0)/(1.0/1024.0));
/FOSET hi[3], h[][], float, 26,   16;
```

------------------------------
## 概要

/FOSETはデータフローユニットの計算結果を回収するための演算結果レジスタのレイアウトを指定します.


------------------------------
## 参照

------------------------------
## Change Log

- 2004.08.16 : the first edition
