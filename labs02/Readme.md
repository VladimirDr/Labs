# Лабораторная работа. Развертывание коммутируемой сети с резервными каналами.

## Задание:
  
   1. Создание сети и настройка основных параметров устройства;
   2. Выбор корневого моста;
   3. Наблюдение за процессом выбора протоколом STP порта, исходя из стоимости портов;
   4. Наблюдение за процессом выбора протоколом STP порта, исходя из приоритета портов;

# Часть 1. Создание сети и настройка основных параметров устройств.

## Шаг 1:	Создайте сеть согласно топологии.  

![Схема](https://github.com/VladimirDr/Labs/blob/master/labs02/%D0%A1%D1%85%D0%B5%D0%BC%D0%B0.png)

## Шаг 2: Выполните инициализацию и перезагрузку коммутаторов.

   Под инициализацией видимо понимается их запуск на стенде. А перезагрузку сделал в следующем шаге, после обновления конфига.

## Шаг 3:	Настройте базовые параметры каждого коммутатор.
   
   1. "S*(config)#no ip domain-lookup" для всех 3-х устройств;
   2. "S*(config)#hostname S1" для всех 3-х устройств;
   3. "S*(config)#enable secret class" для всех 3-х устройств;
   4. "S*(config)#line console 0", "S1(config-line)#password cisco", "S1(config-line)#login" для всех 3-х устройств, 
      "S*(config)#line vty 0 4" и две аналогичные команды из строчки выше для всех 3-х устройств;
   5. "S*(config)#line console 0" и "S1(config-line)#logging synchronous" для всех 3-х устройств;
   6. "S*(config)#banner motd # ... #" настроил банерное сообщение;
   7. "S*(config)#interface vlan 1", "S*(config-if)#ip address 192.168.1.* 255.255.255.0", "S*(config-if)#no shutdown" для всех 3-х     устройств,
   8. "S*#copy running-config startup-config" для всех 3-х устройств.
   
## Шаг 4: Проверьте связь.

   1. Успешно ли выполняется эхо-запрос от коммутатора S1 на коммутатор S2?  ДА.
   2. Успешно ли выполняется эхо-запрос от коммутатора S1 на коммутатор S3?  ДА.
   3. Успешно ли выполняется эхо-запрос от коммутатора S2 на коммутатор S3?  ДА.
   
# Часть 2: Определение корневого моста.

## Шаг 1:	Отключите все порты на коммутаторах

   "S*(config)#interface range ethernet 0/0-3" и "S*(config-if-range)#shutdown" для всех 3-х устройств.
   
## Шаг 2:	Настройте подключенные порты в качестве транковых.

   "S*(config-if-range)#switchport mode trunk" для всех 3-х устройств.
   
## Шаг 3:	Включите порты F0/2 и F0/4 на всех коммутаторах.

   По очереди "S*(config)#interface ethernet 0/1,3" и "S*(config-if)#no shutdown" для всех 3-х устройств.
   
## Шаг 4:	Отобразите данные протокола spanning-tree.
 
## S1#show spanning-tree

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     aabb.cc00.1000
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.1000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/1               Desg FWD 100       128.2    Shr
Et0/3               Desg FWD 100       128.4    Shr

## S2#show spanning-tree

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     aabb.cc00.1000
             Cost        100
             Port        2 (Ethernet0/1)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.2000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/1               Root FWD 100       128.2    Shr
Et0/3               Desg FWD 100       128.4    Shr

## S3#show spanning-tree

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     aabb.cc00.1000
             Cost        100
             Port        4 (Ethernet0/3)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.3000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/1               Altn BLK 100       128.2    Shr
Et0/3               Root FWD 100       128.4    Shr

## В схему ниже запишите роль и состояние (Sts) активных портов на каждом коммутаторе в топологии.
   
![схема с описанием](https://github.com/VladimirDr/Labs/blob/master/labs02/%D0%A1%D1%85%D0%B5%D0%BC%D0%B0%20%D1%81%20%D0%BE%D0%BF%D0%B8%D1%81%D0%B0%D0%BD%D0%B8%D0%B5%D0%BC.png) 
 
## С учетом выходных данных, поступающих с коммутаторов, ответьте на следующие вопросы.
1. Какой коммутатор является корневым мостом? S1
2. Почему этот коммутатор был выбран протоколом spanning-tree в качестве корневого моста?
        При равном приоритете 32769 у него наименьший MAC-адрес aabb.cc00.1000
3. Какие порты на коммутаторе являются корневыми портами? Порт с наименьшей стоимостью для достижения корневого моста
4. Какие порты на коммутаторе являются назначенными портами? Порт с наименьшей стоимостью для возврата к корневому мосту
5. Какой порт отображается в качестве альтернативного и в настоящее время заблокирован? Et0/1 на S3
6. Почему протокол spanning-tree выбрал этот порт в качестве невыделенного (заблокированного) порта? Так как на другой стороне назначенный порт, а не корневой.

# Часть 3:	Наблюдение за процессом выбора протоколом STP порта, исходя из стоимости портов

## Шаг 1:	Определите коммутатор с заблокированным портом.
    
## S3#show spanning-tree   Это Et0/1

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     aabb.cc00.1000
             Cost        100
             Port        4 (Ethernet0/3)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.3000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/1               Altn BLK 100       128.2    Shr
Et0/3               Root FWD 100       128.4    Shr

## Шаг 2:	Измените стоимость порта.

   "S3(config)#interface ethernet 0/3" и "S3(config-if)#spanning-tree cost 18"
   
## Шаг 3:	Просмотрите изменения протокола spanning-tree.
 
## S3#show spanning-tree

   VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     aabb.cc00.1000
             Cost        18
             Port        4 (Ethernet0/3)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.3000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  15  sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/1               Desg LRN 100       128.2    Shr
Et0/3               Root FWD 18        128.4    Shr

## S2#show spanning-tree

   VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     aabb.cc00.1000
             Cost        100
             Port        2 (Ethernet0/1)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.2000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  15  sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/1               Root FWD 100       128.2    Shr
Et0/3               Altn BLK 100       128.4    Shr

Почему протокол spanning-tree заменяет ранее заблокированный порт на назначенный порт и блокирует порт, который был назначенным портом на другом коммутаторе? Так как меняется стоимость корневого пути.

## Шаг 4:	Удалите изменения стоимости порта.

   "S3(config)#interface ethernet 0/3" и "S3(config-if)#no spanning-tree cost 18"
   
## S3#show spanning-tree

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     aabb.cc00.1000
             Cost        100
             Port        4 (Ethernet0/3)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.3000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  15  sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/1               Altn BLK 100       128.2    Shr
Et0/3               Root FWD 100       128.4    Shr

## S2#show spanning-tree

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     aabb.cc00.1000
             Cost        100
             Port        2 (Ethernet0/1)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.2000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  15  sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/1               Root FWD 100       128.2    Shr
Et0/3               Desg FWD 100       128.4    Shr

# Часть 4:	Наблюдение за процессом выбора протоколом STP порта, исходя из приоритета портов.

   a. "S*(config)#interface ethernet 0/* " и "S1(config-if)#no shutdown" для портов 0/0 и 0/2 на всех 3-х устройствах.
   b. 
##   S2#show spanning-tree

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     aabb.cc00.1000
             Cost        100
             Port        1 (Ethernet0/0)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.2000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/0               Root FWD 100       128.1    Shr
Et0/1               Altn BLK 100       128.2    Shr
Et0/2               Desg FWD 100       128.3    Shr
Et0/3               Desg FWD 100       128.4    Shr

## S3#show spanning-tree

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     aabb.cc00.1000
             Cost        100
             Port        3 (Ethernet0/2)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.3000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/0               Altn BLK 100       128.1    Shr
Et0/1               Altn BLK 100       128.2    Shr
Et0/2               Root FWD 100       128.3    Shr
Et0/3               Altn BLK 100       128.4    Shr

Какой порт выбран протоколом STP в качестве порта корневого моста на каждом коммутаторе некорневого моста? S2 Et0/0, S3 Et0/2
Почему протокол STP выбрал эти порты в качестве портов корневого моста на этих коммутаторах? При всех равных условиях(стоимость порта и BID) в расчёт берётся наименьший номер порта.

## 	Вопросы для повторения

1.	Какое значение протокол STP использует первым после выбора корневого моста, чтобы определить выбор порта? Стоимость порта.
2.	Если первое значение на двух портах одинаково, какое следующее значение будет использовать протокол STP при выборе порта? Номер порта.
3.	Если оба значения на двух портах равны, каким будет следующее значение, которое использует протокол STP при выборе порта? Нет такого.


![Конфиг S1](https://github.com/VladimirDr/Labs/blob/master/labs02/ConfigS1)
![Конфиг S2](https://github.com/VladimirDr/Labs/blob/master/labs02/ConfigS2)
![Конфиг S3](https://github.com/VladimirDr/Labs/blob/master/labs02/ConfigS3)
