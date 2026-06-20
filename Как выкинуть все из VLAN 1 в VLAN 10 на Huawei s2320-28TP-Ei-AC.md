Как выкинуть все из VLAN 1 в VLAN 10 на Huawei s2320-28TP-Ei-AC ?  


1)	system-view  
2)	vlan 10  
3)	quit
   
5)	interface gigabitEthernet0/0/5  
6)	port link-type access  
7)	port default vlan 10  
8)	quit
   
10)	interface gigabitEthernet0/0/6  
11)	port link-type access  
12)	port default vlan 10  
13)	quit
    
15)	interface gigabitEthernet0/0/7  
16)	port link-type access  
17)	port default vlan 10  
18)	quit
    
20)	interface gigabitEthernet0/0/8  
21)	port link-type access  
22)	port default vlan 10  
23)	quit
    
25)	Проверяем!  
display vlan  

vlan 1 (vid 1) должен быть пуст!  

Не забываем сохраниться:  
return  
save –y  
