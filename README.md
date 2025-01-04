# Step by Step Pembuatan Database Bakmie Sarmul
### 1. Membuat Table Barang ('barang')
```sql
CREATE TABLE `barang` (
  `ID_BARANG` int(11) NOT NULL AUTO_INCREMENT PRIMARY KEY,
  `NAMA_BARANG` varchar(50) NOT NULL,
  `STOK_KILOAN` int(200) DEFAULT NULL,
  `HARGA_KILOAN` int(11) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;
```
#
![alt text](/Pic/1.png)

### 2. Membuat Table Karyawan ('karyawan')
```sql
CREATE TABLE `karyawan` (
  `ID_KARYAWAN` int(11) NOT NULL AUTO_INCREMENT PRIMARY KEY,
  `NAMA_KARYAWAN` varchar(50) NOT NULL,
  `POSISI_KARYAWAN` varchar(50) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;
```
#
![alt text](/Pic/2.png)

### 3. Membuat Table Supplier ('supplier')
```sql
CREATE TABLE `supplier` (
  `ID_SUPPLIER` int(11) NOT NULL AUTO_INCREMENT PRIMARY KEY,
  `NAMA_SUPPLIER` varchar(50) NOT NULL,
  `KONTAK_SUPPLIER` varchar(20) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;
```
#
![alt text](/Pic/3.png)

### 4. Membuat Table Relasi Restocking ('restocking')
```sql
CREATE TABLE `restocking` (
  `ID_RESTOCKING` int(11) NOT NULL AUTO_INCREMENT PRIMARY KEY,
  `ID_SUPPLIER` int(11) DEFAULT NULL,
  `ID_BARANG` int(11) DEFAULT NULL,
  `JUMLAH_BARANG` int(11) NOT NULL,
  `ID_KARYAWAN` int(11) DEFAULT NULL,
  `WAKTU_RESTOCKING` TIMESTAMP NOT NULL,
      FOREIGN KEY (ID_BARANG) REFERENCES barang(ID_BARANG),
      FOREIGN KEY (ID_SUPPLIER) REFERENCES supplier(ID_SUPPLIER),
      FOREIGN KEY (ID_KARYAWAN) REFERENCES karyawan(ID_KARYAWAN)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;
```
#
![alt text](/Pic/4.png)

### 5. Membuat View ('view_restock')
```sql
CREATE VIEW view_restock AS
SELECT
    R.ID_RESTOCKING AS ID,
    R.ID_SUPPLIER AS ID_SUPPLIER,
    R.ID_BARANG AS ID_BARANG,
    R.JUMLAH_BARANG AS JUMLAH_RESTOCK,
    B.HARGA_KILOAN AS HARGA_KILOAN,
    (R.JUMLAH_BARANG * B.HARGA_KILOAN) AS TOTAL_HARGA,
    R.WAKTU_RESTOCKING AS WAKTU,
    (B.STOK_KILOAN +
        COALESCE(
            (SELECT SUM(r2.JUMLAH_BARANG)
             FROM Restocking r2
             WHERE r2.ID_BARANG = R.ID_BARANG
               AND r2.ID_RESTOCKING <= R.ID_RESTOCKING), 0)
    ) AS LIVE_STOK
FROM
    Restocking R
JOIN
    Barang B ON R.ID_BARANG = B.ID_BARANG;
```
#
![alt text](/Pic/5.png)

