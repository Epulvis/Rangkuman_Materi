# Rangkuman Materi Dasar Jaringan Komputer

## Protocol Layers
Protocol Layers terdiri dari 5 Layers yaitu:
1. Application Layer  
2. Transport Layer  
3. Network Layer  
4. Link Layer  
5. Physical Layer  

---

## Jenis-Jenis Delay dalam Protokol Internet

| Jenis Delay         | Simbol    | Penjelasan                                                                    |
|---------------------|-----------|--------------------------------------------------------------------------------|
| Processing Delay    | d_proc    | Waktu yang dibutuhkan router untuk memproses header paket (biasanya kecil)   |
| Queuing Delay       | d_queue   | Waktu tunggu di antrian router sebelum dikirim                                |
| Transmission Delay  | d_trans   | Waktu untuk me-load data ke dalam link                                        |
| Propagation Delay   | d_prop    | Waktu untuk sinyal bergerak di sepanjang media transmisi                      |

**Rumus Total Nodal Delay:**  
`d_total = d_proc + d_queue + d_trans + d_prop`  

**Transmission Delay:**  
`d_trans = L / R`  
- L = panjang paket (bit)  
- R = bandwidth link (bit per second)  

**Propagation Delay:**  
`d_prop = d / s`  
- d = jarak antar node (meter)  
- s = kecepatan propagasi sinyal (biasanya 2×10⁸ m/s di kabel)

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
