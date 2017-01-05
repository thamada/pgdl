Time-stamp: <2017-01-05 22:48:21 hamada>

# pg_float_compz_abs : 浮動小数点型 絶対値 ゼロ比較


## 書式

pg_float_compz_abs(*x*, *y*, *flag*,   *NFLO*, *NMAN*, *NST*);

## 引数

- *x*: 被比較数
- *y*: 比較数
- *flag*: 比較結果: ```if(|x|>0) flag=1; else flag=0;```
- *NFLO*: 浮動小数点ワードビット幅
- *NMAN*: 仮数部(精度)ビット幅
- *NST*: パイプラインステージ数

## 記述例

```
pg_float_compz_abs(x, y, flag, 18,  8, 1);
pg_float_compz_abs(x, y, flag, 26, 16, 1);
pg_float_compz_abs(x, y, flag, 33, 23, 1);
```

## 概要

```
if(|x|>0) flag=1 else flag=0;
```

## 参照

## Change Log

- 2004.10.14 : the first edition by T. Hamada
