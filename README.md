# hackathon-excel-dqlab-2026

## Hackathon Overview

### Versi DQLab (Penyelenggara Hackathon)

*source: https://academy.dqlab.id/challenge/detail/536*

**Excel Hackathon: Build Inventory Analytics & Planning with Excel**
 
> "Berapa banyak barang yang harus kita pesan hari ini?"

Ini adalah pertanyaan sederhana yang bisa menentukan apakah bisnis kehabisan stok atau kehilangan modal di gudang.

Dalam Excel Hackathon 2026, peserta akan berperan sebagai Inventory Planning Analyst dan membangun Decision Support System menggunakan Microsoft Excel untuk membantu perusahaan mengambil keputusan inventory secara tepat. 

Peserta akan bekerja dengan ribuan data transaksi dan mengubah data tersebut menjadi insight yang dapat digunakan untuk menentukan keputusan pembelian. Berikut tugasnya:

- Process - Mengolah puluhan ribu transaksi secara otomatis dan terstruktur.
- Analyze - Mengidentifikasi kondisi inventory berdasarkan berbagai metrik yang relevan.
- Recommend - Menentukan produk yang perlu direkomendasikan untuk reorder maupun produk yang sebaiknya dihentikan pemesanannya.
- Support Decisions - Menghasilkan informasi yang dapat menjadi dasar pengambilan keputusan pembelian.


### Versi Pemahamanku
*untuk penjelasannya sendiri kutulis pakai kata-kata sendiri, supaya memorable kalau dibaca lagi di masa depan. kutulis setelah selesai mengerjakan hackathon secara menyeluruh.*

**Big Picture:** 
- **Pertanyaan utama** yang mau dijawab itu, berapa banyak barang yang perlu dioder lagi dan kapan tepatnya supaya ga kekurangan tapinya juga ga sampai bikin gudang kepenuhan yang ga bisa ditutup lagi pintunya.
- Supaya stok produknya **aman untuk sepanjang tahun 2026** ini, di sini peserta diminta untuk menghasilkan prediksi kapan dan berapa tepatnya masing-masing produk perlu diorder kembali.
- Untuk bisa memprediksi angka + tanggalnya, kita butuh data, yang mana **data mentah**nya sudah disediakan penyelenggara hackathon sbb:
    - Tanggal di mana terjadi permintaan (demand) produk beserta jumlah permintaannya selama Januari 2026. Produknya sendiri terdiri dari beberapa jenis, di sini istilahnya SKU untuk penyebutannya. SKU PROD A, SKU PROD B, sampai SKU PROD E. Data jumlah permintaan tercatat untuk masing-masing SKU nya. 
    - Kapasitas maksimal gudang untuk masing-masing SKU.
    - Initial stock masing-masing SKU yang tersedia di gudang.
- Ingat apa salah satu **syarat**nya? -> jangan sampai gudang kepenuhan, jangan sampai juga stok kosong / kekurangan dari jumlah permintaan. 
- Jumlah permintaan bisa jadi penentu kuat yang dapat dijadikan bahan prediksi. Kalau kita tahu jumlah permintaan yang kemungkinan terjadi beserta tanggalnya, kita jadi bisa prepare dong berapa tepatnya barang yang perlu di-reorder atau di-restock. Maka dari itu, kita buatlah **proyeksi jumlah permintaan sampai setahun ke depan** sampai akhir tahun, supaya data perkiraan jumlah permintaannya lengkap dan memadai untuk basis penentuan jumlah + tanggal reorder nya. 
- Gimana caranya? Caranya dengan memanfaatkan data mentah jumlah permintaan di bulan Januari yang sudah tercatat sebelumnya, lalu, gunakan rule proyeksi yang sudah ditentukan penyelenggara hackathon untuk melakukan proyeksi sampai akhir tahun (Desember 2026). 
- Janlup kita juga punya data **initial stock** di gudang per tanggal 1 Januari. Data itu juga perlu dijadikan tonggak awal perhitungan stok. Kek misal, di suatu titik data di tanggal tertentu, stok yang tadinya sejumlah initial stock jadi berkurang lantaran terdapat jumlah permintaan sebanyak sekian di tanggal ybs.
- Lalu sekarang kita punya angka proyeksi jumlah permintaan selama setahun ke depan, sekarang apa? Gimana kita bisa pakai itu untuk menentukan jumlah reordering beserta tanggal persisnya? 
- Caranya dengan memperhatikan aspek lead time (sejenis waktu tunggu barang hingga sampai ke gudang terhitung sejak tanggal pemesanan), serta kapasitas gudang itu sendiri. Untuk detailnya lihat di bagian selanjutnya, Raw Data & Tasks, sbb:


## Raw Data & Tasks 

