# Labmobile8_DanielAbdillahArif_ShiftC
# CARA KERJA LOGIN

# A. Pengelolaan API Login

1. koneksi.php

![](Screenshot(270).png)

File ini berfungsi untuk membuat koneksi ke database agar bisa diakses dari sumber berbeda. Berikut adalah detailnya:
- Access-Control-Allow-Origin: *: Mengizinkan akses dari semua asal (domain).
- Access-Control-Allow-Credentials: true: Mengizinkan pengiriman kredensial (seperti cookie) dalam permintaan lintas domain.
- Access-Control-Allow-Methods: Mendefinisikan metode HTTP yang diizinkan, yaitu PUT, GET, HEAD, POST, DELETE, dan OPTIONS.
- Content-Type: application/json; charset=utf-8: Menentukan bahwa konten yang dikirim adalah dalam format JSON dan menggunakan UTF-8.
- Access-Control-Allow-Headers: Mengizinkan header tertentu seperti X-Requested-With, Content-Type, Origin, Authorization, dll., yang umumnya dibutuhkan oleh aplikasi web modern.
- Kemudian menggunakan mysqli_connect untuk menghubungkan aplikasi ke database coba-ionic di localhost dengan username root dan tanpa password. Jika koneksi gagal, maka akan muncul pesan “koneksi gagal”.

2. login.php

![](Screenshot(271).png)

File ini bertanggung jawab untuk melakukan proses autentikasi pengguna berdasarkan input yang dikirimkan oleh pengguna. Berikut adalah detailnya:
Pertama, mengimpor file koneksi.php untuk menghubungkan ke database. Kemudian, mengambil data yang dikirim oleh pengguna dalam format JSON dan mengonversinya menjadi array. Setelah itu, kode mengekstraksi username dan password dari data tersebut, membersihkan spasi pada kedua nilai, dan mengenkripsi password menggunakan MD5. Kode ini kemudian menjalankan query SQL untuk mencari kecocokan username dan password pada tabel user. Jika ditemukan kecocokan, ia mengambil data pengguna dari database, menyimpan username pengguna ke dalam variabel $pesan, membuat token sederhana menggunakan timestamp dan hash password, serta menetapkan status_login menjadi "berhasil". Jika tidak ada kecocokan, status_login diatur ke "gagal". Akhirnya, hasil login dikirim kembali ke pengguna dalam format JSON, dan jika terjadi kesalahan pada query SQL, pesan error dari database akan dicetak.

# B. Persiapan Projek Ionic

1. Mendeklarasikan provideHttpClient pada app.module.ts

![](Screenshot(272).png)

Pertama, modul dan layanan penting diimpor, termasuk BrowserModule untuk aplikasi web, IonicModule dengan strategi rute khusus IonicRouteStrategy, dan provideHttpClient untuk komunikasi HTTP. Modul ini mendeklarasikan AppComponent sebagai komponen utama, mengimpor modul yang dibutuhkan (BrowserModule, IonicModule, dan AppRoutingModule untuk pengaturan rute aplikasi), dan menyediakan RouteReuseStrategy untuk mengatur strategi penggunaan ulang rute. Akhirnya, AppComponent digunakan sebagai komponen awal yang diload.

![](Screenshot(273).png)
![](Screenshot(274).png)

Kode di atas mendefinisikan AuthenticationService untuk autentikasi dalam aplikasi Angular berbasis Ionic. Kelas ini menggunakan HttpClient untuk permintaan HTTP dan AlertController untuk menampilkan notifikasi. Konstanta TOKEN_KEY dan USER_KEY digunakan sebagai kunci penyimpanan data autentikasi pada Preferences, alat penyimpanan lokal. Variabel isAuthenticated berfungsi sebagai penanda autentikasi dengan status awal false, dan authenticationState mengawasi perubahan status autentikasi. Metode saveData menyimpan token dan data pengguna secara lokal serta mengubah status autentikasi menjadi true. Metode loadData memuat data autentikasi yang tersimpan, sementara clearData menghapus data tersebut. postMethod mengirim permintaan HTTP POST ke API yang ditentukan dalam apiURL, yang disetel ke 'http://localhost/coba-login'. Metode notifikasi menampilkan pesan dalam bentuk peringatan, dan logout menghapus status autentikasi serta data yang tersimpan.

![](Screenshot(275).png)

Kode di atas mendefinisikan fungsi authGuard yang berfungsi sebagai guard untuk melindungi rute di aplikasi Angular, memastikan hanya pengguna yang terautentikasi yang dapat mengaksesnya. authGuard menggunakan AuthenticationService untuk memeriksa status autentikasi pengguna dan Router untuk navigasi. Fungsi ini memantau authenticationState dari AuthenticationService dan memfilter nilai null dari aliran data, mengambil hanya satu nilai untuk memproses status autentikasi saat ini. Jika pengguna terautentikasi, fungsi mengizinkan akses ke rute (mengembalikan true); jika tidak, pengguna akan diarahkan ke halaman login (/login) dengan replaceUrl diatur ke true untuk mengganti riwayat navigasi.

