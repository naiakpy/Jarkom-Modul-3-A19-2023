# Jarkom-Modul-3-A19-2023

# Kelompok A19:
| Nama | NRP |
| ---------------------- | ---------- |
| Nayya Kamila Putri Y | 5025211183 |
| Javier Nararya Aqsa Setiyono | 5025211245 |

# Konfigurasi
- Aura
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.178.1.0
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.178.2.0
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 192.178.3.0
	netmask 255.255.255.0

auto eth4
iface eth4 inet static
	address 192.178.4.0
	netmask 255.255.255.0
```

- Himmel (DHCP server)
```
auto eth0
iface eth0 inet static
	address 192.178.1.1
	netmask 255.255.255.0
	gateway 192.178.1.0
```

- Heiter (DNS server)
```
auto eth0
iface eth0 inet static
	address 192.178.1.2
	netmask 255.255.255.0
	gateway 192.178.1.0
```

- Denken (Database server)
```
auto eth0
iface eth0 inet static
	address 192.178.2.1
	netmask 255.255.255.0
	gateway 192.178.2.0
```

- Eisen (Load balancer)
```
auto eth0
iface eth0 inet static
	address 192.178.2.2
	netmask 255.255.255.0
	gateway 192.178.2.0
```

- Fern (Laravel worker)
```
auto eth0
iface eth0 inet static
	address 192.178.4.1
	netmask 255.255.255.0
	gateway 192.178.4.0
```

- Flamme (Laravel worker)
```
auto eth0
iface eth0 inet static
	address 192.178.4.2
	netmask 255.255.255.0
	gateway 192.178.4.0
```

- Frieren (Laravel worker)
```
auto eth0
iface eth0 inet static
	address 192.178.2.2
	netmask 255.255.255.0
	gateway 192.178.2.0
```

- Lugner (PHP worker)
```
auto eth0
iface eth0 inet static
	address 192.178.3.1
	netmask 255.255.255.0
	gateway 192.178.3.0
```

- Linie (PHP worker)
```
auto eth0
iface eth0 inet static
	address 192.178.3.2
	netmask 255.255.255.0
	gateway 192.178.3.0
```

- Lawine (PHP worker)
```
auto eth0
iface eth0 inet static
	address 192.178.3.3
	netmask 255.255.255.0
	gateway 192.178.3.0
```

- Sein, Stark, Revolte, dan Richter (Client)
```
auto eth0
iface eth0 inet dhcp
```

## No 1
Semua CLIENT harus menggunakan konfigurasi dari DHCP Server.

## No 2
Client yang melalui Switch3 mendapatkan range IP dari `192.178.3.16` - `192.178.3.32` dan `192.178.3.64` - `192.178.3.80`
