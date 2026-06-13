Как настроить коммутатор Huawei s2320-28TP-Ei-AC ?  

0)	Логопасс по дефолту: admin  
Пароль:  
admin@huawei.com, либо Admin@huawei.com  
для прошивки V200R020 и новее пароль по умолчанию может отсутствовать.  

1)	Сброс стартовой конфиги:  
reset saved-configuration  

2)	Сброс до завода:  
reset factory-configuration  
в некоторых прошивках этой команды может не быть и она выполняется только из-под <> скобок,  
то есть не из-под system-view.  

3)	Создаём VLAN управления:  
system-view (заходим в условного root-a)  
vlan 99 (номер VLAN-a)  
name Management (имя VLAN-a. Для удобства, может совпадать с описанием…)  
quit  

interface GigabitEthernet0/0/2 (это включение самого порта. То есть, какой порт будет выделен под VLAN ?)  

port link-type access (тип порта. В нашем случае: access, то есть можно подключаться обычным компом)  
port default vlan 99 (добавляем порт в VLAN 99)  
после этих команд полезно прописать: undo shutdown, она покажет включён порт, или нет.  
quit  

interface Vlanif 99  
description Management  
ip address 192.168.99.3 255.255.255.0  
здесь, по идее, тоже полезно прописать undo shutdown.  
undo ipv6 enable  
quit  

4)	Прописываем маршрут по умолчанию:  
ip route-static 0.0.0.0 0.0.0.0 192.168.99.254  

5)	Удаляем VLAN 1:  
undo interface vlanif 1  

6)	Задаём системное имя коммутатора:  
system acsw00.perm.192.168.99.3  

7)	Создаём служебную учётку:  
aaa (это не имя учётки, это переход в режим Authentication, Authorization, Aсcounting).  
local-user admin password irreversible-cipher mysuperstrongpassword123  
local-user admin privilege level 15  
local-user admin service-type telnet ssh terminal ftp  
local-user admin ftp-directory flash: или local-user admin ftp-directory flash  
quit  
user-interface con 0  
authentication-mode aaa  
user privilege level 15  
quit  

8)	Открываем ACL-ки.  
Первой ACL-кой нужно всегда открывать ту, в которой живёт коммутатор, иначе мы потеряем доступ к нему!  
acl number 2099  
rule 10 permit source 192.168.99.0 0.0.0.255  
rule 15 permit source 192.168.10.0 0.0.0.255  
rule 20 permit source 212.33.233.40 0.0.0.7  
rule 25 permit source 109.194.154.0 0.0.0.31  
rule 30 permit source 10.6.32.0 0.0.7.255  
rule 35 permit source 10.2.4.0 0.0.0.127  
rule 100 deny  
quit  

user-interface vty 0 4  
protocol inbound all  
acl 2099 inbound  
authentication-mode aaa  
quit  

9)	Настройка временной зоны.  
system-view  

clock timezone MSK add 03:00:00  

ntp-service enable  
ntp-service unicast-server 192.168.1.1  
ntp-service unicast-server 192.168.1.2  

display ntp-service status (проверка)  

Ну, или вручную:  

clock datetime 2026-06-12 14:30:00 (установка системных даты и времени)  
clock timezone MSK add 03:00:00 (установка часового пояса), или так:  
clock timezone UTC5 add 05:00:00  

display clock (проверка текущего времени)  

10)	Выключаем http, https, включаем ssh и sftp для обнов:  
undo http server enable  
undo http secure-server enable  
telnet server enable  
stelnet server enable  
sftp server enable  

rsa local-key-pair create  
dsa local-key-pair create  

ssh authentication-type default password  
ssh user admin  
ssh user admin authentication-type password  
ssh user admin service-type all  
user-interface vty 0 4  
protocol inbound all  
quit  

11)	Логирование:  
info-center enable  
info-center source shell channel 4 log level notification  
info-center source shell channel 2 log level information  

12)	Настройка loopback-detection:  
loopback-detect packet-interval 1  

13)	Настройка LLDP:  
lldp enable  
ndp disable  
ntdp disable  
lldp message-transmission interval 30  
lldp message-transmission delay 2  
lldp message-transmission hold-multiplier 4  

14)	Включаем stp, выбираем версию rstp:  
stp enable  
stp mode rstp  

На коммутаторах, подключённых к узлу уровня агрегации, уменьшаем stp priority,  
назначая root-ом:  

stp-priority 24576  

на последнем из коммутаторов, подключённых к узлу уровня агрегации уменьшаем stp priority,  
назначенного alternative root – выставляем:  

stp priority 28672  

15)	Сохраняемся:  
return  
save  
По умолчанию, конфиг, скорее всего, будет сохранён в слот «0».  
Сохраниться можно только вот под такими скобками <>.  

__________ ПОЛЕЗНАЯ ИНФОРМАЦИЯ. __________  

16)	Для просмотра всех интерфейсов и их ip-адресов:  
display ip interface brief  

17)	Чтобы посмотреть какие порты в какой VLAN входят:  
display vlan  

18)	Чтобы увидеть полную информацию о конкретном виртуальном интерфейсе:  
display interface Vlanif 99  

19)	Отучаем коммут запрашивать подтверждения:  
mmi-mode enable  

20)	Не забываем, что данная модель коммута имеет только 4 гигабитных порта и подключаться нужно во 2-й жёлтый порт. Все синие (серые) порты по 100 mbps.  

21)	Что делать, если всё настроено, но PUTTY выдаёт ошибку:  
«Signature from servers host key invalid» и даёт подключиться только по telnet и serial?  

-	Открываем PUTTY.  
-	Слева: Connection – SSH – Host Keys – находим RSA и с помощью кнопки “UP” поднимаем RSA в самый верх.  
-	Если есть галочка «Prefer Algorithms for which a host key is known» (предпочитать алгоритмы, для которых известен ключ хоста), то её лучше СНЯТЬ.  
В версии 0.79 она есть, в более старых версиях её может не быть.  

22)	Как сбросить Хувик?  
- Выключаем питание и подаём его обратно. Пока он загружается, быстро-быстро нажимаем Ctrl+B…  
- Дальше он попросит пароль от «БИОСА».  
Чаще всего, это:  
Admin@huawei.com (с большой буквы),  
admin@huawei.com (с маленькой буквы),  
admin  
huawei (маленькими буквами).  
- Для очистки пароля – пункт #7. Clear password for console user (очистить пароль консоли).  
- Дальше – пункт #1. Boot with default mode (загрузиться в обычном режиме).  

Готово! Коммут загрузится со стартовыми настройками, но пароль для входа будет сброшен.  
Если он при входе попросит поменять пароль, или скажет, что ваш пароль не безопасен, можно  
просто нажать «N».  
