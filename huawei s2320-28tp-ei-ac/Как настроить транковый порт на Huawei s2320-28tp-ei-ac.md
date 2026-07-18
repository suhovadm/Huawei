Как настроить транковый порт на Huawei s2320-28tp-ei-ac ?  

1)	system-view  
interface GigabitEthernet0/0/1  
port link-type trunk  
port trunk allow-pass vlan 10 99  
port trunk pvid vlan 99  
quit  
return  
save  

2)	VLAN 1 запрещать отдельно не нужно.  
На Huawei  VLAN 1 просто не добавляется в allow-pass, а значит не проходит.  

3)	Port pvid vlan 99  

Весь неразмеченный трафик = VLAN 99  
Если нам не нужен untagged трафик – можно вообще убрать pvid или оставить как есть.  

4)	Проверяем.  
display interface GigabitEthernet0/0/1  

Должно быть current state: up  

5)	Если сделать port trunk allow-pass vlan all  
тогда VLAN 1 тоже будет проходить, что не есть гуд!  