- **sheet `Demand Projection`:** <br> <br> <img src="img/demand_projection_sheet.png" alt="Demand Projection sheet" width="400"> <br><br>
Keterangan:

    - Demand Projection ini sheet yang ditujukan untuk melakukan proyeksi jumlah permintaan di setiap tanggal yang sama seperti data sebelumnya, untuk setiap bulan sampai Desember 2026. 
    - Produknya ada 5 jenis: SKU PROD A, B, C, D, E.
    - Task nya adalah mengisi proyeksi jumlah permintaan di bulan selanjutnya untuk tanggal yang sama seperti bulan sebelumnya, mengikuti rule proyeksi yang ditentukan di instruksi. Isi di kolom Jumlah.
    - Rule proyeksinya: <br><img src="img/rule_proyeksi_1.jpg" alt="rule proyeksi 1" width="400"><br><img src="img/rule_proyeksi_2.jpg" alt="rule proyeksi 2" width="400">
    - Nantinya hasil proyeksi ini akan digunakan sebagai basis perhitungan stok minimal dan maksimal yang mungkin di gudang untuk masing-masing SKU, di sheet Capacity. 
    - Yang mana, hasil tersebut juga akan digunakan sebagai basis perhitungan untuk penentuan tanggal reorder produk di sheet Warehouse.

<br>

- **sheet `Warehouse`:** <br> <br> <img src="img/warehouse_sheet.png" alt="Warehouse sheet" width="400"> <br> <br>
Keterangan:
    - Sheet Warehouse, versi raw dari sana nya terdiri dari 5 kolom: #, Item / SKU, Date, Stock Qty, dan Status. Baris data raw: baris 2 sampai 5, yang mana seluruhnya merupakan data Initial Stock. 
    - Untuk seterusnya, kolom Status isiannya adalah Reorder, yang mana merupakan jumlah kuantitas produk yang perlu direorder sesuai ketentuan.
    - **Ketentuan reorder:** dilakukan jika jumlah order dari kebutuhan / demand pada tanggal tersebut di  bawah angka minimal inventory level dari produk tersebut. Ketika keadaan tersebut terjadi, reorder harus dilakukan dengan jumlah untuk mencapai inventory level maksimal dari produk tersebut. 
    - Kotretan perhitungan kuantitas reorder beserta gambaran terkait kapan persisnya informasi Stock Qty nya diupdate, untuk case SKU PROD A:  <br> <img src="img/reorder-scratch.jpg" alt="scratch reorder" width="400"> <br> 
    - Lalu pada sheet ini juga diminta untuk membuat kolom baru: **`Arrival Date`** dan **`Kapasitas Gudang`**.
    - Arrival Date diisi dengan tanggal sampainya barang yang sudah diorder ke gudang. Durasi setiap pengiriman adalah 3 hari terhitung sejak tanggal pada kolom Date pada baris data yang sama.
    - Contoh perhitungan kapasitas gudang, untuk case initial stock: <br> <img src="img/kapasitas_gudang.jpg" alt="scratch kapasitas gudang initial stock" width="400"> <br> 
    - Rule perhitungan kapasitas gudang: hitung stok SKU Produk A s.d. stok SKU Produk E di gudang  pada tanggal arrival dikurangi demand pada saat arrival, dibandingkan dengan informasi kapasitas gudang yang ada pada sheet Capacity. Lakukan perhitunganya untuk masing-masing SKU dulu, baru setelah angka masing-masingnya sudah ada, jumlahkan, lalu isi untuk masing-masing SKU dengan arrival date yang sama.


<br>

- **sheet `Capacity`:** <br> <br>
![Capacity sheet](img/capacity_sheet.png) <br><br>
Keterangan:
    - Sheet Capacity terdiri dari dua tabel: Warehouse Capacity dan Min/Max Stock Level.
    - Untuk tabel Warehouse Capacity sudah ditentukan penyelenggara hackathon dari sananya. 
    - Yang perlu dihitung peserta adalah kolom Min dan Max di tabel Min/Max Stock Level untuk setiap SKU.
    - Perhitungannya dilakukan menggunakan jumlah permintaan yang sudah diproyeksikan sebelumnya di sheet Demand Projection.
    - Rule nya, untuk setiap titik data dengan tanggal yang ada di kolom Tanggal Order sheet Demand Projection, hitung total permintaan SKU yang sama selama 6 hari ke depan. 
    - Lalu dari setiap hasil perhitungan total permintaan di setiap tanggal order itu, untuk masing-masing SKU ditentukan nilai max dan min nya, dan nilai itulah yang dimasukkan ke kolom Min dan Max di sheet Capacity, sesuai SKU nya masing-masing. 
    - Rule juga nih, untuk titik data yang ada di tanggal yang datanya ga ada lagi di 6 hari ke depan, jangan diikutkan di max min. 
    - Dalam hal ini, karena data terakhir ada di tanggal 28 Desember 2026, artinya titik data terakhir yang boleh diikutkan ke perhitungan min max adalah titik data yang ada di tanggal sebelum 23 Desember 2026.
    
    <br>

## Solution

