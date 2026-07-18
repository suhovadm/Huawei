Как выкинуть все из VLAN 1 в VLAN 10 на Huawei s2320-28tp-ei-ac ?  


1)	system-view  
2)	vlan 10  
3)	quit  

4)	interface gigabitEthernet0/0/5  
5)	port link-type access  
6)	port default vlan 10  
7)	quit  

8)	interface gigabitEthernet0/0/6  
9)	port link-type access  
10)	port default vlan 10  
11)	quit  

12)	interface gigabitEthernet0/0/7  
13)	port link-type access  
14)	port default vlan 10  
15)	quit  

16)	interface gigabitEthernet0/0/8  
17)	port link-type access  
18)	port default vlan 10  
19)	quit  

20)	Проверяем!  
display vlan  

vlan 1 (vid 1) должен быть пуст!  

Не забываем сохраниться:  
return  
save –y  
