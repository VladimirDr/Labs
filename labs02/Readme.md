# Лабораторная работа. Развертывание коммутируемой сети с резервными каналами.

## Задание:
  
   1. Создание сети и настройка основных параметров устройства;
   2. Выбор корневого моста;
   3. Наблюдение за процессом выбора протоколом STP порта, исходя из стоимости портов;
   4. Наблюдение за процессом выбора протоколом STP порта, исходя из приоритета портов;

# Часть 1. Создание сети и настройка основных параметров устройств.

## Шаг 1:	Создайте сеть согласно топологии.  

## Шаг 2: Выполните инициализацию и перезагрузку коммутаторов.

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
 
## S1show spanning-tree

1.VLAN0001
2.  Spanning tree enabled protocol ieee
3.  Root ID    Priority    32769
4.             Address     aabb.cc00.1000
5.             This bridge is the root
6.             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

7.  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
8.             Address     aabb.cc00.1000
9.             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
8.             Aging Time  300 sec

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


