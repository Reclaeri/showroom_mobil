https://www.linkedin.com/in/revan-oknanda-8950413a6/

# 🏍️ Database Showroom Motor

Project database sederhana menggunakan **MySQL / MariaDB** melalui **XAMPP (Windows)**.  
Database ini mengelola data barang (motor), pembeli, dan transaksi.

---

# 🚀 SETUP DATABASE

## 1️⃣ Masuk ke MySQL
```bash
mysql -u root
```
**Penjelasan:**  
Digunakan untuk login ke server MySQL sebagai user `root`.

---

## 2️⃣ Membuat Database
```sql
CREATE DATABASE showroom_motor;
USE showroom_motor;
```

**Penjelasan:**
- `CREATE DATABASE` → Membuat database baru bernama `showroom_motor`
- `USE` → Mengaktifkan database agar bisa digunakan

---

# 🗄️ STRUKTUR TABEL

## 📦 Tabel `barang`
```sql
CREATE TABLE barang (
    id_barang INT AUTO_INCREMENT PRIMARY KEY,
    nama_barang VARCHAR(100),
    harga DECIMAL(15,2),
    stok INT
);
```

**Penjelasan:**
- `id_barang` → ID unik (primary key)
- `AUTO_INCREMENT` → Bertambah otomatis
- `nama_barang` → Nama motor
- `harga` → Harga motor
- `stok` → Jumlah stok

---

## 👤 Tabel `pembeli`
```sql
CREATE TABLE pembeli (
    id_pembeli INT AUTO_INCREMENT PRIMARY KEY,
    nama_pembeli VARCHAR(100),
    alamat TEXT,
    no_hp VARCHAR(15)
);
```

**Penjelasan:**
Menyimpan data pelanggan yang membeli motor.

---

## 🧾 Tabel `transaksi`
```sql
CREATE TABLE transaksi (
    id_transaksi INT AUTO_INCREMENT PRIMARY KEY,
    nama_barang VARCHAR(100),
    tanggal_transaksi DATE,
    jumlah_beli INT,
    total_bayar DECIMAL(15,2)
);
```

**Penjelasan:**
Menyimpan data pembelian motor.

---

## 🔄 ALTER TABLE
```sql
ALTER TABLE transaksi
ADD nama_pembeli VARCHAR(100) AFTER id_transaksi;
```

**Penjelasan:**
Menambahkan kolom `nama_pembeli` ke dalam tabel transaksi.

---

# 📥 INSERT DATA

## Insert Barang
```sql
INSERT INTO barang (nama_barang, harga, stok) VALUES
('Honda Beat',19000000,15),
('Honda Vario 160',29000000,10),
('Honda Scoopy',23000000,15),
('Honda PCX 160',33000000,10),
('Honda ADV 160',37000000,5),
('Honda Supra X 125',16000000,15),
('Honda Verza',24000000,5);
```

**Penjelasan:**  
Menambahkan 7 data motor ke tabel barang.

---

## Insert Pembeli
```sql
INSERT INTO pembeli (nama_pembeli, alamat, no_hp) VALUES
('Dimas','Bojong','0388173891'),
('Syahrul','Bogor','0388173892'),
('Adli','Citayam','0388173893'),
('Azzam','Citayam','0388173894'),
('Gibran','Sukahati','0388173895'),
('Zaki','Sukahati','0388173896'),
('Revan','Citayam','0388173897');
```

**Penjelasan:**  
Menambahkan 7 data pembeli.

---

## Insert Transaksi
```sql
INSERT INTO transaksi (nama_barang, tanggal_transaksi, jumlah_beli, total_bayar) VALUES
('Honda Beat','2026-02-01',1,19000000),
('Honda Vario 160','2026-02-02',1,29000000),
('Honda Scoopy','2026-02-03',2,46000000),
('Honda PCX 160','2026-02-04',1,33000000),
('Honda ADV 160','2026-02-05',1,37000000),
('Honda Supra X 125','2026-02-06',3,48000000),
('Honda Verza','2026-02-07',1,24000000),
('Honda Beat','2026-02-08',2,38000000),
('Honda Scoopy','2026-02-09',1,23000000),
('Honda PCX 160','2026-02-10',2,66000000);
```

**Penjelasan:**  
Menambahkan 10 transaksi pembelian motor.

---

# 📊 QUERY & PENJELASAN

