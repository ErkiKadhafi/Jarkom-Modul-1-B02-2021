# Jarkom-Modul-1-E02-2021

Berikut ini adalah laporan resmi dari Praktikum Jaringan Komputer Modul 1 tahun 2021 di Institut Teknologi Sepuluh Nopember

Dokumen ini ditulis oleh
* 05111940000063 - Ryan Garnet Andrianto
* 05111940000050 - Erki Kadhafi Rosyid
* 05111940000141 - M. Farhan Haykal

# 1
# 2
# 3
# 4
# 5
# 6 Cari username dan password ketika melakukan login ke FTP Server!

FTP (File Transfer Protocol) adalah sebuah protokol yang digunakan untuk transfer file antara server dan client.
Secara _default_, port yang digunakan oleh server FTP adalah 21. Dengan demikian, tim E02 menggunakan filter `ftp && tcp.port == 21` untuk mencari paket yang dikirimkan melalui FTP.

![image](https://user-images.githubusercontent.com/8071604/134525121-2c3b615b-a7c8-4700-a03d-9e310f87c1a3.png)

Pada gambar di atas, client mengirimkan perintah `USER secretuser`. `secretuser` adalah username yang digunakan oleh komputer client sebagai _credentials_ untuk mengakses FTP server.

Selain username yang terekspos, password `aku.pengen.pw.aja` juga terekspos. Hal ini dikarenakan paket yang dikirim menuju ke FTP server tidak dienkripsi sehingga _packet sniffing_ dengan mudah terjadi. 

![image](https://user-images.githubusercontent.com/8071604/134525894-dfc232bb-f2a1-46e4-8521-27a2a5a7bbea.png)


# 7 Ada 500 file zip yang disimpan ke FTP Server dengan nama 0.zip, 1.zip, 2.zip, ..., 499.zip. Simpan dan Buka file pdf tersebut. (Hint = nama pdf-nya "Real.pdf")

`Real.pdf` adalah sebuah string dengan panjang 8 karakter. Setiap karakter pada string tersebut mempunyai urutan pada tabel ASCII (American Standard Code for Information Interchange). Jika setiap nomor urutan karakter diubah menjadi bilangan heksadesimal, maka `Real.pdf` = `52 65 61 6c 2e 70 64 66`. 

![image](https://user-images.githubusercontent.com/8071604/134527095-7df15762-a848-4435-80f2-2cb9ff1b6462.png)

Pertama-tama, tim E2 melakukan filter `ftp-data` untuk mencari data paket yang menuju ke server FTP. Kemudian, dengan menggunakan fitur pencarian di _software_ Wireshark, paket yang mengandung string `52 65 61 6c 2e 70 64 66` dicari. Setelah ditemukan, dengan fitur TCP Stream yang ada di Wireshark, file rahasia berhasil ditemukan.

Isi dari file rahasia tersebut adalah
![image](https://user-images.githubusercontent.com/8071604/134527075-8975ce6e-2f59-42e2-8cfd-7b29a789aaf6.png)

# 8 Cari paket yang menunjukan pengambilan file dari FTP tersebut!

Pengambilan file dari FTP mempunyai istilah lain yaitu _download_. Perintah FTP yang digunakan untuk mendownload sebuah file adalah `RETR`. Dengan menggunakan filter `ftp.request.command == RETR`, semua paket yang didownload dari FTP akan tayang di Wireshark. Pada kasus ini, Wireshark tidak menampilkan paket apapun sehingga dapat disimpulkan bahwa tidak ada file yang diambil dari FTP server.

![image](https://user-images.githubusercontent.com/8071604/134527236-3ddf27b9-e00e-4550-b0c5-997918d1de2b.png)


# 9 Dari paket-paket yang menuju FTP terdapat inidkasi penyimpanan beberapa file. Salah satunya adalah sebuah file berisi data rahasia dengan nama "secret.zip". Simpan dan buka file tersebut!

Permasalahan ini mirip seperti permasalahan pada nomor 7. Akan tetapi, kali ini, tim E02 menggunakan filter lain yaitu `ftp-data.command contains secret.zip` untuk mencari perintah yang dikirimkan ke server FTP yang mengandung kata `secret.zip`. 

![image](https://user-images.githubusercontent.com/8071604/134527679-ec5c8e61-c68c-48c6-a271-7b568b40a640.png)

Setelah paket ditemukan, dengan fitur TCP Stream yang ada di Wireshark, file `secret.zip` dapat didownload. File `secret.zip` yang didownload terenkripsi oleh password. Pembukaan file `secret.zip` akan dilakukan pada nomor 10.

![image](https://user-images.githubusercontent.com/8071604/134527908-0553eb34-fa1a-40e5-a20a-b360896c2320.png)

# 10 Selain itu terdapat "history.txt" yang kemungkinan berisi history bash server tersebut! Gunakan isi dari "history.txt" untuk menemukan password untuk membuka file rahasia yang ada di "secret.zip"!

Dengan menggunakan filter `ftp-data`, semua paket data yang melalui FTP akan terekspos. Di sini, sebuah file bernama `history.txt` berisi beberapa bash command.

![image](https://user-images.githubusercontent.com/8071604/134528098-cd0f60a3-d385-4642-9288-ee46e6c45603.png)

Berikut ini isi dari history.txt
```bash
ls
key="$(tail -1 bukanapaapa.txt)"
zip -P $key secret.zip Wanted.pdf
rm Wanted.pdf
history | tail -5 > history.txt
```

Pada script bash di atas, diketahui bahwa sebuah password ada di dalam file `bukanapaapa.txt` dan password tersebut digunakan untuk men-zip sebuah file bernama Wanted.pdf. Setelah Wanted.pdf di-zip, file Wanted.pdf dihapus.

Isi dari `bukanapaapa.txt` juga terekspos sehingga password dapat diketahui.

![image](https://user-images.githubusercontent.com/8071604/134528555-97d83ee9-da3b-4f47-9764-172bde237b74.png)

Setelah itu, file `secret.zip` dibuka dengan menggunakan password yang tertulis di `bukanapaapa.txt`. Berikut ini adalah isi dari Wanted.pdf.

![image](https://user-images.githubusercontent.com/8071604/134528682-30f72698-6fa3-4711-be66-6b09ebb5b2df.png)


# 11
# 12
# 13
# 14
# 15
