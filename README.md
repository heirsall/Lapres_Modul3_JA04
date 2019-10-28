# Lapres_Modul3_JA04

<p>Pada topologi tersebut Prof. Oak memerintahkan anda untuk menjadikan PIKACHU sebagai router,
ARTICUNO sebagai DNS Server, MEWTWO sebagai DHCP Server, MOLTRES sebagai Proxy
Server dan UML lainnya sebagai Client.
Pada materi sebelumnya, anda mempelajari cara mengkonfigurasi DHCP Server menggunakan router
PIKACHU. Tetapi Prof. Oak meminta DHCP Server diletakkan pada MEWTWO bukan di router
PIKACHU karena dapat memberatkan kerja router PIKACHU. Karena DHCP Server terletak di
MEWTWO maka PIKACHU digunakan sebagai DHCP Relay.
 </p>
 
 <p> Topologi yang dibuat </p> 
 
 <img src="https://github.com/heirsall/Lapres_Modul3_JA04/blob/master/topologi.jpg"/>
 
 
 
 Kriteria lain yang diminta Prof. Oak pada topologi di atas adalah:
1. Seluruh client TIDAK DIPERBOLEHKAN menggunakan konfigurasi IP Statis.
2. (1) Client di subnet 2 mendapatkan peminjaman alamat IP dengan range 192.168.0.2 s.d.
192.168.0.10 dan 192.168.0.20 s.d. 192.168.0.30 dengan netmask 255.255.255.0.
3. (2) Client di subnet 3 mendapatkan peminjaman alamat IP dengan range dari 192.168.1.2 s.d.
192.168.1.99 dengan netmask 255.255.255.0.
4. (3) Client di subnet 2 mendapatkan peminjaman alamat IP selama 10 menit dan client di
subnet 3 mendapatkan peminjaman alamat IP selama 5 menit.
5. (4) Masing-masing client harus mendapatkan konfigurasi DNS / nameserver yang terdiri atas
IP ARTICUNO, 202.46.129.2, dan 10.151.36.7 secara otomatis.
6. (5) Client PSYDUCK selalu mendapatkan IP 192.168.1.28, TANPA menggunakan
konfigurasi IP Statis.

<p> ubah /etc/dhcp/dhcpd.conf </p>

<img src ="https://github.com/heirsall/Lapres_Modul3_JA04/blob/master/dhcpd.conf.png" />

<p> nano /etc/network/interface di semua client dan samakan isinya </p>

<img src ="https://github.com/heirsall/Lapres_Modul3_JA04/blob/master/interfaces.png" />



<p> cat etc/resolv.conf </p>

<img src ="https://github.com/heirsall/Lapres_Modul3_JA04/blob/master/resolv.conf.png" />

<p> ifconfig client client </p>

<img src ="https://github.com/heirsall/Lapres_Modul3_JA04/blob/master/ifconfig.cubon.png" />
<img src ="https://github.com/heirsall/Lapres_Modul3_JA04/blob/master/ifconfig.snorlax.png" />
<img src ="https://github.com/heirsall/Lapres_Modul3_JA04/blob/master/ifconfig.psyduk.png" />


Prof. Oak tidak ingin jaringan lokalnya terhubung ke Internet secara langsung. Ia ingin jaringan
lokalnya melalui sebuah medium yang dapat menyaring dan membatasi sebelum terhubung ke
Internet, sehingga ia meminta anda membuatkan Proxy untuk kota Pallet.
Pertama, Prof. Oak ingin orang yang bisa mengakses internet melalui proxy hanyalah penduduk kota
Pallet dan trainer yang telah mendaftar di gym. 

(6) Prof. Oak meminta anda membuatkan user
otentikasi dengan format:

● User : yy_moltres_user
● Password : yy_password

Note: yy adalah nama kelompok masing-masing. Contoh : a1_moltres_user

● install squid3 pada UML MOLTRES 
``apt-get install squid3``
● install apache2-utils pada UML MOLTRES 

``apt-get install apache2-utils``
● Buat user dan password baru. 
`` htpasswd -c /etc/squid3/passwd a4_moltres_user dan password: a4_password``

konfigurasikan nano /etc/squid3/squid.conf

``
http_port 7777
visible_hostname mewtwo

auth_param basic program /usr/lib/squid3/ncsa_auth /etc/squid3/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED
http_access allow USERS
``

restart squid 

``service squid3 restart``




Beberapa bulan setelah mendapatkan akses internet, para trainer (subnet 2) menjadi malas melatih
pokemonnya karena terlalu asyik bermain Pokemon Go sehingga para pokemon menjadi lemah.
Selain itu, para penduduk (subnet 3) menjadi malas untuk membuka toko mereka karena terlalu asyik
menonton YouTube Atta Halilintar. Karena permasalahan tersebut, anda diminta membatasi akses
internet untuk penduduk kota Pallet dengan pembatasan akses proxy sebagai berikut:

``````
tes tes
```````
● (7) Untuk Client pada subnet 2 HANYA BISA mengakses internet pada hari Senin
s.d. Jumat pukul 11.00 s.d. 13.00 WIB
● (8) Untuk Client pada subnet 3 HANYA BISA mengakses internet pada hari Senin
s.d. Jumat pukul 20.00 s.d. 07.00 WIB pada keesokan harinya.

Prof. Oak mendapat informasi dari salah satu trainer bahwa google.com merupakan search engine
buatan tim Rocket. Oleh karena itu, Prof. Oak memerintahkan anda untuk membuat aturan (9) setiap
ada koneksi dari subnet AJK (10.151.36.0/24) yang mengakses google.com akan langsung dialihkan
menuju duckduckgo.com. (10) Selain itu, demi menjaga keamanan kota Pallet, anda diminta untuk
mem-block setiap koneksi yang berasal dari subnet Informatics_wifi (10.151.252.0/22) menuju
http://10.151.36.5:5000, sebab subnet Informatics_wifi adalah milik tim Rocket.
(11) Karena menurut Prof. Oak menghafalkan IP Proxy kota Pallet cukup merepotkan, kalian diminta
untuk mempermudah trainer dan penduduk dalam menggunakan Proxy yaitu cukup dengan
mengetikkan proxy.yy.com dan memasukkan port 8888.
