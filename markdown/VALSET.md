Time-stamp: <2017-01-03 03:28:28 hamada>

# /VALSET


------------------------------
## ��

/VALSET *vname*, *hex_value*,  *nbit_lsb*, *nbit_msb*;



------------------------------
## ����

- *vname*: �ǡ����ե��������ѿ�̾
- *hex_value*: ���(C����16�ʥ�ƥ��)
- *nbit_lsb*: VHDL signal�Ǥ�(nbit_msb downto <b>nbit_lsb</b>)��nbit_lsb
- *nbit_msb*: VHDL signal�Ǥ�(<b>nbit_msb</b> downto nbit_lsb)��nbit_msb


------------------------------
## ������

```
/VALSET mmax ,0x9700, 0, 16;
```

------------------------------
## ����

/VALSET�ϥǡ����ե�������ǥӥå����������ꤹ�뤿��Τ�ΤǤ���.
/VASET��HDL�����̤����ץ�����Ѥˤ����Τ�, 
�̾��/CONST_FLOAT�⤷���� /CONST_LOG����Ѥ���л�­���.

�������mmax��, HDL��Ǥ�,
```
  constant mmax : std_logic_vector(16 downto 0) := "01001011100000000"; -- 0x9700
```
�Ȥ������������Ѵ������.


------------------------------
## ����

------------------------------
## Change Log

- 2004.08.16 : the first edition
