Как настроить RSTP на Huawei s2320-28tp-ei-ac ?  

1)	system-view  

2)	stp mode rstp  

3)	return  
save  

4)	Проверяем:  

display stp brief  
Мы должны увидеть:  
Eth-Trunk 1 DESI FORWARDING  
Это означает, что трафик проходит.  
DESI – коммутатор лучший на данном сегменте.  
