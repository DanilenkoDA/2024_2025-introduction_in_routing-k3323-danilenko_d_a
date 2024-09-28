University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)  
Course: [Introduction in routing](https://github.com/itmo-ict-faculty/introduction-in-routing)  
Year: 2024/2025  
Group: K3323  
Author: Danilenko Dmitriy Alexandrovich  
Lab: Lab1  
Date of create: 14.09.2024   
Date of finished: 28.09.2024  
 
# Лабораторная работ №1 "Установка ContainerLab и развертывание тестовой сети связи"

В лабораторной познакомились с инструментом ContainerLab и развернули первую сеть

Использовали VLAN и DHCP

## Конфиги устройств:

### RO

```
# sep/28/2024 09:45:17 by RouterOS 6.47.9
# software id = 
#
#
#
/interface ethernet
set [ find default-name=ether1 ] disable-running-check=no
set [ find default-name=ether2 ] disable-running-check=no
set [ find default-name=ether3 ] disable-running-check=no
/interface vlan
add interface=ether3 name=vlan10 vlan-id=10
add interface=ether3 name=vlan20 vlan-id=20
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/ip pool
add name=pool10 ranges=192.168.100.10-192.168.100.254
add name=pool20 ranges=192.168.200.10-192.168.200.254
/ip dhcp-server
add address-pool=pool10 disabled=no interface=vlan10 name=dhcp10
add address-pool=pool20 disabled=no interface=vlan20 name=dhcp20
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=192.168.200.1/24 interface=vlan20 network=192.168.200.0
add address=192.168.100.1/24 interface=vlan10 network=192.168.100.0
/ip dhcp-client
add disabled=no interface=ether1
/ip dhcp-server network
add address=192.168.100.0/24 gateway=192.168.100.1
add address=192.168.200.0/24 gateway=192.168.200.1
/system identity
set name=RO

```

### SW01

```
# sep/28/2024 09:47:35 by RouterOS 6.47.9
# software id = 
#
#
#
/interface bridge
add name=bridge10
add name=bridge20
/interface ethernet
set [ find default-name=ether1 ] disable-running-check=no
set [ find default-name=ether2 ] disable-running-check=no
set [ find default-name=ether3 ] disable-running-check=no
set [ find default-name=ether4 ] disable-running-check=no
set [ find default-name=ether5 ] disable-running-check=no
/interface vlan
add interface=ether3 name=vlan10 vlan-id=10
add interface=ether3 name=vlan20 vlan-id=20
add interface=ether4 name=vlan100 vlan-id=10
add interface=ether5 name=vlan200 vlan-id=20
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/interface bridge port
add bridge=bridge10 interface=vlan10
add bridge=bridge20 interface=vlan20
add bridge=bridge10 interface=vlan100
add bridge=bridge20 interface=vlan200
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
/ip dhcp-client
add disabled=no interface=ether1
add disabled=no interface=bridge10
add disabled=no interface=bridge20
/system identity
set name=SW01

```

### SW02_01

```
# sep/28/2024 09:50:12 by RouterOS 6.47.9
# software id = 
#
#
#
/interface bridge
add name=bridge10
/interface ethernet
set [ find default-name=ether1 ] disable-running-check=no
set [ find default-name=ether2 ] disable-running-check=no
set [ find default-name=ether3 ] disable-running-check=no
set [ find default-name=ether4 ] disable-running-check=no
/interface vlan
add interface=ether3 name=vlan10 vlan-id=10
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/interface bridge port
add bridge=bridge10 interface=vlan10
add bridge=bridge10 interface=ether4
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
/ip dhcp-client
add disabled=no interface=ether1
add disabled=no interface=bridge10
/system identity
set name=SW02_01

```

### SW02_02

```
# sep/28/2024 09:52:50 by RouterOS 6.47.9
# software id = 
#
#
#
/interface bridge
add name=bridge20
/interface ethernet
set [ find default-name=ether1 ] disable-running-check=no
set [ find default-name=ether2 ] disable-running-check=no
set [ find default-name=ether3 ] disable-running-check=no
set [ find default-name=ether4 ] disable-running-check=no
/interface vlan
add interface=ether3 name=vlan20 vlan-id=20
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/interface bridge port
add bridge=bridge20 interface=vlan20
add bridge=bridge20 interface=ether4
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
/ip dhcp-client
add disabled=no interface=ether1
add disabled=no interface=bridge20
/system identity
set name=SW02_02

```
## Результат пингов
![alt text](https://github.com/DanilenkoDA/2024_2025-introduction_in_routing-k3323-danilenko_d_a/blob/main/lab1/ping_results/1.png)
![alt text](https://github.com/DanilenkoDA/2024_2025-introduction_in_routing-k3323-danilenko_d_a/blob/main/lab1/ping_results/2.png)
![alt text](https://github.com/DanilenkoDA/2024_2025-introduction_in_routing-k3323-danilenko_d_a/blob/main/lab1/ping_results/3.png)
![alt text](https://github.com/DanilenkoDA/2024_2025-introduction_in_routing-k3323-danilenko_d_a/blob/main/lab1/ping_results/4.png)
![alt text](https://github.com/DanilenkoDA/2024_2025-introduction_in_routing-k3323-danilenko_d_a/blob/main/lab1/ping_results/5.png)
