<!-- title: �ĸ�������bb�༭�� -->
<!-- tag: BBK -->
<!-- date: 2011-04-24 01:34:00 -->
<!-- state: published -->

![LibͼƬѡ��](http://upload.eebbk.net/UploadFile/2011-4/201142322592923264.gif)
![�˵�ʽѡ��](http://upload.eebbk.net/UploadFile/2011-4/2011424048880623.jpg)
![ͼƬ��������](http://upload.eebbk.net/UploadFile/2011-4/20114232352943219.gif)

ԭ��**[����](http://club.eebbk.com/bbkbbs/showtopic/257169/1)**

<!-- more -->

```
'//=========ShowPic_Alpha==========//
'//=========��Alpha��ʾͼƬ==========//
'���� Wener
'��̳Id a3160586 (club.eebbk.com   �����)
'QQ 514403150

˵��:
showpic_alpha( srcPage, srcPic, alPic, DisX, DisY, Wid, Hgt, srcX, srcY, showMODE)
���showpic������ֻ����һ��alPic���� ��������һ����
�����alpha��ͨ��ͼ ����bb����ʾ��ɫ��Ŀ���ޣ������ڵ����Ͽ�����һ��ƽ����

'//=========LibSel==========//
'//=========LibͼƬѡ����==========//
'���� Wener
'��̳Id a3160586 (club.eebbk.com   �����)
'QQ 514403150

˵���������������ѡ��Lib�е�ĳһ��ͼƬ��Ȼ�󷵻�ͼƬ���
����������бȽϺõ�UI���������о����ǱȽϺõ�
�������Ҫ������getLibCount �� getInput ����������
LibSel( Lib_Name$)
�������ֻ��ҪLIB�ļ����������



'//=========Selector==========//
'//=========�˵�ʽѡ��==========//
'���� Wener
'��̳Id a3160586 (club.eebbk.com   �����)
'QQ 514403150

˵����������������ֻ��Ĳ˵�������������ѡ���
Selector_Str$���Ϊһ�����飬����ǰ��Ϊ�丳ֵ�ַ���
Selector_StrCount ���ܵ�ѡ�����
���� Selector( MustChoiceOne)
MustChoiceOne�������Ƿ����ѡ��һ�� 1 Ϊ�� Ҳ������˵�����˳�
MustChoiceOneΪ0��ʱ��  �˳�����1 ȷ�Ϸ���ѡ����Selector_Str�е�λ��
Ҳ����Selector_Str$( i)
�����뿴 eg.Selector.bas

'//=========PageAdjust==========//
'//=========ҳ���������==========//
'���� Wener
'��̳Id a3160586 (club.eebbk.com   �����)
'QQ 514403150

˵���������������photoshop��һЩ������
ƽ����ɫ������rbgͨ��������������ʹҳ��ڰ׻�
�ɾ�����ת180��ͼ��ȣ���������ʹ�õ�ʱ���ְ�
����������ɵ�bin�ļ��ϴ�300��k����Ϊ�����˴󲿷ֿհ�


����˵����
����PageAdjust�⣬����������������ҪSYS_Fe$ ���ȫ�ֱ���
����о������Ӧ�ö�Ҫ�е�һ�����������ԾͶ�������ȫ��
IF GetEnv!() = Env_SIM Then
	SYS_Fe$ = ".rlb"
Else
	SYS_Fe$ = ".lib"
End IF
Ҳ�����ж�������ʲô��ʽ��
```