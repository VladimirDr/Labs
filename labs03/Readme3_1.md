# Лабораторная работа. Настройка EtherChannel.

## Топология

Схема

Таблица адресации

# Часть 1:	Настройка основных параметров коммутатора.

## Шаг 1:	Создайте сеть согласно топологии.
 Смотри схему в пункте "Топология".

## Шаг 2:	Выполните инициализацию и перезагрузку коммутаторов.
 Запуск заново созданных коммутаторов.
 
## Шаг 3:	Настройте базовые параметры каждого коммутатора. 
 1. "S*(config)#no ip domain-lookup" для всех 3-х устройств;
 2. "S*(config)#hostname S*" для всех 3-х устройств;
 3. "S*(config)#service password-encryption" для всех 3-х устройств;
 4. "S*(config)#banner motd # This is a secure system. Authorized Access Only! #" для всех 3-х устройств;
 5. "S*(config)#enable password class" для всех 3-х устройств;
 6. "S*(config)#line console 0", "S*(config-line)#password cisco", "S*(config-line)#login" для всех 3-х устройств,
    "S*(config)#line vty 0 4" и две аналогичные команды из строчки выше для всех 3-х устройств;
 7. "logging synchronous" изначально присутствует в конфиге для line con 0;   
 8. "S*(config)#interface range ethernet 0/0-3 и 1/0-3" и "S*(config-if-range)#shutdown" для всех 3-х устройств;
 9. "S*(config)#vlan 99" и "S*(config-vlan)#name Managment" для всех 3-х устройств; 
10. "S*(config)#vlan 10" и "S*(config-vlan)#name Staff" для всех 3-х устройств;
11. "S*(config)#interface ethernet 1/0" и "S*(config-if)#switchport access vlan 10" для всех 3-х устройств;
12. "S*(config)#interface vlan 99" и "S*(config-if)#ip address 192.168.99.1,2,3 255.255.255.0" и "S*(config-if)#no shutdown" для всех 3-х устройств;
13. "S*#copy running-config startup-config" для всех 3-х устройств;

## Шаг 4:	Настройте компьютеры.

   VPCS> ip 192.168.10.1,2,3/24 для всех 3-х устройств.
   
# Часть 2:	Настройка протокола PAgP.

## Шаг 1:	Настройте PAgP на S1 и S3.
 
 ![Конфиг](https://github.com/VladimirDr/Labs/blob/master/labs03/Screens/ConfigS1.png)

 
