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

Tahap pertama yang perlu dilakukan adalah menyiapkan konfigurasi topologi dan setup sesuai dengan ketentuan yang telah ditetapkan sebelumnya. Untuk keperluan pengujian, kita harus menambahkan domain terdaftar seperti riegel.canyon.a19.com untuk worker Laravel dan granz.channel.a19.com untuk worker PHP. Kedua domain tersebut harus diarahkan ke worker yang memiliki alamat IP `192.178.x.1`. Karena sebelumnya semua worker menggunakan layanan DHCP dalam konfigurasi topologi, maka perlu dilakukan beberapa modifikasi pada node Lugner dan Fern agar domain-domain tersebut dapat berfungsi dengan baik.

- Fern (Laravel worker)
```
auto eth0
iface eth0 inet static
	address 192.178.4.1
	netmask 255.255.255.0
	gateway 192.178.4.0
```

- Lugner (PHP worker)
```
auto eth0
iface eth0 inet static
	address 192.178.3.1
	netmask 255.255.255.0
	gateway 192.178.3.0
```

Jalankan command di bawah ini di DNS Server (Heiter)
```
echo 'zone "riegel.canyon.a19.com" {
    type master;
    file "/etc/bind/sites/riegel.canyon.a19.com";
};

zone "granz.channel.a19.com" {
    type master;
    file "/etc/bind/sites/granz.channel.a19.com";
};

zone "1.178.192.in-addr.arpa" {
    type master;
    file "/etc/bind/sites/1.178.192.in-addr.arpa";
};' > /etc/bind/named.conf.local

mkdir -p /etc/bind/sites
cp /etc/bind/db.local /etc/bind/sites/riegel.canyon.a19.com
cp /etc/bind/db.local /etc/bind/sites/granz.channel.a19.com
cp /etc/bind/db.local /etc/bind/sites/1.178.192.in-addr.arpa

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     riegel.canyon.a19.com. root.riegel.canyon.a19.com. (
                        2023111401      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      riegel.canyon.a19.com.
@       IN      A       192.178.4.1     ; IP Fern
www     IN      CNAME   riegel.canyon.a19.com.' > /etc/bind/sites/riegel.canyon.a19.com

echo '
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     granz.channel.a19.com. root.granz.channel.a19.com. (
                        2023111401      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      granz.channel.a19.com.
@       IN      A       192.178.3.1     ; IP Lugner
www     IN      CNAME   granz.channel.a19.com.' > /etc/bind/sites/granz.channel.a19.com

echo 'options {
      directory "/var/cache/bind";

      forwarders {
              192.168.122.1;
      };

      // dnssec-validation auto;
      allow-query{any;};
      auth-nxdomain no;    # conform to RFC1035
      listen-on-v6 { any; };
}; ' >/etc/bind/named.conf.options

service bind9 start
```

## No 2
Client yang melalui Switch3 mendapatkan range IP dari `192.178.3.16` - `192.178.3.32` dan `192.178.3.64` - `192.178.3.80`

Jalankan command di bawah ini pada DHCP Server
```
echo 'subnet 192.178.1.0 netmask 255.255.255.0 {
}

subnet 192.178.2.0 netmask 255.255.255.0 {
}

subnet 192.178.3.0 netmask 255.255.255.0 {
    range 192.178.3.16 192.178.3.32;
    range 192.178.3.64 192.178.3.80;
    option routers 192.178.3.0;
}' > /etc/dhcp/dhcpd.conf
```
## No 3
Client yang melalui Switch4 mendapatkan range IP dari ```[prefix IP].4.12 - [prefix IP].4.20``` dan ```[prefix IP].4.160 - [prefix IP].4.168```

Langkah berikutnya adalah menambahkan beberapa konfigurasi baru pada switch4 dengan menggunakan perintah yang telah disediakan sebelumnya.
```
echo 'subnet 192.178.1.0 netmask 255.255.255.0 {
}

subnet 192.178.2.0 netmask 255.255.255.0 {
}

subnet 192.178.3.0 netmask 255.255.255.0 {
    range 192.178.3.16 192.178.3.32;
    range 192.178.3.64 192.178.3.80;
    option routers 192.178.3.0;
}

subnet 192.178.4.0 netmask 255.255.255.0 {
    range 192.178.4.12 192.178.4.20;
    range 192.178.4.160 192.178.4.168;
    option routers 192.178.4.0;
} ' > /etc/dhcp/dhcpd.conf
```

