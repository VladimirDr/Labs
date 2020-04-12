# Лабораторная работа. Настройка расширенных функций EIGRP для IPv4
 ## 	Топология
 
 ![схема](https://github.com/VladimirDr/Labs/blob/master/Lab08/8_2/Cxema8_2.png)

# 	Таблица адресации

 ![схема](https://github.com/VladimirDr/Labs/blob/master/Lab08/8_2/Tabl8_2.png)

# 	Задачи
 ## Часть 1. Создание сети и настройка основных параметров устройства
 ## Часть 2. Настройка EIGRP и проверка подключения
 ## Часть 3. Настройка EIGRP для автоматического объединения
 ## Часть 4. Настройка и распространение статического маршрута по умолчанию
 ## Часть 5. Выполнение точной настройки EIGRP
    1. Настройте параметры использования пропускной способности для EIGRP.
    2.	Настройте интервал отправки пакетов приветствия (hello) и таймер удержания для EIGRP.
  
# Часть 1:	Создание сети и настройка основных параметров устройства
    В части 1 вам предстоит настроить топологию сети и сделать базовую настройку устройств.
    
 ## Шаг 1:	Создайте сеть согласно топологии.    
    Выполнено.
 ## Шаг 2:	Настройте узлы ПК.
    Выполнено.
 ![схема](https://github.com/VladimirDr/Labs/blob/master/Lab08/8_2/PC8_2.png)
 
 ## Шаг 3:	Выполните запуск и перезагрузку маршрутизаторов.
    Выполнено.
 ## Шаг 4:	Произведите базовую настройку маршрутизаторов.
    Выполнено. В папке 8_2 приложены конфигурации маршрутизаторов.
    
# Часть 2:	Настройка EIGRP и проверка подключения
 ## Шаг 1:	Настройте EIGRP.
    a: 
      1. R1(config)#router eigrp 1
      2. R1(config-router)#network 192.168.1.0 0.0.0.255
      3. R1(config-router)#network 192.168.12.0 0.0.0.3
      4. R1(config-router)#network 192.168.13.0 0.0.0.3
    b:
         R1(config-router)#passive-interface ethernet 0/0
    c:
      1. R1(config)#interface serial 1/0
      2. R1(config-if)#bandwidth 1024
      3. R1(config)#interface serial 1/1
      4. R1(config-if)#bandwidth 64
    d:
      1. R2(config)#router eigrp 1
      2. R2(config-router)#network 192.168.2.0 0.0.0.255
      3. R2(config-router)#network 192.168.12.0 0.0.0.3
      4. R2(config-router)#network 192.168.23.0 0.0.0.3
      5. R2(config-router)#passive-interface ethernet 0/0
      6. R2(config)#interface serial 1/0
      7. R2(config-if)#bandwidth 1024
    e:
      1. R3(config)#router eigrp 1
      2. R3(config-router)#network 192.168.3.0 0.0.0.255
      3. R3(config-router)#network 192.168.13.0 0.0.0.3
      4. R3(config-router)#network 192.168.23.0 0.0.0.3
      5. R3(config-router)#passive-interface ethernet 0/0
      6. R3(config)#interface serial 1/0
      7. R3(config-if)#bandwidth 64
     
 ## Шаг 2:	Проверьте связь.
 ![схема](https://github.com/VladimirDr/Labs/blob/master/Lab08/8_2/Ping8_2(all).png)
 
    Ping между PC-A,B,C так же есть.
    
# Часть 3:	Настройка EIGRP для автоматического объединения. 
 ## Шаг 1:	Настройте EIGRP для автоматического объединения.
    a. 
      1. R1#show ip protocols
      2. Automatic Summarization: disabled
    b.
      1. R1(config)#interface loopback 1
      2. R1(config-if)#ip address 192.168.11.1 255.255.255.252
      3. R1(config)#interface loopback 5
      4. R1(config-if)#ip address 192.168.11.5 255.255.255.252
      5. R1(config)#interface loopback 9
      6. R1(config-if)#ip address 192.168.11.9 255.255.255.252
      7. R1(config)#interface loopback 13
      8. R1(config-if)#ip address 192.168.11.13 255.255.255.252
    c.
      1. R1(config)#router eigrp 1
      2. R1(config-router)#network 192.168.11.0 0.0.0.3
      3. R1(config-router)#network 192.168.11.4 0.0.0.3
      4. R1(config-router)#network 192.168.11.8 0.0.0.3
      5. R1(config-router)#network 192.168.11.12 0.0.0.3
    d.
      Как 4 маршрута полученных через EIGRP.
      1. D        192.168.11.0 [90/3139840] via 192.168.12.1, 00:28:30, Serial1/0
      2. D        192.168.11.4 [90/3139840] via 192.168.12.1, 00:28:20, Serial1/0
      3. D        192.168.11.8 [90/3139840] via 192.168.12.1, 00:28:15, Serial1/0
      4. D        192.168.11.12 [90/3139840] via 192.168.12.1, 00:28:11, Serial1/0
    e.
      1. R1(config-router)#auto-summary
      2. D        192.168.11.0 [90/3139840] via 192.168.12.1, 00:01:27, Serial1/0 вместо 4 маршрутов, как в пункте "d"
      3. Добавилось ещё 3 маршрута:
         a. так как R1 объединил интерфейсы 192.168.12.1 и 192.168.12.2 в суммарный маршрут "D        192.168.12.0/24 [90/41536000] via 192.168.23.2, 00:01:27, Serial1/1" и передал его R2.
         b. так как R1 объединил интерфейсы 192.168.13.1 и 192.168.13.2 в суммарный маршрут "D        192.168.13.0/24 [90/41024000] via 192.168.12.1, 00:01:27, Serial1/0" и передал его R2.
         c. R3 передал R2 "D        192.168.13.0/30 [90/41024000] via 192.168.23.2, 00:01:27, Serial1/1"
 
 ![схема](https://github.com/VladimirDr/Labs/blob/master/Lab08/8_2/8_2_route.png)         
 
 
         f. Выполнил команды из пунктов b,c,d,e(1) на R3.
      
# Часть 4:	Настройка и распространение статического маршрута по умолчанию.
    a. Настроил.
    b. R2(config)#ip route 0.0.0.0 0.0.0.0 loopback 1
    c. R2(config-router)#redistribute static
    e. "D*EX 0.0.0.0/0 [170/3139840] via 192.168.12.2, 00:07:46, Serial1/0" - как кандидат в default EIGRP EXTERNAL AD-170.
    
# Часть 5:	Подгонка EIGRP.
 ## Шаг 1:	Настройте параметры использования пропускной способности для EIGRP.
    a. R1(config-if)#ip bandwidth-percent eigrp 1 75
    b. R2(config-if)#ip bandwidth-percent eigrp 1 75
    c. R1(config-if)#ip bandwidth-percent eigrp 1 40
    e. R3(config-if)#ip bandwidth-percent eigrp 1 40
 ## Шаг 2:	Настройте интервал отправки пакетов приветствия (hello) и таймер удержания для EIGRP.
    a. R2#show ip eigrp interfaces detail 
       1. Укажите значение таймера приветствия по умолчанию 5 sec.
       2. Укажите значение таймера удержания по умолчанию  15 sec.
    b. 
       1. R1(config)#interface serial 1/0
       2. R1(config-if)#ip hello-interval eigrp 1 60
       3. R1(config-if)#ip hold-time eigrp 1 180
       4. R1(config)#interface serial 1/1
       5. R1(config-if)#ip hello-interval eigrp 1 60
       6. R1(config-if)#ip hold-time eigrp 1 180
    c. Выполнил команды из пункта "b" на R2,R3.
    
# 	Вопросы для повторения.
    1. В чем заключаются преимущества объединения маршрутов? Уменьшение кол-ва записей в таблице маршрутизации, меньше кол-во query-запросов. 
    
    2. Почему при настройке таймеров EIGRP необходимо настраивать значение времени удержания равным или больше интервала приветствия?       Если интервал удержания будет меньше интервала приветствия, то сосед будет считаться более недоступным.     
    
