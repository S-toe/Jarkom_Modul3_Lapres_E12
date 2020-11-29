# Jarkom_Modul3_Lapres_E12
* Restu Agung Parama - 05111840000123

### Soal 1. Membuat topologi jaringan demi kelancaran TA-nya dengan kriteria sebagai berikut:
![image](https://user-images.githubusercontent.com/58405725/100545118-e42e9380-328c-11eb-8ec0-0d7416bba4b2.png)


### Soal 2. SURABAYA ditunjuk sebagai perantara (DHCP Relay) antara DHCP Server dan client.
Install dulu “apt-get install isc-dhcp-relay <br>
nano /etc/default/isc-dhcp-relay<br>
Lalu atur konfigurasi seperti gambar dibawah <br>
![image](https://user-images.githubusercontent.com/58405725/100545138-058f7f80-328d-11eb-920a-1a14d4313b99.png)
berdasarkan config yang dibuat dia akan men-forward request ke IP TUBAN dan melayani eth1, eth2, dan eth3.

### Soal 3-6. Client pada subnet 1 mendapatkan range IP dari 192.168.0.10 sampai 192.168.0.100 dan 192.168.0.110 sampai 192.168.0.200. .Client pada subnet 3 mendapatkan range IP dari 192.168.1.50 sampai 192.168.1.70..Client mendapatkan DNS Malang dan DNS 202.46.129.2 dari DHCP. Client di subnet 1 mendapatkan peminjaman alamat IP selama 5 menit, sedangkan (6) client pada subnet 3 mendapatkan peminjaman IP selama 10 menit.
Menentukan interface eth0 untuk diberikan layanan DHCP. Konfigurasi pada DHCP Server TUBAN seperti gambar dibawah. <br>
![image](https://user-images.githubusercontent.com/58405725/100545211-569f7380-328d-11eb-825d-6c23bae75282.png)
Konfigurasi pada masing-masing subnet untuk mendapatkan Peminjaman IP, DNS Server serta waktu peminjamannya <br>
![image](https://user-images.githubusercontent.com/58405725/100545252-83538b00-328d-11eb-9667-1120ec00c8d9.png)
Contoh hasil test di client : <br>
##### MADIUN <br>
![image](https://user-images.githubusercontent.com/58405725/100545273-9b2b0f00-328d-11eb-8e44-fad1c779ef5f.png)


### Soal 7. User autentikasi milik Anri memiliki format: ● User : userta_yyy ● Password : inipassw0rdta_yyy Keterangan : yyy adalah nama kelompok masing-masing.
![image](https://user-images.githubusercontent.com/58405725/100545358-0e348580-328e-11eb-8d13-1306b8476d25.png)
Gambar config untuk no 7,8,9,10,11
<br>
Langkah pertama yaitu Install `apt-get install apache2-utils` <br>
Buat user dan password baru, ketikkan `htpasswd -c /etc/squid3/passwd userta_E12`
New password `inipassw0rdta_E12` kemudian re-type password <br>
Kemudian agar akses proxy hanya bisa dilakukan oleh anri sebagai user, tambahkan config ini pada file config squid
<br>
```
auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED
http_access allow USERS
```

### Soal 8. Setiap hari Selasa-Rabu pukul 13.00-18.00. Bu Meguri membatasi penggunaan internet Anri hanya pada jadwal yang telah ditentukan itu saja. Maka diluar jam tersebut, Anri tidak dapat mengakses jaringan internet dengan proxy tersebut. Jadwal bimbingan dengan Bu Meguri adalah
Konfigurasi pada acl.conf untuk menentukan waktu aksesnya.
<br>
```
acl AVAILABLE_WORKING time TW 13:00-18:00
```
kemudian menambahkan include file acl.conf tadi agar terbaca di config squid.


### Soal 9. Setiap hari Selasa-Kamis pukul 21.00 - 09.00 keesokan harinya (sampai Jumat jam 09.00). Agar Anri bisa fokus mengerjakan TA.
Sama seperti sebelum hanya seperti ini: <br>
```
acl Bimbingan_1 time TWH 21:00-24:00
acl Bimbingan_2 time WHF 00:00-09:00
```
kemudian untuk mengaktifkannya gunakan perintah <br> 
```
http_access allow AVAIBLE_WORKING
http_access allow Bimbingan_1
http_access allow Bimbingan_2
```
### Soal 10. Setiap dia mengakses google.com, maka akan di redirect menuju monta.if.its.ac.id agar Anri selalu ingat untuk mengerjakan TA🙂. Untuk menandakan bahwa Proxy Server ini adalah Proxy yang dibuat oleh Anri.
Lakukan konfigurasi pada squid.conf seperti ini : <br>
```
acl pindah url_regex .google.com
http_access deny pindah
deny_info monta.if.its.ac.id
```
artinya dia akan mendetect url google.com kemudian akan memblock access-nya dan menggantinya dengan halaman monta.if.its.ac.id
### Soal 11. Bu Meguri meminta Anri untuk mengubah error page default squid
Download file ERR_ACCESS_DENIED terlebih dahulu lalu copy ke `/usr/share/squid/errors/templates`. Setelah itu error_directory nya diarahkan ke directory tadi.<br> 
```
error_directory /usr/share/squid/errors/templates
```

### Soal 12. Karena Bu Meguri dan Anri adalah tipe orang pelupa, maka untuk memudahkan mereka, Anri memiliki ide ketika menggunakan proxy cukup dengan mengetikkan domain janganlupa-ta.yyy.pw dan memasukkan port 8080. Keterangan : yyy adalah nama kelompok masing-masing. Contoh: janganlupa-ta.c01.pw
Setting dns di MALANG agar janganlupa-ta.E12.pw mengarah ke MOJOKERTO. <br>
![image](https://user-images.githubusercontent.com/58405725/100545901-e98ddd00-3290-11eb-94bc-60fdf1245a92.png)
![image](https://user-images.githubusercontent.com/58405725/100545952-2bb71e80-3291-11eb-9a9e-9d13e91c5df2.png)
<br>
kemudian coba ping ke domain tersebut menggunakan uml client.
![image](https://user-images.githubusercontent.com/58405725/100545963-396ca400-3291-11eb-9be3-d14f617bc8f8.png)
Jika sudah maka untuk proxy kita sudah bisa menggunakan domain yang dibuat tadi sebagai pengganti IP Mojokerto.








