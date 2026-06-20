Как настроить RSTP на Huawei s2320-28TP-Ei-AC ?  

1)	system-view  

2)	stp mode rstp  
 
3)
return  
save  

5)	Проверяем:  

display stp brief  
Мы должны увидеть:  
Eth-Trunk 1 DESI FORWARDING  
Это означает, что трафик проходит.  
DESI – коммутатор лучший на данном сегменте.  