*Well, generally . . .*

- Solusi inti dari hackathon ini pada dasarnya adalah kemampuan diri dalam memahami permainan kata.
- Tidak ada analisis data tingkat tinggi yang diperlukan untuk menyelesaikan seluruh task nya. Tugasnya hanya sekedar memahami instruksi, lalu eksekusi sesuai instruksi, dengan tetap memastikan bahwa tidak ada perbedaan pemahaman antara pemberi instruksi dengan penerima instruksi.
- Jika pemahaman antara pemberi dan penerima instruksi sudah dipastikan sama, sisanya tinggal serahkan pekerjaan 'administratif' ke AI. Lalu pastikan, apakah formula yang dihasilkan sudah sesuai dengan yang diekspektasikan atau belum.
- Jikapun masih belum terlalu sering pakai excel dan masih rada kagok dengan function bawaannya, tinggal baca saja penjelasannya di Help yang sudah disediakan Microsoft Excel 365. Atau, kalau butuh gaya ngobrol, ya balik ke AI lagi minta jelasin pakai bahasa yang mudah dimengerti bocil 0 tahun.
- Tapi penjelasan di Help itu saja pun sudah cukup. Penjelasannya mudah dicerna, ada contoh case nya juga. Jadi, untuk perkara kagok function bukan masalah besar. Lagian kalau udah sering dipakai dan udah sering ketemu, lama-lama function-function itu dengan sendirinya terekam juga di luar kepala.
- Untuk solusi akhir dapat dilihat di attempt terakhir sebagai berikut: https://github.com/dinanabila/hackathon-excel-dqlab-2026/tree/main/07


## Mischellaneous

### Pre Thoughts

*Pre thoughts sebelum mulai ikut hackathon ini:*

Tujuan ikutan hackathon ini:
- Mencicip dunia supply chain, inventory stock. 
- Me-refresh skill excel. 
- Upaya menambah kompetensi untuk peluang opportunity yang akan datang. 

Plan alur berpikir buat nyelesaiin hackathon ini:
- Baca dan pahami pdf task hackathon nya. 
- Pikirkan dulu solusinya secara mandiri sebelum melibatkan AI. 
- Setelah dapat raw sketch dan big picture nya, berangkat ke AI, ngobrol. 
- Aplikasikan rumus excel hasil rundingan dengan AI. 
- Submit.

My subjective random thoughts during hackathon:
- Mengingat rule yang membebaskan peserta untuk menggunakan AI, maka melihat leaderboard jadi kurang apple-to-apple dijadikan tolok ukur untuk membandingkan progres diri buatku. Terlebih saat ini aku lebih memprioritaskan pematangan konsep secara keseluruhan terlebih dahulu, baru kemudian mengejar kecepatan pengerjaan.
- Yang oleh karenanya, leaderboard di hackathon kali ini sejatinya kebanyakannya lebih berfungsi sebagai pembanding seberapa bagus prompt dan seberapa cepat AI yang digunakan peserta. Saat ini aku belum dalam kondisi seperti itu, masih dalam fase memahami dan memaknai.
- Jadi di hackathon ini, fokusnya lebih ke mindset kalau di sini aku berkompetisi terhadap diriku sendiri, dengan caraku sendiri, dengan pace ku sendiri. 


### Mid Thoughts

*thought after doing 6th attempt during hackathon*:

Sooo in the end, the fastest solution in the end is training agent, then?

<img src="img/training-agent.jpg" alt="Warehouse sheet" width="500"> <br>

Training agent? Nah nah. This world seems determined to let **s**~~tu~~**peed**~~ity~~ guide humans through a life stripped of color and challenge. The only challenge left is prompting and proofreading, like an artist who’s no longer touch canvas, paints, nor brushes, only guide and see and guide and see.

Berusaha memahami di tengah derasnya arus kecepatan itu, sebenarnya sulit, dan menyesakkan. 

Tapi, meski begitu, aku tetap ingin memahami, aku tetap ingin mengerti, 

dan aku tetap akan berproses. 

### After Thoughts

*thought after doing this hackathon case mindfully:* 

Menarik siii, case ini. Jadi tau, apasi basis yang diperlukan sebagai penentu kapan dan berapa tepatnya barang yang kita jual harus direorder / direstock kembali tanpa harus menuhin gudang dan tanpa harus kena masalah 'yah mohon maaf stoknya lagi kosong kak'. 

Untuk analisis data, menurutku itu ga disediakan di hackathon ini. Karenaaa, rule nya sendiri udah ditentukan tanpa harus bikin peserta mikir lagi (lihat lagi rule perhitungan kapasitas gudang, sama, rule proyeksi demand yang amat sangat detail itu kwwkw). 

Padahal, justru di situ seninya, di penentuan angka di setiap rule nya. Mungkin kalau di lapangannya rule kayak gitu sebenarnya hasil kerjaan anak-anak data science. 

Mungkin.