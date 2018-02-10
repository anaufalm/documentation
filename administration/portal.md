# Portal

Portal menyediakan kemampuan untuk mengakses data dan fungsi crm spesifik untuk pelanggan dan mitra Anda. Administrator dapat membuat beberapa portal. Setiap portal bisa memiliki pengaturan sendiri, dasbor, daftar pengguna, pengaturan akses kontrol.

Untuk membuat portal ikuti Administrasi> Portal, klik tombol Buat Portal.

* `Aktif`. Jika tidak cek portal tidak akan tersedia bagi siapa saja.
* `Default`. Berarti portal itu akan tersedia dengan tautan yang lebih singkat: http(s)://TAUTAN_ESPO_ANDA/portal.
* `Peran`. Tentukan satu atau beberapa peran portal yang akan diterapkan pada pengguna yang masuk ke portal. Informasi lebih lanjut tentang peran portal ada di bawah ini.
* `Daftar Tab`. Tab yang akan ditampilkan di bilah navigasi.
* `Tata letak Dasbor`. Tentukan dasbor yang akan ditampilkan di halaman awal portal. Perhatikan bahwa pengguna portal tidak dapat mengonfigurasi dasbor mereka.
* `Tautan`. Bidang baca-saja yang menampilkan link yang bisa anda akses dengan portal.

## Pengguna Portal

Administrator dapat membuat pengguna portal.

1. Administrasi > Pengguna.
2. Klik kanan tarik-ulur di sebelah Buat pengguna.
3. Klik Buat User Portal.
4. Pilih Hubungi pengguna portal yang akan ditautkan.
5. Isi form dan klik simpan.

Pengguna portal harus terhubung ke catatan Portal agar dapat mengakses portal tersebut.

## Peran Portal

Peran portal mirip dengan peran reguler di EspoCRM namun dengan beberapa perbedaan.

* Tingkat `tidak-diatur` menyangkal terhadap akses.
* Level `sendiri` berarti rekaman dibuat oleh pengguna. Misalnya pengguna portal mengumpulkan beberapa kasus dan kasus ini dimiliki oleh pengguna ini.
* Level `akun` berarti bahwa catatan tersebut terkait dengan akun yang terkait dengan pengguna portal.
* Level `kontak` berarti record berhubungan dengan kontak yang berhubungan dengan pengguna portal.

Bidang Pengguna and Tim yang ditugaskan hanya untuk pengguna portal.

### Contoh

`Pengguna portal harus dapat membuat kasus, melihat kasus yang terkait dengan akun mereka; mereka harus bisa melihat basis pengetahuan.`

1. Buka formulir Buat Peran Portal (Administrasi> Peran Portal> Buat Peran).
2. Aktifkan akses ke Kasus, atur: `buat - ya, baca - akun, ubah - tidak, hapus - tidak, siaran - akun`.
3. Aktifkan akses ke Basis Pengetahuan, atur `buat - tidak, baca - akun, ubah - tidak, hapus - tidak`.
4. Ubah catatan portal Anda (Administrasi> Portal). Pilih peran portal Anda di bidang Peran dan kemudian simpan.

## Akses ke Portal

Anda dapat menemukan tautan untuk portal Anda di bidang 'Tautan' dari catatan portal. Juga mungkin untuk menggunakan alat konfigurasi server (seperti mod_rewrite) untuk dapat mengakses dengan tautan yang berbeda. Untuk kasus ini, Anda perlu mengisi kolom 'Tautan Khusus'.

### Akses portal dengan Tautan Kustom untuk server Apache

Tautan Khusus: nama-host-portal-saya.com

#### crm.portal.conf
```
<VirtualHost *:80>
	DocumentRoot /path/to/espocrm/instance/
	ServerName my-portal-host-name.com

    <Directory /path/to/espocrm/instance/>
        Options Indexes FollowSymLinks
        AllowOverride All
        Order allow,deny
        allow from all
    </Directory>

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

ServerAlias my-portal-host-name.com

```

#### Mod menulis ulang peran

Tentukan ID catatan ganti dengan `{PORTAL_ID}`. ID catatan utama dapat tersedia di bilah alamat browser web Anda saat Anda membuka detail tampilan catatan portal. Seperti: https://my-espocrm-url.com/#Portal/16b9hm41c069e6j24. 16b9hm41c069e6j24 adalah id rekam portal.

```
  RewriteCond %{HTTP_HOST} ^portal-host-name.com$
  RewriteRule ^client - [L]

  RewriteCond %{HTTP_HOST} ^portal-host-name.com$
  RewriteCond %{REQUEST_URI} !^/portal/{PORTAL_ID}/.*$
  RewriteRule ^(.*)$ /portal/{PORTAL_ID}/$1 [L]
```
