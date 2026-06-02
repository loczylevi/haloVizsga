# haloVizsga

### isten terve

## bocsánat Nagy __I__ vel az Isten 🙏🙏🙏 in nomine patris et fili 🙏🙏🙏

<img src="2omb-prayge.gif">
<i>in nomine patris et fili <br>
cuius regio eius religio <br>
in medius res deus ex machine <br>
nomen est omen carpe diem <br>
divida et impara <br>
Eredics atyánk mindörökké ámen</i> 🙏 <br>

# Cisco Packet Tracer IPv4 + VLAN + OSPF Jegyzet

---

# 1. IPv4 címzés alapok

## CIDR maszk

Példa:

```text
192.168.1.64/27
```

A `/27` jelentése:

```text
11111111.11111111.11111111.11100000
```

Decimálisan:

```text
255.255.255.224
```

---

# 2. Blokkméret számolás

Formula:

```text
256 - utolsó maszk oktett
```

Példa:

```text
256 - 224 = 32
```

Subnetek:

```text
0
32
64
96
128
160
192
224
```

---

# 3. Network + Broadcast + Hostok

Példa:

```text
192.168.1.64/27
```

Következő subnet:

```text
192.168.1.96
```

Ezért:

```text
Network:   192.168.1.64
Broadcast: 192.168.1.95
```

Host tartomány:

```text
192.168.1.65 - 192.168.1.94
```

---

# 4. Szabályok

| Típus | Jelentés |
|---|---|
| Network cím | első cím |
| Broadcast cím | utolsó cím |
| Első host | network +1 |
| Utolsó host | broadcast -1 |

---

# 5. /30 hálózatok

Router-router linkekhez.

Példa:

```text
10.0.0.0/30
```

Maszk:

```text
255.255.255.252
```

Blokkméret:

```text
256 - 252 = 4
```

Tartomány:

```text
10.0.0.0 - 10.0.0.3
```

Hostok:

```text
10.0.0.1
10.0.0.2
```

Broadcast:

```text
10.0.0.3
```

---

# 6. Wildcard mask

OSPF-hez kell.

Számolás:

```text
255.255.255.255
-
Subnet Mask
```

Példa:

```text
255.255.255.255
255.255.255.224
----------------
0.0.0.31
```

OSPF:

```cisco
network 192.168.1.64 0.0.0.31 area 0


```

# Cisco eszköz alapkonfiguráció

Az eszközre kattintva feljövő konfigurációs ablak **CLI** fülén az alábbi parancsokat kell kiadni.

1. **NEM lépünk be az automatikus konfigurációba!**
2. Privilegizált módba váltás:

```bash
enable
```

3. Globális konfiguráció módba váltás:

```bash
configure terminal
```

4. Eszköz elnevezése:

```bash
hostname akármi
```

---

## Jelszavas védelem beállítása

A hálózati berendezéseket érdemes jelszóval védeni. Az alapvető beállítási lehetőségeket próbáljuk ki az egyik routeren.

### 1. Privilegizált mód titkosított jelszava

```bash
enable secret hauenapa55
```

### 2. Konzolos belépés jelszava és kiírás szinkronizálás

```bash
configure terminal
line console 0
password hauconpa55
login
logging synchronous
exit
```

### 2. interface ipv4 cim + netmaszk beállitás ás port felkapcsolás
```bash
interface GigabitEthernet0/0
ip address 172.16.17.158 255.255.255.224
no shutdown
```

### 2. interface ipv6 cim + netmaszk beállitás ás port felkapcsolás
```bash
! unicast commandal engedélyezzük a routerbe az ipv6 cimek áramlását
ipv6 unicast-routing
interface GigabitEthernet0/0
ipv6 address 2001:DB8:BE:1::158/64
ipv6 address FE80::1 link-local
```
---

# 7. VLAN alapok

## VLAN létrehozás

```cisco
enable
configure terminal

! VLAN létrehozása
vlan 10

! VLAN név
name USERS
```

---

# 8. Access port

Egyetlen VLAN forgalma.

```cisco
interface fastEthernet0/1

! access mód
switchport mode access

! VLAN hozzárendelés
switchport access vlan 10

! interfész bekapcsolása
no shutdown
```

---

# 9. Trunk port

Több VLAN szállítása.

```cisco
interface fastEthernet0/24

! trunk mód
switchport mode trunk

! engedélyezett VLAN-ok
switchport trunk allowed vlan 10,20,30

no shutdown
```


# 11. Statikus útvonal

Syntax:

```cisco
ip route CELHALOZAT MASZK NEXT-HOP
```

Példa:

```cisco
ip route 10.0.0.0 255.255.255.0 192.168.1.1
```

Jelentés:

```text
A 10.0.0.0 hálózat felé
küldd a csomagot a 192.168.1.1 routernek
```

---

# 12. OSPF alapok

## OSPF bekapcsolása

```cisco
router ospf 1
```

Az `1` a process ID.

---

## Hálózat hozzáadása

```cisco
network 192.168.1.0 0.0.0.255 area 0
```

---

# 13. OSPF area

Leggyakoribb:

```text
area 0
```

Ez a backbone area.

---

# 14. IPv6 alapok

Példa:

```text
2001:DB8:1::/64
```

Hostok:

```text
2001:DB8:1::1
2001:DB8:1::2
2001:DB8:1::3
```

---

# 15. IPv6 konfiguráció

```cisco
interface g0/0

ipv6 address 2001:DB8:1::1/64

no shutdown
```

---

# 16. Show parancsok

## IP interfészek

```cisco
show ip interface brief
```

---

## Routing tábla

```cisco
show ip route
```

---

## VLAN lista

```cisco
show vlan brief
```

---

## Trunk ellenőrzés

```cisco
show interfaces trunk
```

---

## OSPF neighbor

```cisco
show ip ospf neighbor
```

---

# 17. Gyakori hibák

## Elfelejtett no shutdown

Tünet:

```text
administratively down
```

Megoldás:

```cisco
no shutdown
```

---

## Rossz VLAN lista

Rossz:

```cisco
switchport trunk allowed vlan 10 20
```

Helyes:

```cisco
switchport trunk allowed vlan 10,20
```

---

## VLAN nem létezik

Hiba:

```text
Bad VLAN list
```

Megoldás:

```cisco
vlan 10
```

---

# 18. Subnet gyors számolás cheat sheet

| CIDR | Maszk | Host |
|---|---|---|
| /24 | 255.255.255.0 | 254 |
| /25 | 255.255.255.128 | 126 |
| /26 | 255.255.255.192 | 62 |
| /27 | 255.255.255.224 | 30 |
| /28 | 255.255.255.240 | 14 |
| /29 | 255.255.255.248 | 6 |
| /30 | 255.255.255.252 | 2 |

---

# 19. Cisco módok

| Prompt | Jelentés |
|---|---|
| `>` | user mode |
| `#` | privileged mode |
| `(config)#` | global config |
| `(config-if)#` | interface config |

---

# 20. Packet Tracer tipikus sorrend

1. kábelezés
2. IP címek
3. gateway
4. VLAN
5. trunk
6. router interfészek
7. routing
8. ping
9. browser teszt

---
