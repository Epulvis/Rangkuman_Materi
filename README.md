# Rangkuman Materi Dasar Jaringan Komputer

## Protocol Layers

Protocol Layers terdiri dari 5 Layers yaitu:

1. Application Layer
2. Transport Layer
3. Network Layer
4. Link Layer
5. Physical Layer

### Fungsi Utama Tiap Layer dan Contoh Protokol

| Layer       | Fungsi Utama                                                         | Contoh Protokol                     |
| ----------- | -------------------------------------------------------------------- | ----------------------------------- |
| Application | Antarmuka pengguna, pengelolaan aplikasi jaringan                    | HTTP, SMTP, DNS, FTP, DHCP          |
| Transport   | Pengiriman data end-to-end, reliabilitas, kontrol kemacetan & aliran | TCP, UDP                            |
| Network     | Penentuan rute dan pengalamatan paket antar jaringan                 | IP, ICMP, ARP                       |
| Link        | Komunikasi antar perangkat satu jaringan fisik                       | Ethernet, Wi-Fi, PPP                |
| Physical    | Pengiriman bit melalui media fisik (kabel, sinyal, gelombang)        | Ethernet (fisik), DSL, fiber, kabel |

---

## Jenis-Jenis Delay dalam Protokol Internet

| Jenis Delay        | Simbol   | Penjelasan                                                                 |
| ------------------ | -------- | -------------------------------------------------------------------------- |
| Processing Delay   | d\_proc  | Waktu yang dibutuhkan router untuk memproses header paket (biasanya kecil) |
| Queuing Delay      | d\_queue | Waktu tunggu di antrian router sebelum dikirim                             |
| Transmission Delay | d\_trans | Waktu untuk me-load data ke dalam link                                     |
| Propagation Delay  | d\_prop  | Waktu untuk sinyal bergerak di sepanjang media transmisi                   |

**Rumus Total Nodal Delay:**
`d_total = d_proc + d_queue + d_trans + d_prop`

**Transmission Delay:**
`d_trans = L / R`

* L = panjang paket (bit)
* R = bandwidth link (bit per second)

**Propagation Delay:**
`d_prop = d / s`

* d = jarak antar node (meter)
* s = kecepatan propagasi sinyal (biasanya 2×10⁸ m/s di kabel)

---

## Perbedaan Throughput dan Bandwidth

| Aspek           | Bandwidth                        | Throughput                             |
| --------------- | -------------------------------- | -------------------------------------- |
| Definisi        | Kapasitas maksimal link          | Jumlah data aktual yang ditransmisikan |
| Sifat           | Teoritis (ideal)                 | Nyata (real-time)                      |
| Satuan          | bps (bit per second), Mbps, Gbps | bps, Mbps, Gbps                        |
| Faktor Pengaruh | Fisik jaringan (media)           | Kemacetan, error, jarak, protokol      |

---

## Application Layer

| Protokol     | Fungsi Utama           | Port     | Contoh Penggunaan                  |
|--------------|------------------------|----------|------------------------------------|
| HTTP/HTTPS   | Akses web              | 80/443   | Buka website                       |
| SMTP         | Kirim email            | 25       | Kirim email via Gmail              |
| POP3         | Ambil email            | 110      | Download email ke aplikasi         |
| IMAP         | Akses email terpusat   | 143      | Sinkron email antar perangkat      |
| DNS          | Resolusi nama          | 53       | google.com → IP                    |
| FTP          | Transfer file          | 20/21    | Upload ke server                   |
| BitTorrent   | P2P file sharing       | –        | Download file besar                |
| DHCP         | Pemberian IP otomatis  | –        | Dapat IP saat konek WiFi           |

---

## Cara Kerja DNS Secara Ringkas

1. Pengguna mengetik nama domain (misal: google.com)
2. DNS resolver lokal mencari ke cache, jika tidak ada:
3. Menghubungi root DNS → TLD DNS → authoritative DNS
4. Mendapatkan IP address → dikembalikan ke pengguna
5. Browser menggunakan IP tersebut untuk terhubung ke server