---

## 🔹 SELECT Semua Data
```sql
SELECT * FROM barang;
SELECT * FROM pembeli;
SELECT * FROM transaksi;
```

**Penjelasan:**  
Menampilkan seluruh isi tabel.

---

## 🔹 UPDATE Data
```sql
UPDATE pembeli
SET nama_pembeli = 'Bama'
WHERE id_pembeli = 4;
```

**Penjelasan:**  
Mengubah nama pembeli dengan ID 4 menjadi Bama.

---

## 🔹 Aggregate Function

### Rata-rata Harga
```sql
SELECT AVG(harga) FROM barang;
```
Menampilkan rata-rata harga motor.

### Harga Minimum
```sql
SELECT MIN(harga) FROM barang;
```
Menampilkan harga termurah.

### Harga Maksimum
```sql
SELECT MAX(harga) FROM barang;
```
Menampilkan harga termahal.

---

## 🔹 Subquery
```sql
SELECT nama_barang, harga
FROM barang
WHERE harga = (SELECT MIN(harga) FROM barang);
```

**Penjelasan:**  
Menampilkan motor dengan harga paling murah menggunakan subquery.

---

## 🔹 WHERE + OR + LIMIT
```sql
SELECT *
FROM transaksi
WHERE nama_barang = 'Honda Beat'
OR jumlah_beli = 1
LIMIT 5;
```

**Penjelasan:**
- Menampilkan transaksi Honda Beat
- ATAU jumlah beli = 1
- `LIMIT 5` membatasi 5 baris pertama

---

# 🔗 JOIN

## 🔹 INNER JOIN
```sql
SELECT
    t.id_transaksi,
    t.nama_pembeli,
    p.alamat,
    t.nama_barang,
    b.harga,
    t.jumlah_beli,
    t.total_bayar
FROM transaksi t
INNER JOIN pembeli p
    ON t.nama_pembeli = p.nama_pembeli
INNER JOIN barang b
    ON t.nama_barang = b.nama_barang;
```

**Penjelasan:**  
Menggabungkan tabel transaksi, pembeli, dan barang.  
Hanya data yang cocok di semua tabel yang ditampilkan.

---

## 🔹 LEFT JOIN
```sql
SELECT
    b.nama_barang,
    b.harga,
    t.jumlah_beli
FROM barang b
LEFT JOIN transaksi t
    ON b.nama_barang = t.nama_barang;
```

**Penjelasan:**  
Menampilkan semua barang walaupun belum ada transaksi.

---

## 🔹 UNION
```sql
SELECT b.nama_barang, t.tanggal_transaksi
FROM barang b
LEFT JOIN transaksi t ON b.nama_barang = t.nama_barang

UNION

SELECT b.nama_barang, t.tanggal_transaksi
FROM barang b
RIGHT JOIN transaksi t ON b.nama_barang = t.nama_barang;
```

**Penjelasan:**  
Menggabungkan hasil LEFT JOIN dan RIGHT JOIN menjadi satu hasil.

---

# 👁️ VIEW

## Membuat View
```sql
CREATE VIEW view_detail_transaksi AS
SELECT
    t.id_transaksi,
    p.nama_pembeli,
    p.alamat,
    p.no_hp,
    b.nama_barang,
    b.harga,
    t.jumlah_beli,
    t.total_bayar,
    t.tanggal_transaksi
FROM transaksi t
JOIN pembeli p ON t.nama_pembeli = p.nama_pembeli
JOIN barang b ON t.nama_barang = b.nama_barang;
```

**Penjelasan:**  
Membuat view untuk menampilkan detail transaksi tanpa perlu menulis JOIN berulang.

---

## Menampilkan View
```sql
SELECT * FROM view_detail_transaksi;
```

**Penjelasan:**  
Menampilkan isi dari view yang sudah dibuat.

---

# 🎯 Materi yang Dipelajari

- CREATE DATABASE
- CREATE TABLE
- INSERT
- UPDATE
- SELECT
- WHERE
- LIMIT
- Aggregate Function
- Subquery
- JOIN
- UNION
- ALTER TABLE
- VIEW

---

# 📌 Kesimpulan

Project ini menunjukkan cara membuat database showroom motor lengkap dengan relasi antar tabel dan pengolahan data transaksi menggunakan MySQL/MariaDB.
