Как проверить, что порты административно выключены на Huawei s2320-28tp-ei-ac ?  

1)	Смотрим:  
display interface brief  
Всё, что будет выглядеть вот так: *down - значит принудительно выключено администратором.  

2)	Чтобы их поднять – делаем следующее:  

system-view  

interface GigabitEthernet0/0/1  
undo shutdown  

interface GigabitEthernet0/0/2  
undo shutdown  

return  
save –y  

Команда «undo shutdown» говорит «отменить выключение», т.е. «включить обратно».  