## No 4
Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut

Langkah selanjutnya adalah menambahkan beberapa konfigurasi baru pada switch4. Ini melibatkan penggunaan perintah untuk menetapkan opsi `broadcast-address` dan option `domain-name-server`. Hal ini bertujuan agar DNS yang telah dipersiapkan sebelumnya dapat digunakan dengan efektif.
```
subnet 192.178.3.0 netmask 255.255.255.0 {
    ...
    option broadcast-address 192.178.3.255;
    option domain-name-servers 192.178.1.2;
    ...
}

subnet 192.178.4.0 netmask 255.255.255.0 {
    option broadcast-address 192.178.4.255;
    option domain-name-servers 192.178.1.2;
}
```

Lalu gunakan shell script berikut
```
echo 'subnet 192.178.1.0 netmask 255.255.255.0 {
}

subnet 192.178.2.0 netmask 255.255.255.0 {
}

subnet 192.178.3.0 netmask 255.255.255.0 {
    range 192.178.3.16 192.178.3.32;
    range 192.178.3.64 192.178.3.80;
    option routers 192.178.3.0;
    option broadcast-address 192.178.3.255;
    option domain-name-servers 192.178.1.2;
}

subnet 192.178.4.0 netmask 255.255.255.0 {
    range 192.178.4.12 192.178.4.20;
    range 192.178.4.160 192.178.4.168;
    option routers 192.178.4.0;
    option broadcast-address 192.178.4.255;
    option domain-name-servers 192.178.1.2;
} ' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server start
```

Untuk setup DHCP Relay, jalankan command dibawah ini
```
echo '# Defaults for isc-dhcp-relay initscript
# sourced by /etc/init.d/isc-dhcp-relay
# installed at /etc/default/isc-dhcp-relay by the maintainer scripts

#
# This is a POSIX shell fragment
#

# What servers should the DHCP relay forward requests to?
SERVERS="192.178.1.1"

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES="eth1 eth2 eth3 eth4"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS=""' > /etc/default/isc-dhcp-relay

service isc-dhcp-relay start
```

Dalam file `/etc/sysctl.conf`, perlu dihilangkan tanda komentar (#) pada bagian `net.ipv4.ip_forward=1`. Hal ini akan mengaktifkan fungsi IP forwarding di sistem

Penting untuk mengingat untuk me-restart seluruh klien agar mereka dapat memperoleh penyewaan alamat IP dari DHCP Server yang telah dikonfigurasi sebelumnya

## No 5
Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit

Diperlukan penyesuaian pada durasi peminjaman alamat IP (lease time) untuk switch3 dan switch4. Berdasarkan permintaan, switch3 akan memberikan alamat IP untuk 3 menit (180 detik) dan switch4 untuk 12 menit (720 detik). Durasi maksimum peminjaman alamat IP yang diminta adalah 96 menit (5760 detik).

Untuk menyesuaikan durasi peminjaman IP sesuai aturan yang disebutkan, perlu dilakukan penyesuaian konfigurasi menggunakan fungsi `default-lease-time` dan `max-lease-time` dalam konfigurasi DHCP Server. Dilakukan pengaturan durasi peminjaman IP untuk switch3 dan switch4 ke dalam detik.

Setelah konfigurasi tersebut disesuaikan, DHCP Server perlu di-restart agar perubahan konfigurasi dapat diterapkan dengan menjalankan perintah yang sesuai.
```
echo 'subnet 192.178.1.0 netmask 255.255.255.0 {
}

subnet 192.178.2.0 netmask 255.255.255.0 {
}

subnet 192.178.3.0 netmask 255.255.255.0 {
    range 192.178.3.16 192.178.3.32;
    range 192.178.3.64 192.178.3.80;
    option routers 192.178.3.0;
    option broadcast-address 192.178.3.255;
    option domain-name-servers 192.178.1.2;
    default-lease-time 180;
    max-lease-time 5760;
}

subnet 192.178.4.0 netmask 255.255.255.0 {
    range 192.178.4.12 192.178.4.20;
    range 192.178.4.160 192.178.4.168;
    option routers 192.178.4.0;
    option broadcast-address 192.178.4.255;
    option domain-name-servers 192.178.1.2;
    default-lease-time 720;
    max-lease-time 5760;
}

service isc-dhcp-server restart
```