![](Screenshot(276).png)

Kode di atas mendefinisikan autoLoginGuard, sebuah route guard untuk aplikasi Angular yang mengecek status autentikasi pengguna sebelum masuk ke halaman tertentu. autoLoginGuard menggunakan AuthenticationService untuk memantau status autentikasi melalui authenticationState. Guard ini mengimpor inject untuk menyuntikkan AuthenticationService dan Router, kemudian memfilter nilai null dalam aliran status autentikasi dan mengambil satu nilai saja untuk diproses. Jika pengguna sudah terautentikasi, autoLoginGuard mengarahkan mereka langsung ke halaman /home, menggantikan riwayat URL sebelumnya. Jika pengguna belum terautentikasi, guard tetap mengizinkan akses ke halaman yang dituju tanpa mengubah rute, mengembalikan true untuk melanjutkan navigasi.

2. Menambahkan Guard pada app/app-routing.module.ts

![](Screenshot(277).png)

Kode di atas mengatur rute utama untuk aplikasi Angular dengan modul AppRoutingModule. Pada konfigurasi rute (routes), terdapat beberapa pengaturan: rute /home memuat modul HomePageModule secara dinamis dan dilindungi oleh authGuard, yang memastikan hanya pengguna yang terautentikasi dapat mengakses halaman ini. Rute '' mengarahkan ulang pengguna ke halaman login jika mereka mengakses root (/) dari aplikasi. Rute /login memuat modul LoginPageModule secara dinamis dan dilindungi oleh autoLoginGuard, yang akan mengarahkan pengguna ke halaman home jika mereka sudah terautentikasi. RouterModule diatur untuk memuat modul secara cepat menggunakan strategi PreloadAllModules untuk meningkatkan performa aplikasi.

3. Membuat halaman login pada app/login/login.page.html dan atur pengolahan login pada app/login/login.page.ts

![](Screenshot(278).png)

Kode di atas adalah antarmuka halaman login menggunakan Ionic. Sebuah ion-card digunakan sebagai wadah formulir login, berisi dua ion-item untuk memasukkan username dan password dengan label di atas input, masing-masing memiliki properti [(ngModel)] untuk mengikat data ke variabel username dan password. ion-button bertindak sebagai tombol login dengan warna merah dan diperluas secara penuh (expand="block"), serta memicu fungsi login() saat diklik. Tampilan ini menyajikan antarmuka login yang sederhana dan responsif.

![](Screenshot(279).png)

Kode di atas mendefinisikan komponen LoginPage untuk halaman login di aplikasi Angular. Variabel username dan password digunakan untuk menyimpan input pengguna. Pada metode login(), aplikasi memeriksa apakah username dan password tidak kosong; jika ada input, objek data yang berisi username dan password dikirim melalui postMethod dari AuthenticationService ke endpoint login.php. Jika respons dari server menunjukkan login berhasil (status_login == "berhasil"), data autentikasi (token dan username) disimpan menggunakan saveData, dan pengguna diarahkan ke halaman /home. Jika login gagal, pengguna menerima notifikasi dengan pesan bahwa "Username atau Password Salah". Jika terjadi kesalahan jaringan, pengguna mendapatkan notifikasi "Login Gagal Periksa Koneksi Internet Anda". Jika salah satu input kosong, pengguna diberi notifikasi "Username atau Password Tidak Boleh Kosong".

4. Membuat halaman home pada app/home/home.page.html dan mengatur pengolahan data home pada app/home/home.page.ts

![](Screenshot(280).png)

Kode diatas merupakan kode antarmuka halaman utama (Home) di aplikasi Ionic.Pada ion-content, yang memiliki properti fullscreen, terdapat item logout (ion-item) yang dilengkapi ikon exit dan label "Logout". Saat ion-item ini diklik, fungsi logout() akan dipanggil. Di bawah item logout, ada teks "Selamat datang" yang menampilkan nama pengguna ({{ nama }}) yang diambil dari variabel nama. Halaman ini sederhana dan berfungsi untuk menyambut pengguna serta menyediakan opsi logout dengan ikon dan label yang mudah diakses.

![](Screenshot(281).png)

Kode di atas adalah komponen Angular bernama HomePage yang bertindak sebagai halaman beranda dalam aplikasi. Komponen ini memiliki properti nama, yang nilainya diambil dari AuthenticationService. AuthenticationService sendiri merupakan layanan yang mengelola proses otentikasi pengguna, termasuk menyediakan nama pengguna yang telah login. Pada bagian constructor, nama pengguna disimpan dalam properti nama. Metode logout() digunakan untuk melakukan proses logout dengan memanggil metode logout dari AuthenticationService dan mengarahkan pengguna ke halaman login menggunakan Router.

# Tampilan Aplikasi

![](Screenshot(267).png)

![](Screenshot(268).png)

![](Screenshot(269).png)