---

## Transport Layer

### 1. UDP (User Datagram Protocol)
- Connectionless: Tidak ada sesi awal antara pengirim dan penerima.  
- Tidak andal (unreliable): Tidak menjamin data sampai.  
- Tidak berurutan.  
- Tanpa flow control dan congestion control.  
- Cepat dan ringan, cocok untuk real-time.  
**Contoh:** Streaming, Zoom, YouTube Live  

### 2. TCP (Transmission Control Protocol)
- Connection-oriented (3-way handshake)  
- Andal dan berurutan  
- Flow control dan congestion control  
**Contoh:** HTTP, Email

### Multiplexing dan Demultiplexing
- Multiplexing : mengirim data dari beberapa aplikasi keluar ke jaringan\
Proses menggabungkan data dari banyak aplikasi yang berbeda di satu host untuk dikirim ke jaringan.\
📥 Dari banyak aplikasi → ➡️ satu koneksi jaringan keluar
- Demultiplexing : menerima data dan mengarahkan ke aplikasi yang benar\
Proses menerima data dari jaringan dan mengarahkan ke aplikasi yang tepat di host tujuan.\
➡️ Data masuk dari jaringan → 📤 diarahkan ke aplikasi yang sesuai

| Fitur             | **TCP (Connection-Oriented)**                | **UDP (Connectionless)**               |
| ----------------- | -------------------------------------------- | -------------------------------------- |
| Koneksi           | Ya (handshake)                               | Tidak                                  |
| Identitas koneksi | 4-tuple (src IP, src port, dst IP, dst port) | 2-tuple (dst IP, dst port)             |
| Multiplexing      | Berdasarkan banyak koneksi TCP               | Berdasarkan port tujuan                |
| Demultiplexing    | Sangat spesifik (per koneksi)                | Lebih sederhana, berdasarkan port saja |
| Contoh Protokol   | HTTP, FTP, SMTP                              | DNS, VoIP, DHCP                        |

---

## Protokol Pengiriman Data

### Go-Back-N (GBN)
- Sender kirim banyak paket (window size N)  
- Receiver hanya terima yang urut  
- Jika ada error → sender kirim ulang dari yang error  

### Stop-and-Wait
- Kirim 1 paket → tunggu ACK → baru kirim berikutnya  
- Sederhana tapi lambat  

### Selective Repeat
- Receiver bisa simpan paket yang tidak urut  
- Hanya kirim ulang yang hilang saja  

---

## TCP Congestion Control

**Kemacetan terjadi saat:**  
- Router penuh  
- Paket hilang  
- Performa menurun  

### Komponen:
- Congestion Window (cwnd)

### 4 Fase Utama:
1. **Slow Start**: cwnd naik eksponensial  
2. **Congestion Avoidance**: cwnd naik linear  
3. **Fast Retransmit**: kirim ulang setelah 3 duplicate ACK  
4. **Fast Recovery**: cwnd turun setengah, lalu lanjut pelan

| Fitur               | Penjelasan                                      |
|---------------------|--------------------------------------------------|
| Slow Start          | cwnd naik cepat untuk uji kapasitas             |
| Congestion Avoidance| cwnd naik pelan untuk cegah kemacetan           |
| Fast Retransmit     | Retransmisi cepat tanpa tunggu timeout          |
| Fast Recovery       | Kurangi cwnd, lalu lanjut dari pertengahan      |

---
### AIMD adalah algoritma kontrol kemacetan pada TCP
### cwnd adalah ukuran jendela yang mengontrol berapa banyak data (segmen) yang bisa dikirim sebelum menunggu ACK.
📌 Fungsi Utama:
- Mengatur jumlah maksimum data yang bisa berada di jaringan sekaligus.
- Mencerminkan tingkat kepercayaan sender terhadap kondisi jaringan.
### ssthresh adalah batas antara dua fase:
- Slow Start (eksponensial)
- Congestion Avoidance (linear)\
📌 Fungsi Utama:
- Menentukan kapan TCP berhenti naik cepat dan mulai naik pelan.

