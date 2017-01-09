Time-stamp: <2017-01-10 00:04:20 hamada>

# pg_float_fixaccum : ���Ѵ��դ�(��ư�������͸��꾮����) ���꾮������ �ѻ�

## ��

pg_float_fixaccum(*x*, *z*, *NFLO*, *NMAN*, *NFIX*, *NACC*, *NST*, *RM*);<br>

## ����

- *x*:  ������
- *z*:  �黻��� : x���ѻ�
- *NFLO*: ��ư������ *x* ��ɥӥå���
- *NAN*: ��ư�������� *x* �������ӥå���
- *NFIX*: �Ѵ�����꾮����(�����������ɽ��) ��ɥӥå���
- *NACC*: ����(������ѻ�) ���꾮����(2���ɽ��) *z* ��ɥӥå���
- *NST*:  �ѥ��ץ饤�󥹥ơ����� (2�ʾ夬ɬ��)
- *RM*:  �Ѵ��ݤ�⡼�� : ��ά��. ��ά����TRUNC.

## ������

```
pg_float_fixaccum(x, z, 18,  8, 49, 57, 2);
pg_float_fixaccum(x, z, 26, 16, 57, 64, 4);
```

## ����
 
���Ϥ��ѻ�
```
     z += x;
```
��׻�����.
��ư��������������(NFLO�ӥå�)��������, ���꾮������(NFIX�ӥå�)���Ѵ���
NACC�ӥåȤ��ѻ�����.

������ñ���<b>pg_conv_floattofix</b>��<b>pg_fix_accum</b>����Ѥ��Ƥ�������Ǥ�.

## ����

- <a href="pg_conv_floattofix.md">pg_conv_floattofix</a>
- <a href="pg_fix_accum.md">pg_fix_accum</a>


## Change Log

- 2004.10.30 : the first edition by T. Hamada
