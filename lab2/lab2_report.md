University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)  
Course: [Introduction in routing](https://github.com/itmo-ict-faculty/introduction-in-routing)  
Year: 2024/2025  
Group: K3323  
Author: Danilenko Dmitriy Alexandrovich  
Lab: Lab2  
Date of create: 14.10.2024   
Date of finished: 16.10.2024  
 
# Лабораторная работ №2 "Эмуляция распределенной корпоративной сети связи, настройка статической маршрутизации между филиалами"  

В лабораторной познакомились со статической маршрутизацией, развернули сеть связи на предприятии  
Схема: 
![alt text](https://github.com/DanilenkoDA/2024_2025-introduction_in_routing-k3323-danilenko_d_a/blob/main/lab2/schema.png)  

## Конфиги устройств:  

### RO01.BRL  

```
/interface ethernet
set [ find default-name=ether1 ] disable-running-check=no
set [ find default-name=ether2 ] disable-running-check=no
set [ find default-name=ether3 ] disable-running-check=no
set [ find default-name=ether4 ] disable-running-check=no
set [ find default-name=ether5 ] disable-running-check=no
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/ip pool
add name=BRL ranges=10.10.10.2-10.10.10.254
/ip dhcp-server
add address-pool=BRL disabled=no interface=ether3 name=BRL_dhcp
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=10.10.10.1/24 interface=ether3 network=10.10.10.0
add address=192.168.1.1/30 interface=ether4 network=192.168.1.0
add address=192.168.3.2/30 interface=ether5 network=192.168.3.0
/ip dhcp-client
add disabled=no interface=ether1
/ip route
add distance=1 dst-address=10.10.20.0/24 gateway=192.168.1.2
add distance=1 dst-address=10.10.30.0/24 gateway=192.168.3.1
/system identity
set name=RO01.BRL


```

### RO01.FRT  

```
# oct/15/2024 21:56:32 by RouterOS 6.47.9
# software id = 
#
#
#
/interface ethernet
set [ find default-name=ether1 ] disable-running-check=no
set [ find default-name=ether2 ] disable-running-check=no
set [ find default-name=ether3 ] disable-running-check=no
set [ find default-name=ether4 ] disable-running-check=no
set [ find default-name=ether5 ] disable-running-check=no
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/ip pool
add name=FRT ranges=10.10.20.2-10.10.20.254
/ip dhcp-server
add address-pool=FRT disabled=no interface=ether4 name=FRT_dhcp
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=192.168.1.2/30 interface=ether3 network=192.168.1.0
add address=10.10.20.1/24 interface=ether4 network=10.10.20.0
add address=192.168.2.1/30 interface=ether5 network=192.168.2.0
/ip dhcp-client
add disabled=no interface=ether1
/ip route
add distance=1 dst-address=10.10.10.0/24 gateway=192.168.1.1
add distance=1 dst-address=10.10.30.0/24 gateway=192.168.2.2
/system identity
set name=RO01.FRT
```

### RO01.MSK  

```
# oct/15/2024 21:57:06 by RouterOS 6.47.9
# software id = 
#
#
#
/interface ethernet
set [ find default-name=ether1 ] disable-running-check=no
set [ find default-name=ether2 ] disable-running-check=no
set [ find default-name=ether3 ] disable-running-check=no
set [ find default-name=ether4 ] disable-running-check=no
set [ find default-name=ether5 ] disable-running-check=no
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/ip pool
add name=MSK ranges=10.10.30.2-10.10.30.254
/ip dhcp-server
add address-pool=MSK disabled=no interface=ether5 name=MSK_dhcp
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=192.168.3.1/30 interface=ether3 network=192.168.3.0
add address=192.168.2.2/30 interface=ether4 network=192.168.2.0
add address=10.10.30.1/24 interface=ether5 network=10.10.30.0
/ip dhcp-client
add disabled=no interface=ether1
/ip route
add distance=1 dst-address=10.10.10.0/24 gateway=192.168.3.2
add distance=1 dst-address=10.10.20.0/24 gateway=192.168.2.1
/system identity
set name=RO01.MSK


```


## Результат пингов
![alt text](https://github.com/DanilenkoDA/2024_2025-introduction_in_routing-k3323-danilenko_d_a/blob/main/lab2/ping_results/1.png)
![alt text](https://github.com/DanilenkoDA/2024_2025-introduction_in_routing-k3323-danilenko_d_a/blob/main/lab2/ping_results/2.png)
![alt text](https://github.com/DanilenkoDA/2024_2025-introduction_in_routing-k3323-danilenko_d_a/blob/main/lab2/ping_results/3.png)


