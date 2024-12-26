University: [ITMO University](https://itmo.ru/ru/)
Faculty: [FICT](https://fict.itmo.ru)
Course: [Introduction in routing](https://github.com/itmo-ict-faculty/introduction-in-routing)
Year: 2024/2025
Group: K3323
Author: Danilenko Dmitriy Alexandrovich
Lab: Lab4
Date of create: 25.12.2024
Date of finished: 

# Лабораторная работа №4 "Эмуляция распределенной корпоративной сети связи, настройка iBGP, организация L3VPN, VPLS"

Схема:
![alt text](https://github.com/DanilenkoDA/2024_2025-introduction_in_routing-k3323-danilenko_d_a/blob/main/lab4/schema.png)

# Часть 1

## Конфиги устройств:

### R01.SPB

```
# dec/25/2024 19:10:53 by RouterOS 6.47.9
# software id = 
#
#
#
/interface bridge
add name=loopback0
/interface ethernet
set [ find default-name=ether1 ] disable-running-check=no
set [ find default-name=ether2 ] disable-running-check=no
set [ find default-name=ether3 ] disable-running-check=no
set [ find default-name=ether4 ] disable-running-check=no
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing bgp instance
set default as=65500 router-id=10.10.10.1
/routing ospf instance
set [ find default=yes ] router-id=10.10.10.1
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=10.10.10.1 interface=loopback0 network=10.10.10.1
add address=192.168.1.1/30 interface=ether3 network=192.168.1.0
add address=192.168.2.1/30 interface=ether4 network=192.168.2.0
/ip dhcp-client
add disabled=no interface=ether1
/ip route vrf
add export-route-targets=65500:100 import-route-targets=65500:100 interfaces=ether3 route-distinguisher=65500:100 \
    routing-mark=vrf1
/mpls ldp
set enabled=yes lsr-id=10.10.10.1 transport-address=10.10.10.1
/mpls ldp interface
add interface=ether3
add interface=ether4
/routing bgp instance vrf
add redistribute-connected=yes redistribute-ospf=yes routing-mark=vrf1
/routing bgp peer
add address-families=vpnv4 name=peer1 remote-address=10.10.10.2 remote-as=65500 update-source=loopback0
/routing ospf network
add area=backbone network=192.168.1.0/30
add area=backbone network=192.168.2.0/30
add area=backbone network=10.10.10.1/32
/system identity
set name=R01.SPB


```

### R01.HKI

```
# dec/25/2024 19:12:28 by RouterOS 6.47.9
# software id = 
#
#
#
/interface bridge
add name=loopback0
/interface ethernet
set [ find default-name=ether1 ] disable-running-check=no
set [ find default-name=ether2 ] disable-running-check=no
set [ find default-name=ether3 ] disable-running-check=no
set [ find default-name=ether4 ] disable-running-check=no
set [ find default-name=ether5 ] disable-running-check=no
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing bgp instance
set default as=65500 router-id=10.10.10.2
/routing ospf instance
set [ find default=yes ] router-id=10.10.10.2
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=10.10.10.2 interface=loopback0 network=10.10.10.2
add address=192.168.2.2/30 interface=ether3 network=192.168.2.0
add address=192.168.3.1/30 interface=ether4 network=192.168.3.0
add address=192.168.4.1/30 interface=ether5 network=192.168.4.0
/ip dhcp-client
add disabled=no interface=ether1
/mpls ldp
set enabled=yes lsr-id=10.10.10.2 transport-address=10.10.10.2
/mpls ldp interface
add interface=ether3
add interface=ether4
add interface=ether5
/routing bgp peer
add address-families=vpnv4 name=peer1 remote-address=10.10.10.1 remote-as=65500 update-source=loopback0
add address-families=vpnv4 name=peer2 remote-address=10.10.10.3 remote-as=65500 route-reflect=yes update-source=\
    loopback0
add address-families=vpnv4 name=peer3 remote-address=10.10.10.5 remote-as=65500 route-reflect=yes update-source=\
    loopback0
/routing ospf network
add area=backbone network=192.168.2.0/30
add area=backbone network=192.168.3.0/30
add area=backbone network=192.168.4.0/30
add area=backbone network=10.10.10.2/32
/system identity
set name=R01.HKI
```

### R01.NY

```
# dec/25/2024 19:14:16 by RouterOS 6.47.9
# software id = 
#
#
#
/interface bridge
add name=loopback0
/interface ethernet
set [ find default-name=ether1 ] disable-running-check=no
set [ find default-name=ether2 ] disable-running-check=no
set [ find default-name=ether3 ] disable-running-check=no
set [ find default-name=ether4 ] disable-running-check=no
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing bgp instance
set default as=65500 router-id=10.10.10.4
/routing ospf instance
set [ find default=yes ] router-id=10.10.10.4
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=10.10.10.4 interface=loopback0 network=10.10.10.4
add address=192.168.6.2/30 interface=ether3 network=192.168.6.0
add address=192.168.7.1/30 interface=ether4 network=192.168.7.0
/ip dhcp-client
add disabled=no interface=ether1
/ip route vrf
add export-route-targets=65500:100 import-route-targets=65500:100 interfaces=ether4 route-distinguisher=65500:100 \
    routing-mark=vrf1
/mpls ldp
set enabled=yes lsr-id=10.10.10.4 transport-address=10.10.10.4
/mpls ldp interface
add interface=ether3
add interface=ether4
/routing bgp instance vrf
add redistribute-connected=yes redistribute-ospf=yes routing-mark=vrf1
/routing bgp peer
add address-families=vpnv4 name=peer1 remote-address=10.10.10.3 remote-as=65500 update-source=loopback0
/routing ospf network
add area=backbone network=192.168.6.0/30
add area=backbone network=192.168.7.0/30
add area=backbone network=10.10.10.4/32
/system identity
set name=R01.NY
```

### R01.LND

```
# dec/25/2024 19:15:09 by RouterOS 6.47.9
# software id = 
#
#
#
/interface bridge
add name=loopback0
/interface ethernet
set [ find default-name=ether1 ] disable-running-check=no
set [ find default-name=ether2 ] disable-running-check=no
set [ find default-name=ether3 ] disable-running-check=no
set [ find default-name=ether4 ] disable-running-check=no
set [ find default-name=ether5 ] disable-running-check=no
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing bgp instance
set default as=65500 router-id=10.10.10.3
/routing ospf instance
set [ find default=yes ] router-id=10.10.10.3
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=10.10.10.3 interface=loopback0 network=10.10.10.3
add address=192.168.4.2/30 interface=ether3 network=192.168.4.0
add address=192.168.5.1/30 interface=ether4 network=192.168.5.0
add address=192.168.6.1/30 interface=ether5 network=192.168.6.0
/ip dhcp-client
add disabled=no interface=ether1
/mpls ldp
set enabled=yes lsr-id=10.10.10.3 transport-address=10.10.10.3
/mpls ldp interface
add interface=ether3
add interface=ether4
add interface=ether5
/routing bgp peer
add address-families=vpnv4 name=peer1 remote-address=10.10.10.4 remote-as=65500 update-source=loopback0
add address-families=vpnv4 name=peer2 remote-address=10.10.10.2 remote-as=65500 route-reflect=yes update-source=\
    loopback0
add address-families=vpnv4 name=peer3 remote-address=10.10.10.5 remote-as=65500 route-reflect=yes update-source=\
    loopback0
/routing ospf network
add area=backbone network=192.168.4.0/30
add area=backbone network=192.168.5.0/30
add area=backbone network=192.168.6.0/30
add area=backbone network=10.10.10.3/32
/system identity
set name=R01.LND
```

### R01.LBN

```
# dec/25/2024 19:16:14 by RouterOS 6.47.9
# software id = 
#
#
#
/interface bridge
add name=loopback0
/interface ethernet
set [ find default-name=ether1 ] disable-running-check=no
set [ find default-name=ether2 ] disable-running-check=no
set [ find default-name=ether3 ] disable-running-check=no
set [ find default-name=ether4 ] disable-running-check=no
set [ find default-name=ether5 ] disable-running-check=no
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing bgp instance
set default as=65500 router-id=10.10.10.5
/routing ospf instance
set [ find default=yes ] router-id=10.10.10.5
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=10.10.10.5 interface=loopback0 network=10.10.10.5
add address=192.168.3.2/30 interface=ether3 network=192.168.3.0
add address=192.168.8.1/30 interface=ether4 network=192.168.8.0
add address=192.168.5.2/30 interface=ether5 network=192.168.5.0
/ip dhcp-client
add disabled=no interface=ether1
/mpls ldp
set enabled=yes lsr-id=10.10.10.5 transport-address=10.10.10.5
/mpls ldp interface
add interface=ether3
add interface=ether4
add interface=ether5
/routing bgp peer
add address-families=vpnv4 name=peer1 remote-address=10.10.10.6 remote-as=65500 update-source=loopback0
add address-families=vpnv4 name=peer2 remote-address=10.10.10.2 remote-as=65500 route-reflect=yes update-source=\
    loopback0
add address-families=vpnv4 name=peer3 remote-address=10.10.10.3 remote-as=65500 route-reflect=yes update-source=\
    loopback0
/routing ospf network
add area=backbone network=192.168.3.0/30
add area=backbone network=192.168.8.0/30
add area=backbone network=192.168.5.0/30
add area=backbone network=10.10.10.5/32
/system identity
set name=R01.LBN

```

### R01.SVL

```
# dec/25/2024 19:17:21 by RouterOS 6.47.9
# software id = 
#
#
#
/interface bridge
add name=loopback0
/interface ethernet
set [ find default-name=ether1 ] disable-running-check=no
set [ find default-name=ether2 ] disable-running-check=no
set [ find default-name=ether3 ] disable-running-check=no
set [ find default-name=ether4 ] disable-running-check=no
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing bgp instance
set default as=65500 router-id=10.10.10.6
/routing ospf instance
set [ find default=yes ] router-id=10.10.10.6
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=10.10.10.6 interface=loopback0 network=10.10.10.6
add address=192.168.8.2/30 interface=ether3 network=192.168.8.0
add address=192.168.9.1/30 interface=ether4 network=192.168.9.0
/ip dhcp-client
add disabled=no interface=ether1
/ip route vrf
add export-route-targets=65500:100 import-route-targets=65500:100 interfaces=ether4 route-distinguisher=65500:100 routing-mark=vrf1
/mpls ldp
set enabled=yes lsr-id=10.10.10.6 transport-address=10.10.10.6
/mpls ldp interface
add interface=ether3
add interface=ether4
/routing bgp instance vrf
add redistribute-connected=yes redistribute-ospf=yes routing-mark=vrf1
/routing bgp peer
add address-families=vpnv4 name=peer1 remote-address=10.10.10.5 remote-as=65500 update-source=loopback0
/routing ospf network
add area=backbone network=192.168.8.0/30
add area=backbone network=192.168.9.0/30
add area=backbone network=10.10.10.6/32
/system identity
set name=R01.SVL
```

## Результат пингов

![alt text](https://github.com/DanilenkoDA/2024_2025-introduction_in_routing-k3323-danilenko_d_a/blob/main/lab4/ping_results/ping1.png)

![alt text](https://github.com/DanilenkoDA/2024_2025-introduction_in_routing-k3323-danilenko_d_a/blob/main/lab4/ping_results/ping2.png)

![alt text](https://github.com/DanilenkoDA/2024_2025-introduction_in_routing-k3323-danilenko_d_a/blob/main/lab4/ping_results/ping3.png)  

# Часть 2

## Конфиги устройств

### R01.SPB

```
# dec/26/2024 19:57:12 by RouterOS 6.47.9
# software id = 
#
#
#
/interface bridge
add name=loopback0
add name=vpls protocol-mode=none
/interface ethernet
set [ find default-name=ether1 ] disable-running-check=no
set [ find default-name=ether2 ] disable-running-check=no
set [ find default-name=ether3 ] disable-running-check=no
set [ find default-name=ether4 ] disable-running-check=no
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing bgp instance
set default as=65500 router-id=10.10.10.1
/routing ospf instance
set [ find default=yes ] router-id=10.10.10.1
/interface bridge port
add bridge=vpls interface=ether3
/interface vpls bgp-vpls
add bridge=vpls export-route-targets=1:2 import-route-targets=1:2 name=vpls route-distinguisher=1:2
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=10.10.10.1 interface=loopback0 network=10.10.10.1
add address=192.168.1.1/30 interface=ether3 network=192.168.1.0
add address=192.168.2.1/30 interface=ether4 network=192.168.2.0
add address=10.0.0.2/24 interface=vpls network=10.0.0.0
/ip dhcp-client
add disabled=no interface=ether1
/mpls ldp
set enabled=yes lsr-id=10.10.10.1 transport-address=10.10.10.1
/mpls ldp interface
add interface=ether3
add interface=ether4
/routing bgp instance vrf
add redistribute-connected=yes redistribute-ospf=yes routing-mark=vrf1
/routing bgp peer
add address-families=l2vpn name=peer1 remote-address=10.10.10.2 remote-as=65500 update-source=loopback0
/routing ospf network
add area=backbone network=192.168.1.0/30
add area=backbone network=192.168.2.0/30
add area=backbone network=10.10.10.1/32
/system identity
set name=R01.SPB
```

### R01.HKI

```
# dec/26/2024 20:08:15 by RouterOS 6.47.9
# software id = 
#
#
#
/interface bridge
add name=loopback0
/interface ethernet
set [ find default-name=ether1 ] disable-running-check=no
set [ find default-name=ether2 ] disable-running-check=no
set [ find default-name=ether3 ] disable-running-check=no
set [ find default-name=ether4 ] disable-running-check=no
set [ find default-name=ether5 ] disable-running-check=no
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing bgp instance
set default as=65500 router-id=10.10.10.2
/routing ospf instance
set [ find default=yes ] router-id=10.10.10.2
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=10.10.10.2 interface=loopback0 network=10.10.10.2
add address=192.168.2.2/30 interface=ether3 network=192.168.2.0
add address=192.168.3.1/30 interface=ether4 network=192.168.3.0
add address=192.168.4.1/30 interface=ether5 network=192.168.4.0
/ip dhcp-client
add disabled=no interface=ether1
/mpls ldp
set enabled=yes lsr-id=10.10.10.2 transport-address=10.10.10.2
/mpls ldp interface
add interface=ether3
add interface=ether4
add interface=ether5
/routing bgp peer
add address-families=l2vpn name=peer1 remote-address=10.10.10.1 remote-as=65500 update-source=loopback0
add address-families=l2vpn name=peer2 remote-address=10.10.10.3 remote-as=65500 route-reflect=yes update-source=loopback0
add address-families=l2vpn name=peer3 remote-address=10.10.10.5 remote-as=65500 route-reflect=yes update-source=loopback0
/routing ospf network
add area=backbone network=192.168.2.0/30
add area=backbone network=192.168.3.0/30
add area=backbone network=192.168.4.0/30
add area=backbone network=10.10.10.2/32
/system identity
set name=R01.HKI
```

### R01.NY

```
# dec/26/2024 20:12:00 by RouterOS 6.47.9
# software id = 
#
#
#
/interface bridge
add name=loopback0
add name=vpls protocol-mode=none
/interface ethernet
set [ find default-name=ether1 ] disable-running-check=no
set [ find default-name=ether2 ] disable-running-check=no
set [ find default-name=ether3 ] disable-running-check=no
set [ find default-name=ether4 ] disable-running-check=no
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing bgp instance
set default as=65500 router-id=10.10.10.4
/routing ospf instance
set [ find default=yes ] router-id=10.10.10.4
/interface bridge port
add bridge=vpls interface=ether4
/interface vpls bgp-vpls
add bridge=vpls export-route-targets=1:2 import-route-targets=1:2 name=vpls route-distinguisher=1:2 site-id=6
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=10.10.10.4 interface=loopback0 network=10.10.10.4
add address=192.168.6.2/30 interface=ether3 network=192.168.6.0
add address=192.168.7.1/30 interface=ether4 network=192.168.7.0
add address=10.0.0.1/24 interface=vpls network=10.0.0.0
/ip dhcp-client
add disabled=no interface=ether1
/mpls ldp
set enabled=yes lsr-id=10.10.10.4 transport-address=10.10.10.4
/mpls ldp interface
add interface=ether3
add interface=ether4
/routing bgp instance vrf
add redistribute-connected=yes redistribute-ospf=yes routing-mark=vrf1
/routing bgp peer
add address-families=l2vpn name=peer1 remote-address=10.10.10.3 remote-as=65500 update-source=loopback0
/routing ospf network
add area=backbone network=192.168.6.0/30
add area=backbone network=192.168.7.0/30
add area=backbone network=10.10.10.4/32
/system identity
set name=R01.NY
```

### R01.LND

```
# dec/26/2024 20:16:02 by RouterOS 6.47.9
# software id = 
#
#
#
/interface bridge
add name=loopback0
/interface ethernet
set [ find default-name=ether1 ] disable-running-check=no
set [ find default-name=ether2 ] disable-running-check=no
set [ find default-name=ether3 ] disable-running-check=no
set [ find default-name=ether4 ] disable-running-check=no
set [ find default-name=ether5 ] disable-running-check=no
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing bgp instance
set default as=65500 router-id=10.10.10.3
/routing ospf instance
set [ find default=yes ] router-id=10.10.10.3
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=10.10.10.3 interface=loopback0 network=10.10.10.3
add address=192.168.4.2/30 interface=ether3 network=192.168.4.0
add address=192.168.5.1/30 interface=ether4 network=192.168.5.0
add address=192.168.6.1/30 interface=ether5 network=192.168.6.0
/ip dhcp-client
add disabled=no interface=ether1
/mpls ldp
set enabled=yes lsr-id=10.10.10.3 transport-address=10.10.10.3
/mpls ldp interface
add interface=ether3
add interface=ether4
add interface=ether5
/routing bgp peer
add address-families=l2vpn name=peer1 remote-address=10.10.10.4 remote-as=65500 update-source=loopback0
add address-families=l2vpn name=peer2 remote-address=10.10.10.2 remote-as=65500 route-reflect=yes update-source=loopback0
add address-families=l2vpn name=peer3 remote-address=10.10.10.5 remote-as=65500 route-reflect=yes update-source=loopback0
/routing ospf network
add area=backbone network=192.168.4.0/30
add area=backbone network=192.168.5.0/30
add area=backbone network=192.168.6.0/30
add area=backbone network=10.10.10.3/32
/system identity
set name=R01.LND
```

### R01.LBN

```
# dec/26/2024 20:20:54 by RouterOS 6.47.9
# software id = 
#
#
#
/interface bridge
add name=loopback0
/interface ethernet
set [ find default-name=ether1 ] disable-running-check=no
set [ find default-name=ether2 ] disable-running-check=no
set [ find default-name=ether3 ] disable-running-check=no
set [ find default-name=ether4 ] disable-running-check=no
set [ find default-name=ether5 ] disable-running-check=no
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing bgp instance
set default as=65500 router-id=10.10.10.5
/routing ospf instance
set [ find default=yes ] router-id=10.10.10.5
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=10.10.10.5 interface=loopback0 network=10.10.10.5
add address=192.168.3.2/30 interface=ether3 network=192.168.3.0
add address=192.168.8.1/30 interface=ether4 network=192.168.8.0
add address=192.168.5.2/30 interface=ether5 network=192.168.5.0
/ip dhcp-client
add disabled=no interface=ether1
/mpls ldp
set enabled=yes lsr-id=10.10.10.5 transport-address=10.10.10.5
/mpls ldp interface
add interface=ether3
add interface=ether4
add interface=ether5
/routing bgp peer
add address-families=l2vpn name=peer1 remote-address=10.10.10.6 remote-as=65500 update-source=loopback0
add address-families=l2vpn name=peer2 remote-address=10.10.10.2 remote-as=65500 route-reflect=yes update-source=loopback0
add address-families=l2vpn name=peer3 remote-address=10.10.10.3 remote-as=65500 route-reflect=yes update-source=loopback0
/routing ospf network
add area=backbone network=192.168.3.0/30
add area=backbone network=192.168.8.0/30
add area=backbone network=192.168.5.0/30
add area=backbone network=10.10.10.5/32
/system identity
set name=R01.LBN
```

### R01.SVL

```
# dec/26/2024 20:23:40 by RouterOS 6.47.9
# software id = 
#
#
#
/interface bridge
add name=loopback0
add name=vpls protocol-mode=none
/interface ethernet
set [ find default-name=ether1 ] disable-running-check=no
set [ find default-name=ether2 ] disable-running-check=no
set [ find default-name=ether3 ] disable-running-check=no
set [ find default-name=ether4 ] disable-running-check=no
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing bgp instance
set default as=65500 router-id=10.10.10.6
/routing ospf instance
set [ find default=yes ] router-id=10.10.10.6
/interface bridge port
add bridge=vpls interface=ether4
/interface vpls bgp-vpls
add bridge=vpls export-route-targets=1:2 import-route-targets=1:2 name=vpls route-distinguisher=1:2 site-id=4
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=10.10.10.6 interface=loopback0 network=10.10.10.6
add address=192.168.8.2/30 interface=ether3 network=192.168.8.0
add address=192.168.9.1/30 interface=ether4 network=192.168.9.0
add address=10.0.0.3/24 interface=vpls network=10.0.0.0
/ip dhcp-client
add disabled=no interface=ether1
/mpls ldp
set enabled=yes lsr-id=10.10.10.6 transport-address=10.10.10.6
/mpls ldp interface
add interface=ether3
add interface=ether4
/routing bgp instance vrf
add redistribute-connected=yes redistribute-ospf=yes routing-mark=vrf1
/routing bgp peer
add address-families=l2vpn name=peer1 remote-address=10.10.10.5 remote-as=65500 update-source=loopback0
/routing ospf network
add area=backbone network=192.168.8.0/30
add area=backbone network=192.168.9.0/30
add area=backbone network=10.10.10.6/32
/system identity
set name=R01.SVL
```

## Результаты пингов

![alt text](https://github.com/DanilenkoDA/2024_2025-introduction_in_routing-k3323-danilenko_d_a/blob/main/lab4/ping_results/ping4.png)

![alt text](https://github.com/DanilenkoDA/2024_2025-introduction_in_routing-k3323-danilenko_d_a/blob/main/lab4/ping_results/ping5.png)