### 6. Input Data pada Table Barang
```sql
INSERT INTO `barang` (`NAMA_BARANG`, `STOK_KILOAN`, `HARGA_KILOAN`) VALUES
('Sawi', '1', 14000),
('Ayam', '1', 50000),
('Bawang', '1', 5000),
('Mie', '1', 20000),
('Daun Bawang', '1', 8000),
('Saus', '1', 19000),
('Kecap Manis', '1', 30000),
('Chili Oil', '1', 70000),
('Pangsit', '1', 25000),
('Garam', '1', 10000),
('Mecin', '1', 38000),
('Kecap Asin', '1', 25000),
('Bihun', '1', 20000),
('Kwetiaw', '1', 12000),
('Aqua Botol', '1', 4000),
('Tisu', '1', 40000),
('Bakso', '1', 24000),
('Selada', '1', 34000),
('Minyak Bawang', '1', 55000),
('Gas Elpigi', '6', 6000);
```
#
![alt text](/Pic/6.png)
#
![alt text](/Pic/7.png)

### 7. Input Data pada Table Karyawan
```sql
INSERT INTO `karyawan` (`NAMA_KARYAWAN`, `POSISI_KARYAWAN`) VALUES
('Sarmul', 'Owner'),
('Agus', 'Staf'),
('Bagas', 'Staf'),
('Nur', 'Staf'),
('Harianto', 'Staf'),
('Tri', 'Staf'),
('Zaenal', 'Staf'),
('Abidin', 'Staf'),
('Arifin', 'Staf'),
('Lutfi', 'Staf'),
('Beni', 'Staf'),
('Rizki', 'Staf'),
('Abdi', 'Staf'),
('Budi', 'Staf'),
('Ani', 'Staf'),
('Dadang', 'Staf'),
('Ardi', 'Staf'),
('Rafli', 'Staf'),
('Hafidz', 'Staf'),
('Rehan', 'Staf');
```
#
![alt text](/Pic/8.png)
#
![alt text](/Pic/9.png)

### 8. Input Data pada Table Supplier
```sql
INSERT INTO `supplier` (`NAMA_SUPPLIER`, `KONTAK_SUPPLIER`) VALUES
('Toko Sayuran', '08123456789'),
('Toko Ayam Potong Daniel', '08123456789'),
('Toko Bawang Tegal', '08123456789'),
('Toko Mie Xie', '08123456789'),
('Toko Dedaunan Hijau', '08123456789'),
('Toko Saus Mentai', '08123456789'),
('Toko Malika', '08123456789'),
('Toko Chilili', '08123456789'),
('Toko Punksit', '08123456789'),
('Toko Garam Asli Laut', '08123456789'),
('Toko Gen Z', '08123456789'),
('Toko Malika Asin', '08123456789'),
('Toko Sohun Bersaudara', '08123456789'),
('Toko Kwe dan Tiaw', '08123456789'),
('Toko Dehidrasi', '08123456789'),
('Toko Pembersih Segala', '08123456789'),
('Toko Frozen Food Bella', '08123456789'),
('Toko Sesayuran', '08123456789'),
('Toko Perminyakan', '08123456789'),
('Toko Gas', '08123456789');
```
#
![alt text](/Pic/10.png)
#
![alt text](/Pic/11.png)

### 9. Input Data pada Table Relasi Restocking
```sql
INSERT INTO `restocking` (`ID_SUPPLIER`, `ID_BARANG`, `JUMLAH_BARANG`, `ID_KARYAWAN`) VALUES
('1', '1', '2', '1'),
('2', '2', '3', '2'),
('3', '3', '5', '3'),
('4', '4', '4', '4'),
('5', '5', '6', '5'),
('6', '6', '8', '6'),
('7', '7', '1', '7'),
('8', '8', '12', '8'),
('9', '9', '10', '9'),
('10', '10', '2', '10'),
('11', '11', '3', '11'),
('12', '12', '5', '12'),
('13', '13', '4', '13'),
('14', '14', '6', '14'),
('15', '15', '7', '15'),
('16', '16', '4', '16'),
('17', '17', '8', '17'),
('18', '18', '9', '18'),
('19', '19', '6', '19'),
('20', '20', '2', '20');
```
#
![alt text](/Pic/12.png)
#
![alt text](/Pic/13.png)

### 10. Tampilan pada view_restock
#
![alt text](/Pic/14.png)

