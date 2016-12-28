Time-stamp: <2016-12-29 02:11:47 hamada>

# /FOSET

------------------------------
## ��

/FOSET *vname*, *iname*, <b>fix</b>,   *len*, *scale*;

/FOSET *vname*, *iname*, <b>ufix</b>,  *len*, *scale*, *offset*;

/FOSET *vname*, *iname*, <b>log</b>,   *len*, *mlen*,  *scale*;

/FOSET *vname*, *iname*, <b>float</b>, *len*, *mlen*,  *rmode*;

------------------------------
## ����

- *vname: �ǡ����ե��������ѿ�̾
- *iname: *vname*���б����륤�󥿥ե���������
- **fix**, **ufix**, **log**, **float**: *vname*�η�
- *len*: *vname*�ӥå�Ĺ
- *mlen*: **log**, **float**���β������ӥå�Ĺ
- *scale*: **fix**, **ufix**, **log**�����Ѵ�������Υ�������󥰷���
- *offset*: ufix�����Ѵ�������Υ��ե��å���
- *rmode*: �ݤ�⡼��(��ά��) FORCE1(force to one), TOEVEN(to the nearest even), TRUNC(truncation)�������ǽ. ��ά����TOEVEN.

------------------------------
## ������

```
/FOSET xi[3], x[][], fix,   32,       (pow(2.0,32.0)/64.0);
/FOSET yi[3], x[][], ufix,  32,       (pow(2.0,32.0)/64.0), (32.0);
/FOSET hi,    h[],   log,   17,    8, (pow(2.0,60.0)/(1.0/1024.0));
/FOSET hi[3], h[][], float, 26,   16;
```

------------------------------
## ����

/FOSET�ϥǡ����ե���˥åȤη׻���̤������뤿��α黻��̥쥸�����Υ쥤�����Ȥ���ꤷ�ޤ�.


------------------------------
## ����

------------------------------
## Change Log

- 2004.08.16 : the first edition
