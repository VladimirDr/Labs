# Лабораторная работа. Поиск и устранение неполадок в работе EtherChannel.
## 	Топология.

![Схема](https://github.com/VladimirDr/Labs/blob/master/labs03/Screens/Cxema3_2.png)  

![Схема](https://github.com/VladimirDr/Labs/blob/master/labs03/Screens/Tabl3_2.png)

# Задачи.
## Часть 1. Построение сети и загрузка настроек устройств.
## Часть 2. Отладка EtherChannel.

# Часть 1:	Построение сети и загрузка настроек устройств.
## Шаг 1:	Создайте сеть согласно топологии.
   Выполнено.
## Шаг 2:	Настройте узлы ПК.
   Выполнено.
## Шаг 3:	Удалите загрузочную конфигурацию и настройки VLAN, а затем перезагрузите коммутаторы.
   Созданы новые коммутаторы.
## Шаг 4:	Загрузите конфигурации коммутаторов.
   Ниже конфиги коммутаторов:
![Схема](https://github.com/VladimirDr/Labs/blob/master/labs03/Screens/S1_3_2.png)  
![Схема](https://github.com/VladimirDr/Labs/blob/master/labs03/Screens/S2_3_2.png)
![Схема](https://github.com/VladimirDr/Labs/blob/master/labs03/Screens/S3_3_2.png)

# Часть 2: поиск и устранение неисправностей в работе EtherChannel.
## Шаг 1: Выполните поиск и устранение неполадок в работе маршрутизатора S1.
   1. Нет, агрегированные каналы 1 и 2 не отображаются как транковые.
   2. 1      Po1(SD)         LACP      Et0/0(s)    Et0/1(s) - здесь "D" говорит нам, что канал не поднят и это из-за того, что на той      стороне S3(PAgP). "s"-не совсем понял, что значит. Что они в составе агрегированного канала, но он не работает?
      
      2      Po2(SU)         PAgP      Et0/2(P)    Et0/3(P) - здесь всё хорошо. Канал поднят, протокол PAgP и физические интерфейсы        агрегированы в него. Но он не транковый.
   3. Посмотрел.
   4. "S1(config)#interface Port-channel 2" и "S1(config-if)#switchport mode trunk" сделал P02 транковым.
      
      
