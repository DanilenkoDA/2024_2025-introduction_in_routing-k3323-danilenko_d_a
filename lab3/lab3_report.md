University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)  
Course: [Introduction in routing](https://github.com/itmo-ict-faculty/introduction-in-routing)  
Year: 2024/2025  
Group: K3323  
Author: Danilenko Dmitriy Alexandrovich  
Lab: Lab3  
Date of create: 9.12.2024  
Date of finished:  

# Лабораторная работа №3 "Эмуляция распределенной корпоративной сети связи, настройка OSPF и MPLS, организация первого EoMPLS"

В лабораторной изучили протоколы OSPF и MPLS
Схема:
![alt text](https://github.com/DanilenkoDA/2024_2025-introduction_in_routing-k3323-danilenko_d_a/blob/main/lab3/schema.png)

## Конфиги устройств:

### R01.SPB  

```
# dec/08/2024 22:31:08 by RouterOS 6.47.9
# software id = 
#
#
#
/interface bridge
add name=lobridge
add name=vpn
/interface ethernet
set [ find default-name=ether1 ] disable-running-check=no
set [ find default-name=ether2 ] disable-running-check=no
set [ find default-name=ether3 ] disable-running-check=no
set [ find default-name=ether4 ] disable-running-check=no
set [ find default-name=ether5 ] disable-running-check=no
/interface vpls
add disabled=no l2mtu=1500 mac-address=02:4D:20:A8:0C:52 name=EoMPLS remote-peer=10.10.10.4 vpls-id=150:150
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing ospf instance
set [ find default=yes ] router-id=10.10.10.1
/interface bridge port
add bridge=vpn interface=ether3
add bridge=vpn interface=EoMPLS
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=10.10.10.1 interface=lobridge network=10.10.10.1
add address=172.16.0.1/24 interface=vpn network=172.16.0.0
add address=10.10.7.1/30 interface=ether3 network=10.10.7.0
add address=10.10.0.1/30 interface=ether4 network=10.10.0.0
add address=10.10.5.2/30 interface=ether5 network=10.10.5.0
/ip dhcp-client
add disabled=no interface=ether1
/mpls ldp
set enabled=yes lsr-id=10.10.10.1 transport-address=10.10.10.1
/mpls ldp interface
add interface=ether3
add interface=ether4
add interface=ether5
/routing ospf network
add area=backbone network=10.10.10.1/32
add area=backbone network=10.10.7.0/30
add area=backbone network=10.10.0.0/30
add area=backbone network=10.10.5.0/30
/system identity
set name=R01.SPB

```

### R01.MSK
```
/interface bridge
add name=lobridge
/interface ethernet
set [ find default-name=ether1 ] disable-running-check=no
set [ find default-name=ether2 ] disable-running-check=no
set [ find default-name=ether3 ] disable-running-check=no
set [ find default-name=ether4 ] disable-running-check=no
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing ospf instance
set [ find default=yes ] router-id=10.10.10.2
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=10.10.10.2 interface=lobridge network=10.10.10.2
add address=10.10.0.2/30 interface=ether3 network=10.10.0.0
add address=10.10.1.1/30 interface=ether4 network=10.10.1.0
/ip dhcp-client
add disabled=no interface=ether1
/mpls ldp
set enabled=yes lsr-id=10.10.10.2 transport-address=10.10.10.2
/mpls ldp interface
add interface=ether3
add interface=ether4
/routing ospf network
add area=backbone network=10.10.10.2/32
add area=backbone network=10.10.0.0/30
add area=backbone network=10.10.1.0/30
/system identity
set name=R01.MSC
```

### R01.LBN
```
/interface bridge
add name=lobridge
/interface ethernet
set [ find default-name=ether1 ] disable-running-check=no
set [ find default-name=ether2 ] disable-running-check=no
set [ find default-name=ether3 ] disable-running-check=no
set [ find default-name=ether4 ] disable-running-check=no
set [ find default-name=ether5 ] disable-running-check=no
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing ospf instance
set [ find default=yes ] router-id=10.10.10.3
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=10.10.10.3 interface=lobridge network=10.10.10.3
add address=10.10.1.2/30 interface=ether3 network=10.10.1.0
add address=10.10.2.1/30 interface=ether4 network=10.10.2.0
add address=10.10.6.2/30 interface=ether5 network=10.10.6.0
/ip dhcp-client
add disabled=no interface=ether1
/mpls ldp
set enabled=yes lsr-id=10.10.10.3 transport-address=10.10.10.3
/mpls ldp interface
add interface=ether3
add interface=ether4
add interface=ether5
/routing ospf network
add area=backbone network=10.10.10.3/32
add area=backbone network=10.10.1.0/30
add area=backbone network=10.10.2.0/30
add area=backbone network=10.10.6.0/30
/system identity
set name=R01.LBN
```

### R01.NY
```
/interface bridge
add name=lobridge
add name=vpn
/interface ethernet
set [ find default-name=ether1 ] disable-running-check=no
set [ find default-name=ether2 ] disable-running-check=no
set [ find default-name=ether3 ] disable-running-check=no
set [ find default-name=ether4 ] disable-running-check=no
set [ find default-name=ether5 ] disable-running-check=no
/interface vpls
add disabled=no l2mtu=1500 mac-address=02:DE:9F:D0:10:DC name=EoMPLS remote-peer=10.10.10.1 vpls-id=150:150
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing ospf instance
set [ find default=yes ] router-id=10.10.10.4
/interface bridge port
add bridge=vpn interface=ether4
add bridge=vpn interface=EoMPLS
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=10.10.10.4 interface=lobridge network=10.10.10.4
add address=10.10.2.2/30 interface=ether3 network=10.10.2.0
add address=10.10.8.1/30 interface=ether4 network=10.10.8.0
add address=172.16.0.2/24 interface=vpn network=172.16.0.0
add address=10.10.3.1/30 interface=ether5 network=10.10.3.0
/ip dhcp-client
add disabled=no interface=ether1
/mpls ldp
set enabled=yes lsr-id=10.10.10.4 transport-address=10.10.10.4
/mpls ldp interface
add interface=ether3
add interface=ether4
add interface=ether5
/routing ospf network
add area=backbone network=10.10.10.4/32
add area=backbone network=10.10.3.0/30
add area=backbone network=10.10.2.0/30
/system identity
set name=R01.NY
```

### R01.LND
```
/interface bridge
add name=lobridge
/interface ethernet
set [ find default-name=ether1 ] disable-running-check=no
set [ find default-name=ether2 ] disable-running-check=no
set [ find default-name=ether3 ] disable-running-check=no
set [ find default-name=ether4 ] disable-running-check=no
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing ospf instance
set [ find default=yes ] router-id=10.10.10.5
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=10.10.10.5 interface=lobridge network=10.10.10.5
add address=10.10.4.1/30 interface=ether3 network=10.10.4.0
add address=10.10.3.2/30 interface=ether4 network=10.10.3.0
/ip dhcp-client
add disabled=no interface=ether1
/mpls ldp
set enabled=yes lsr-id=10.10.10.5 transport-address=10.10.10.5
/mpls ldp interface
add interface=ether3
add interface=ether4
/routing ospf network
add area=backbone network=10.10.10.5/32
add area=backbone network=10.10.4.0/30
add area=backbone network=10.10.3.0/30
/system identity
set name=R01.LND
```

### R01.HKI
```
/interface bridge
add name=lobridge
/interface ethernet
set [ find default-name=ether1 ] disable-running-check=no
set [ find default-name=ether2 ] disable-running-check=no
set [ find default-name=ether3 ] disable-running-check=no
set [ find default-name=ether4 ] disable-running-check=no
set [ find default-name=ether5 ] disable-running-check=no
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing ospf instance
set [ find default=yes ] router-id=10.10.10.6
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=10.10.10.6 interface=lobridge network=10.10.10.6
add address=10.10.5.1/30 interface=ether3 network=10.10.5.0
add address=10.10.6.1/30 interface=ether5 network=10.10.6.0
add address=10.10.4.2/30 interface=ether4 network=10.10.4.0
/ip dhcp-client
add disabled=no interface=ether1
/mpls ldp
set enabled=yes lsr-id=10.10.10.6 transport-address=10.10.10.6
/mpls ldp interface
add interface=ether3
add interface=ether4
add interface=ether5
/routing ospf network
add area=backbone network=10.10.10.6/32
add area=backbone network=10.10.5.0/30
add area=backbone network=10.10.6.0/30
add area=backbone network=10.10.4.0/30
/system identity
set name=R01.HKI
```
## Результат пингов

### PC1
![alt text](https://github.com/DanilenkoDA/2024_2025-introduction_in_routing-k3323-danilenko_d_a/blob/main/lab3/ping_results/ping1.png)  

### SGI-PRISM 
![alt text](https://github.com/DanilenkoDA/2024_2025-introduction_in_routing-k3323-danilenko_d_a/blob/main/lab3/ping_results/ping2.png)  
![alt text](https://github.com/DanilenkoDA/2024_2025-introduction_in_routing-k3323-danilenko_d_a/blob/main/lab3/ping_results/traceroute.png) 

### R01.SPB
![alt text](https://github.com/DanilenkoDA/2024_2025-introduction_in_routing-k3323-danilenko_d_a/blob/main/lab3/ping_results/neighbor_print.png) 

### R01.HKI
![alt text](https://github.com/DanilenkoDA/2024_2025-introduction_in_routing-k3323-danilenko_d_a/blob/main/lab3/ping_results/neighbor_print2.png) 