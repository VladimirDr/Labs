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
    
