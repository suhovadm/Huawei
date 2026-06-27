Как настроить LACP на Huawei s2320-28TP-Ei-AC ?  

1)	Если надо посмотреть прошивку коммута для гугления точного синтаксиса команд:  
display version  
ищем строки вида:  
software | version | vrp ® software, version 5.xxx (V200…)  

2)	Переходим в режим настройки:  
system-view  

3)	Создаём LACP (Eth-Trunk)  

interface Eth-Trunk 1 (E – с большой буквы, T – с большой буквы)  
mode lacp  
quit  

4)	Добавляем порты в Trunk.  

interface GigabitEthernet0/0/3 (G – с большой буквы, E – с большой буквы, 0/0/3 – слитно)  
Eth-Trunk 1  
quit  

interface GigabitEthernet0/0/4 (G – с большой буквы, E – с большой буквы, 0/0/4 – слитно)  
Eth-Trunk 1  
quit  

5)	Если порты были настроены ранее и ругается, нужно очистить их.  
system-view  

interface GigabitEthernet0/0/3  
undo shutdown  
clear configuration this  

interface GigabitEthernet0/0/4  
undo shutdown  
clear configuration this  

и снова добавляем их в Trunk:  
Eth-Trunk 1 для 0/0/3  
Eth-Trunk 1 для 0/0/4  

6)	Сохраняем конфигу:  
return  
save  

7)	Проверяем, что LACP работает.  
display Eth-Trunk 1  
и смотрим…  
Operation Status: UP  
Number of up ports: 2  
Working mode: LACP  

Если у нас никакие провода не подключены, или на одной из сторон LACP пока не настроен,  
то в строчках будет вот так:  
Operation Status: DOWN  
Number of up ports: 0  
Working mode: LACP  
 
8)	Также можно проверить порты по отдельности:  
display interface GigabitEthernet0/0/3  
display interface GigabitEthernet0/0/4  

Смотрим самую первую строчку, в ней должно быть вот так:  
LINE PROTOCOL CURRENT STATE: UP  

9)	Теперь пропишем, что можно передавать в LACP.  
system-view  
interface Eth-Trunk 1  
port link-type trunk  
port trunk allow-pass vlan 10 99  
quit  

Говорим, что через LACP можно передавать 10 и 99 VLAN, клиенты (10) и управление (99).  
