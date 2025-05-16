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