Time-stamp: <2017-01-05 22:48:21 hamada>

# pg_float_compz_abs : ��ư�������� ������ �������


## ��

pg_float_compz_abs(*x*, *y*, *flag*,   *NFLO*, *NMAN*, *NST*);

## ����

- *x*: ����ӿ�
- *y*: ��ӿ�
- *flag*: ��ӷ��: ```if(|x|>0) flag=1; else flag=0;```
- *NFLO*: ��ư��������ɥӥå���
- *NMAN*: ������(����)�ӥå���
- *NST*: �ѥ��ץ饤�󥹥ơ�����

## ������

```
pg_float_compz_abs(x, y, flag, 18,  8, 1);
pg_float_compz_abs(x, y, flag, 26, 16, 1);
pg_float_compz_abs(x, y, flag, 33, 23, 1);
```

## ����

```
if(|x|>0) flag=1 else flag=0;
```

## ����

## Change Log

- 2004.10.14 : the first edition by T. Hamada
