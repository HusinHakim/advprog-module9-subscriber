# Penjelasan AMQP dan Koneksi RabbitMQ

## a. Apa itu AMQP?
AMQP (Advanced Message Queuing Protocol) adalah protokol komunikasi standar terbuka yang digunakan dalam middleware pesan. Protokol ini memungkinkan aplikasi dan sistem yang berbeda untuk bertukar data melalui pesan, terlepas dari bahasa pemrograman atau platform yang digunakan. AMQP dirancang untuk mendukung berbagai pola komunikasi seperti publish/subscribe, point-to-point, dan routing.

## b. Penjelasan string koneksi "amqp://guest:guest@localhost:5672"

- **guest pertama**: Username yang digunakan untuk autentikasi ke server RabbitMQ. Dalam kasus ini, menggunakan akun default "guest".

- **guest kedua**: Password untuk akun tersebut, juga menggunakan nilai default "guest".

- **localhost:5672**: Alamat dan port server RabbitMQ.
  - **localhost**: Menunjukkan bahwa server RabbitMQ berjalan di komputer lokal
  - **5672**: Port standar yang digunakan oleh AMQP untuk koneksi RabbitMQ

Secara keseluruhan, string "amqp://guest:guest@localhost:5672" adalah URL koneksi yang memberitahu aplikasi bagaimana cara terhubung ke server RabbitMQ yang berjalan di komputer lokal, menggunakan kredensial default.

## Simulasi Antrian Pesan saat Subscriber Lambat
![Queued Messages](images/chrome_e6RnKd8kAd.png)

Dalam realitas, terkadang program berjalan lambat sementara permintaan dari pengguna tinggi. Ingat saat Anda mengisi IRS setiap semester. Memproses IRS membutuhkan waktu komputasi, tetapi selama "SIAK War", begitu banyak mahasiswa mencoba mengisi secepat mungkin. Permintaan sangat tinggi, tetapi daya komputasi lambat. Jika ini terjadi, sistem dapat crash seperti yang kadang terjadi dengan SIAK.

Pada simulasi ini, subscriber dibuat lambat dengan menambahkan delay 1 detik untuk setiap proses. Hasilnya dapat dilihat pada screenshot di atas. Hal ini menunjukkan bahwa producer dapat terus mengirim permintaan, dan permintaan tersebut (sebagai event) ditempatkan dalam antrian pesan. Secara perlahan, consumer akan memprosesnya satu per satu.

Pada grafik "Queued messages", terlihat lonjakan jumlah pesan yang tertunda dalam antrian (36 pesan) yang belum diproses (Unacked). Grafik "Message rates" menunjukkan garis merah yang tinggi, yang merepresentasikan tingkat publish yang tinggi, sementara garis ungu (consumer ack) tetap rendah karena subscriber memproses pesan dengan lambat. Ini adalah contoh sempurna bagaimana message broker dapat berfungsi sebagai buffer antara sistem yang memiliki kecepatan pemrosesan berbeda, mencegah kehilangan data dan kegagalan sistem saat terjadi lonjakan beban.

## Scaling dengan Multiple Subscribers
![Multiple Subscribers](images/chrome_OOCkSoqlPP.png)

Jika kita memiliki subscriber yang lambat, apa yang harus kita lakukan? Dalam arsitektur event-driven, memungkinkan untuk menjalankan banyak subscriber yang terhubung ke antrian yang sama. Dengan cara ini, beban pemrosesan dapat didistribusikan di antara beberapa instance subscriber.

Screenshot ini menunjukkan hasil setelah menjalankan beberapa instance subscriber secara bersamaan. Terlihat bahwa pemrosesan event dibagi di antara subscriber yang berbeda - satu memproses "Budi" dan "Dira", sementara yang lain memproses "Amir", "Cica", dan "Emir". Ini menunjukkan bahwa RabbitMQ mendistribusikan pesan di antara subscriber yang tersedia menggunakan pola round-robin.

Pada grafik "Message rates", terlihat bahwa lonjakan pesan dapat diproses lebih cepat dibandingkan dengan hanya menggunakan satu subscriber. Ini adalah contoh konsep scaling horizontal yang digunakan dalam sistem terdistribusi untuk menangani beban tinggi.

## Refleksi dan Perbaikan

Berdasarkan kode publisher dan subscriber yang ada, terdapat beberapa area yang dapat ditingkatkan untuk performa dan ketahanan sistem yang lebih baik seperti implementasi konkurensi pada subscriber untuk memproses beberapa pesan secara bersamaan dengan threading atau async/await, penggunaan batching pada publisher untuk meningkatkan throughput pada volume tinggi, serta pengembangan skenario load testing untuk memastikan sistem dapat menskalakan dengan baik pada berbagai pola traffic. 