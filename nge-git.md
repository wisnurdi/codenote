#BISMILLAH

Dengan ini kami merubah readme.md

Teman-teman, ternyata bekerja dengan git begitu mudah dan enak. Bagi Anda yang belum tahu, mungkin harus mencobanya demi kebaikan kita.

Syaratnya, kita sudah menginstal git di komputer kita.

Berikut adalah langkah-langkahnya:

1. Buat sebuah folder dan jalankan terminal pada folder tersebut. Selanjutnya, jalankan perintah berikut `git init`
2. Berikutnya, kita bisa menambah file-file, bisa langsung copy paste file ataupun melalui command line juga. Misalnya ketikkan berikut `echo "file Readme">> README.md`
3. Selanjutnya, tambahkan file tersebut sebagai bagian dari repository kita dengan perintah `git add README.md`
4. Adapun jika filenya banyak, baik baru nambah atau update file-file yang sudah-sudah, kita bisa ketik perintah `git add .`
5. Jangan lupa menjalankan perintah `commit` dengan mengetikkan keterangan commit-nya, yakni `git commit -m "keterangan commitnya apa"`
6. Nah, selanjutnya, jika repo di github yang kita miliki masih kosong, kita bisa menambahkan file-file dengan perintah berikut `git remote add origin http://github.com/wisnurdi/bismillah.git`. Kalau bukan kosongan, ya Anda akan mengalami error.
7. Terakhir, kita lakukan `push` dengan perintah berikut: `git push -u origin master`. Pada bagian ini kita akan dimintai username dan password kita di github. Perhatikan, jika perintah `push` ditolak, kemudian dia minta agar lakukan `pull`, ya lakukan perintah `pull` dulu. Jadi, jalankan dulu `git pull -u origin master` 