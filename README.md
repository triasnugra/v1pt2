Aku baca ulang `engine.mjs` dulu karena aku perlu tahu persis bagaimana setup Monday High/Wolf/Double Deviation dibuat sebelum mengubah apa pun.Aku sudah paham di mana akar masalahnya di kode. Sebelum mengubah apa pun, aku lihat dulu bagian `scan` di `index.html` karena perubahan engine harus tetap kompatibel dengan pemindaian.Itu cuma error shell di baris `diff` (sintaks bash di `sh`), bukan di patch-nya. Aku cek ulang terpisah.Sebelum kamu deploy, aku hitung dulu angka **v1 dengan penilaian yang adil**, memakai data yang kamu tempel. Penilaiannya kupakai ulang dari fungsi `grade` yang sama.Brace expansion tidak jalan di `sh`, jadi `cp` gagal dan script tidak terbuat. Kuulangi dengan perintah `cp` terpisah.Skor v1 dengan penilaian yang adil adalah **50%**, bukan 47%. Jadi kelemahan v1 itu nyata dan bukan artefak penilaianku. Sekarang tab Uji dibuat menampilkan v1 dan v2 berdampingan.Terakhir, jalankan seluruh pipeline lagi dengan server Binance palsu untuk memastikan jalur dua varian, penilaian, dan statistik tidak crash.Poin 1 dan 2 sudah jadi, plus tiga tambahan. Engine v2 belum diuji dengan data Binance asli, jadi belum ada bukti bahwa v2 lebih baik dari v1. Angka itu baru muncul setelah kamu menjalankannya.

## Poin 1: penilaian yang adil

Aku memakai ulang data v1 yang kamu tempel dengan aturan baru. Hasilnya skor v1 jadi **50%**, bukan 47%, jadi kelemahan v1 itu nyata dan bukan artefak penilaianku.

Aturan barunya:
- Order tunda di ekstrem range tidak lagi dianggap bertentangan dengan Eliz yang "menunggu ekstrem" (BTC-0827 sekarang "sama").
- Short aktif tepat di bawah harga saat Eliz bullish tetap dianggap "beda" (ETH-0717).
- BTC-0729 kuubah jadi setup long. Itu keputusan penilaianku sendiri, karena chart-nya menunjukkan sweep lalu pantulan dan post 30 Juli menyebutnya "rencana sempurna". Datasetnya kutandai sebagai inferensi.

## Poin 2: akar masalah short palsu

Di v1, "Sweep Monday High" menyala kalau harga pernah di atas Monday High kapan saja sepanjang minggu itu, bukan sapuan yang baru terjadi. Itulah asal short di SOL-0815, LTC, dan BTC-0827.

v2 hanya menghitung sapuan dalam 48 jam terakhir. Di skenario uji, sapuan 68 jam lalu menyala di v1 dan mati di v2, sedangkan sapuan 16 jam lalu menyala di keduanya.

## Tambahan di engine v2

- **Tengah range** kini diukur terhadap Monday Range. Sebelumnya BTC 28 Juli disebut "tengah range" padahal harga hanya 0,3% di atas Monday Low.
- **Breakout trendline** adalah pola baru: dua close melewati garis, breakout masih baru, harga belum jauh dari garis. Ini muncul di 5 dari 21 post Eliz (ALICE, TURBO, FLOKI, FET, SOL) dan v1 tidak punya pola itu.
- **Kotak S/R weekly** dihitung dari pivot weekly yang dihormati berulang kali. Ini belum tampil di UI, hanya masuk ke penilaian level.
- Setup yang melawan struktur harian kini mendapat peringatan.

## Cara kamu mendapat angka pembanding

1. Deploy empat file: `engine.mjs`, `validate.mjs`, `eliz-dataset.json`, dan `index.html`. `engine.mjs` juga berubah sekarang.
2. Di tab Uji, jalankan uji dataset lalu uji pembanding acak. Pembanding acak sekarang 100 sampel dan menjalankan v1 dan v2 pada jam yang sama.
3. Tab itu menampilkan skor, level, simulasi plan, dan baseline acak untuk v1 dan v2 berdampingan.
4. Kolom baru median dan "tanpa trade terbaik" ada supaya satu trade besar seperti PHA +29R tidak menipu rata-rata.
5. Tempel hasilnya ke chat ini.

## Yang sudah kuuji

- Mode `legacy` identik dengan v1 asli pada 60 dari 60 data sintetis (44 punya setup), jadi perbandingan adil.
- Sapuan basi vs baru, breakout trendline, tengah range terhadap Monday Range, dan level weekly semuanya berperilaku sesuai niat pada skenario buatan.
- Seluruh pipeline dua varian jalan tanpa crash dengan server Binance palsu. Itu hanya membuktikan alur kodenya, bukan kualitas hasilnya.

## Peringatan

- **Angka ambangnya tebakan.** Jendela 48 jam, batas "aktif" 0,5%, dan jarak breakout 2 ATR belum diuji pada data asli. Kalau hasilnya buruk, aku ubah dari angka itu.
- **Perkiraan hasil nyata.** Short palsu SOL-0815, LTC, dan SOL-0625 seharusnya hilang, tapi itu baru bisa jadi "tidak terdeteksi", bukan otomatis jadi long yang benar. Mendapat setup long yang benar butuh pola baru.
- **Yang sengaja tidak kuubah.** Lebar SL, Wolf, dan Double Deviation tidak kusentuh. XPL kena SL satu jam setelah entry lalu naik 30%, tapi satu kasus tidak cukup untuk melonggarkan SL tanpa data.
- **Sampel kecil.** 18 entri dan 100 sampel acak tetap menghasilkan angka yang berisik. Selisih satu-dua poin antara v1 dan v2 belum berarti apa-apa.