---


## End-to-End vs Network-Assisted Congestion Control

| Jenis               | End-to-End Congestion Control              | Network-Assisted Congestion Control                |
| ------------------- | ------------------------------------------ | -------------------------------------------------- |
| Informasi Kemacetan | Diperkirakan oleh pengirim sendiri         | Router memberi tahu langsung                       |
| Protokol Umum       | TCP (standar)                              | ATM, Explicit Congestion Notification (ECN)        |
| Implementasi        | Tidak perlu bantuan dari router            | Butuh dukungan router                              |
| Mana lebih baik?    | End-to-end lebih fleksibel, luas digunakan | Network-assisted bisa lebih optimal, tapi kompleks |

---

## Forwarding vs Routing

| Aspek       | Forwarding                         | Routing                                   |
| ----------- | ---------------------------------- | ----------------------------------------- |
| Fungsi      | Mengirim paket ke interface tujuan | Menentukan jalur terbaik untuk pengiriman |
| Lokasi      | Setiap kali paket tiba di router   | Periodik, saat routing table diperbarui   |
| Berdasarkan | Tabel routing                      | Protokol routing (OSPF, RIP, BGP)         |

---

## Network Layer

### Address Class

| Class | Range Octet 1 | Default Subnet Mask | Network Bits | Host Bits | Contoh IP     |
|-------|----------------|----------------------|---------------|------------|---------------|
| A     | 1 – 126        | 255.0.0.0            | 8 bits        | 24 bits     | 10.0.0.1      |
| B     | 128 – 191      | 255.255.0.0          | 16 bits       | 16 bits     | 172.16.5.4    |
| C     | 192 – 223      | 255.255.255.0        | 24 bits       | 8 bits      | 192.168.1.10  |
| D     | 224 – 239      | – (Multicast)        | –             | –          | 224.0.0.1     |
| E     | 240 – 255      | – (Experimental)     | –             | –          | 250.1.1.1     |

---

### Network & Host ID

| IP Address         | Class | Subnet Mask       | Network ID         | Host ID       |
|--------------------|--------|-------------------|---------------------|----------------|
| 10.1.2.3           | A      | 255.0.0.0         | 10.0.0.0            | 1.2.3          |
| 172.20.1.1         | B      | 255.255.0.0       | 172.20.0.0          | 1.1            |
| 192.168.100.10     | C      | 255.255.255.0     | 192.168.100.0       | 10             |

---

### Subnetting

- **10.x.x.x** untuk Class A  
- **Subnet address** tergantung jumlah host

| Fungsi                 | Rumus                             |
|------------------------|-----------------------------------|
| Jumlah Subnet          | `2^n` (n = bit yang dipinjam)     |
| Jumlah Host/Subnet     | `2^h - 2` (h = bit host)          |
| Block Size             | `256 - oktet terakhir subnet mask`|
| CIDR                   | Jumlah bit 1 dalam subnet mask    |

---

## Longest Prefix Matching

Adalah teknik dalam router untuk memilih entri routing paling spesifik (paling panjang cocokannya) saat mencocokkan alamat IP tujuan dengan prefix dalam tabel routing.

**Contoh:**
Alamat tujuan: `192.168.10.5`
Tabel routing:

* 192.168.0.0/16 → Interface A
* 192.168.10.0/24 → Interface B
* 192.168.10.0/25 → Interface C

Hasil: cocok paling panjang = /25 → Interface C digunakan.

---

## Contoh Perhitungan Subnet Mask

**IP Address:** 192.168.10.10/26
**Subnet Mask:** 255.255.255.192 (karena /26 berarti 6 bit untuk host)

**Blok ukuran:** 256 - 192 = 64
**Subnet:**

* 192.168.10.0 → valid host: 192.168.10.1 - 192.168.10.62
* Broadcast: 192.168.10.63

Jika IP berada di 192.168.10.10, maka:

* Network ID: 192.168.10.0
* Broadcast: 192.168.10.63
* Range host valid: 192.168.10.1 – 192.168.10.62

---
