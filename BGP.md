# BGP

Полезная [ссылка](https://linkmeup.gitbook.io/sdsm/8.-bgp-i-ip-sla/2.-bgp/2.-nastroika-bgp-i-praktika/0.-nastroika-bgp) 

## Топология
![Alt text](figs/bgp.jpeg "Топология")

### Router10
```
(config)#       interface GigabitEthernet0/0 
(config-if)#    ip address 192.168.1.2 255.255.255.0

(config)#       interface GigabitEthernet0/1
(config-if)#    ip address 193.167.1.1 255.255.255.0

(config)#           router rip
(config-router)#    version 2
(config-router)#    redistribute static 
(config-router)#    network 10.0.0.0

(config)# ip route 10.0.0.0     255.255.255.0 GigabitEthernet0/0 
(config)# ip route 193.167.1.0  255.255.255.0 GigabitEthernet0/1 
(config)# ip route 192.168.1.0  255.255.255.0 GigabitEthernet0/0 
(config)# ip route 0.0.0.0 0.0.0.0 10.0.0.1
```

### Router6
```
(config)#       interface GigabitEthernet0/0
(config-if)#    ip address 10.0.0.1 255.255.255.0

(config)#       interface GigabitEthernet0/1
(config-if)#    ip address 192.168.1.1 255.255.255.0

(config)#           router bgp 100                              // 100 - номер автономной системы
(config-router)#    neighbor 10.0.0.2 remote-as 200             // указываем соседа, и обозначаем, что это автономная система
(config-router)#    network 192.168.1.0                         // указание подключенных сетей
(config-router)#    network 193.167.1.0
(config-router)#    redistribute static                         // добавляем распространение статических маршрутов
(config-router)#    redistribute connected                      // добавляем распространение подключенных маршрутизаторов

(config)# ip route 193.167.1.0 255.255.255.0 GigabitEthernet0/1 // настройка статического маршрута
```

### Router7
```
(config)#       interface GigabitEthernet0/0
(config-if)#    ip address 10.0.0.2 255.255.255.0

(config)#       interface GigabitEthernet0/1
(config-if)#    ip address 172.10.1.1 255.255.255.0

(config)#           router bgp 200                              // 200 - номер автономной системы
(config-router)#    neighbor 10.0.0.1 remote-as 100             // указываем соседа, и обозначаем, что это автономная система
(config-router)#    network 172.10.0.0                          // указание подключенных сетей
(config-router)#    network 173.10.0.0
(config-router)#    redistribute static                         // добавляем распространение статических маршрутов
(config-router)#    redistribute connected                      // добавляем распространение подключенных маршрутизаторов

(config)# ip route 173.10.1.0 255.255.255.0 GigabitEthernet0/1  // настройка статического маршрута
```

### Router9
```
(config)#       interface GigabitEthernet0/0
(config-if)#    ip address 172.10.1.2 255.255.255.0

(config)#       interface GigabitEthernet0/1
(config-if)#    ip address 173.10.1.1 255.255.255.0

(config)#           router rip
(config-router)#    version 2
(config-router)#    redistribute static 
(config-router)#    network 10.0.0.0

(config)# ip route 172.10.1.0   255.255.255.0 GigabitEthernet0/0 100
(config)# ip route 173.10.1.0   255.255.255.0 GigabitEthernet0/1 100
(config)# ip route 10.0.0.0     255.255.255.0 GigabitEthernet0/0
(config)# ip route 0.0.0.0 0.0.0.0 10.0.0.2 
```