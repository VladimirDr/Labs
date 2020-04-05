# Лабораторная работа. Базовая настройка протокола EIGRP для IPv4
## 	Топология
![схема](https://github.com/VladimirDr/Labs/blob/master/Lab08/Cxema8_1.png)
## 	Таблица адресации
![схема](https://github.com/VladimirDr/Labs/blob/master/Lab08/Tabl8_1.png)
# Задачи
  ## Часть 1. Построение сети и проверка соединения
  ## Часть 2. Настройка маршрутизации EIGRP
  ## Часть 3. Проверка маршрутизации EIGRP
  ## Часть 4. Настройка пропускной способности и пассивных интерфейсов

# Часть 1:	Построение сети и проверка связи  
  
  ## Шаг 1:	Создайте сеть согласно топологии.
  Выполнено, схема в начале лабораторной работы.
  
  ## Шаг 2:	Настройте узлы ПК.
  Выполнено.
  ![схема](https://github.com/VladimirDr/Labs/blob/master/Lab08/PC8_1.png)
  
  ## Шаг 3:	Выполните запуск и перезагрузку маршрутизаторов.
  Выполнено.
  
  ## Шаг 4:	Произведите базовую настройку маршрутизаторов.
   1. "R*(config)#no ip domain-lookup" для всех 3-х устройств;
   2. "R*(config)#hostname R*" для всех 3-х устройств;
   3. "R*(config)#service password-encryption" для всех 3-х устройств;
   4. "R*(config)#banner motd # This is a secure system. Authorized Access Only! #" для всех 3-х устройств;
   5. "R*(config)#enable password class" для всех 3-х устройств;
   6. "R*(config)#line console 0", "R*(config-line)#password cisco", "R*(config-line)#login" для всех 3-х устройств,
      "R*(config)#line vty 0 4" и две аналогичные команды из строчки выше для всех 3-х устройств;
   7. "logging synchronous" изначально присутствует в конфиге для line con 0;
   8. Настроил IP-адреса для маршрутизаторов в соответствии с таблицей адресации. 
   
  ## Шаг 5:	Проверьте подключение.
  ![схема](https://github.com/VladimirDr/Labs/blob/master/Lab08/Ping8_1.png)
  
# Часть 2:	Настройка маршрутизации EIGRP
  ## Шаг 1:	Включите маршрутизацию EIGRP на маршрутизаторе R1. Используйте номер автономной системы 10.
     R1(config)# router eigrp 10
     
  ## Шаг 2:	Объявите напрямую подключенные сети на маршрутизаторе R1, используя шаблонную маску.
     R1(config-router)# network 10.1.1.0 0.0.0.3
     R1(config-router)# network 192.168.1.0 0.0.0.255
     R1(config-router)# network 10.3.3.0 0.0.0.3
     
  Комментарии:
  
  
  ## Шаг 3:	Включите маршрутизацию EIGRP и объявите напрямую подключенные сети на маршрутизаторах R2 и R3.
     "R*(config)# router eigrp 10" для 2 и 3 маршрутизатора.
     "R2(config-router)# network 10.1.1.0 0.0.0.3"
     "R2(config-router)# network 192.168.2.0 0.0.0.255"
     "R2(config-router)# network 10.2.2.0 0.0.0.3"
     "R3(config-router)# network 10.3.3.0 0.0.0.3"
     "R3(config-router)# network 192.168.3.0 0.0.0.255"
     "R3(config-router)# network 10.2.2.0 0.0.0.3"
     
     
