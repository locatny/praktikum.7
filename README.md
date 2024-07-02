# praktikum.7

## ER-D

![](Foto/erd1.png)

## Data

![](Foto/data.png)

### *Tabel Perusahaan*
```sql
CREATE TABLE perusahaan(
id_p VARCHAR(10) PRIMARY KEY NOT NULL,
nama VARCHAR(45) NOT NULL,
alamat VARCHAR(45)
);

INSERT INTO perusahaan (id_p, nama, alamat) VALUES
('P01', 'Kantor Pusat', NULL),
('P02', 'Cabang Bekasi', NULL);
```
### *Output Tabel Perusahaan :*

![](Foto/tabelperusahaan.png)

### *Tabel Departemen*
```sql
CREATE TABLE departemen(
id_dept VARCHAR(10) PRIMARY KEY NOT NULL,
nama VARCHAR(45) NOT NULL,
id_p VARCHAR(10) NOT NULL,
manajer_nik VARCHAR(10)
FOREIGN KEY (id_p) REFERENCES perusahaan(id_p) 
);

INSERT INTO Departemen (id_dept, nama, id_p, manajer_nik) VALUES
('D01', 'Produksi', 'P02', 'N01'),
('D02', 'Marketing', 'P01', 'N03'),
('D03', 'RnD', 'P02', NULL),
('D04', 'Logistik', 'P02', NULL);
```

### *Output Tabel Departemen :*
![](Foto/departemen1.png)

### *Tabel Karyawan*
```sql
CREATE TABLE karyawan (
nik VARCHAR(10) PRIMARY KEY NOT NULL,
nama VARCHAR(45) NOT NULL,
id_dept VARCHAR(10),
sup_nik VARCHAR(10),
gaji_pokok INT(10) NOT NULL,
FOREIGN KEY (id_dept) REFERENCES Departemen(id_dept)
);

INSERT INTO karyawan (nik, nama, id_dept, sup_nik, gaji_pokok) VALUES
('N01', 'Ari', 'D01', NULL, 2000000),
('N02', 'Dina', 'D01', NULL, 2500000),
('N03', 'Rika', 'D03', NULL, 2400000),
('N04', 'Ratih', 'D01', 'N01', 3000000),
('N05', 'Riko', 'D01', 'N01', 2800000),
('N06', 'Dani', 'D02', NULL, 2100000),
('N07', 'Anis', 'D02', 'N06', 5000000),
('N08', 'Dika', 'D02', 'N06', 4000000),
('N09', 'Raka', 'D03', 'N06', 2000000);
```
### *Output Tabel Karyawan :*
![](Foto/karyawan1.png)

### *Tabel Project*
```sql
CREATE TABLE project (
id_proj VARCHAR(10) PRIMARY KEY NOT NULL,
nama VARCHAR(45) NOT NULL,
tgl_mulai DATETIME,
tgl_selesai DATETIME,
status TINYINT(1)
);

INSERT INTO project (id_proj, nama, tgl_mulai, tgl_selesai, status) VALUES
('PJ01', 'A', '2019-01-10', '2019-03-10', '1'),
('PJ02', 'B', '2019-02-15', '2019-04-10', '1'),
('PJ03', 'C', '2019-03-21', '2019-05-10', '1');
```

### *Output Tabel Project :*
![](Foto/project1.png)

### *Tabel Project Detail*
```sql
CREATE TABLE project_detail (
id_proj VARCHAR(10) NOT NULL,
nik VARCHAR(10) NOT NULL
);

INSERT INTO project_detail (id_proj, nik) VALUES
('PJ01', 'N01'),
('PJ01', 'N02'),
('PJ01', 'N03'),
('PJ01', 'N04'),
('PJ01', 'N05'),
('PJ01', 'N07'),
('PJ01', 'N08'),
('PJ02', 'N01'),
('PJ02', 'N03'),
('PJ02', 'N05'),
('PJ03', 'N03'),
('PJ03', 'N07'),
('PJ03', 'N08');
```

### *Output Tabel Project Detail :*
![](Foto/projectdetail1.png)

## Latihan 
### 1. Tampilkan data karyawan yang bekerja pada departemen yang sama dengan karyawan yang bernama Dika
```sql
SELECT * FROM karyawan
WHERE id_dept = (SELECT id_dept FROM karyawan WHERE nama = 'Dika');
```
*Output :*

![](Foto/a1.png)

### 2. Tampilkan data karyawan yang gajinya lebih besar dari rata-rata gaji semua karyawan. urutkan menurun berdasarkan besaran gaji
```sql
SELECT * FROM karyawan
WHERE gaji_pokok > (SELECT AVG(gaji_pokok) FROM karyawan
ORDER BY gaji_pokok ASC);
```
*Output :*

![](Foto/a2.png)

### 3. Tampilkan nik dan nama karyawan untuk semua karyawan yang bekerja di department yang sama dengan karyawan dengan nama yang mengandung huruf 'K'.
```sql
SELECT nik, nama FROM karyawan
WHERE id_dept IN (SELECT id_dept FROM karyawan WHERE nama LIKE '%K%');
```
*Output :*

![](Foto/a3.png)

### 4. Tampilkan data karyawan yang bekerja pada departemen yang ada di kantor pusat.
```sql
SELECT * FROM karyawan
WHERE id_dept IN (SELECT id_dept FROM departemen WHERE id_p = 'P01');
```
*Output :*

![](Foto/a4.png)

### 5. Tampilkan nik dan nama karyawan untuk semua karyawan yang bekerja didepartment yang sama dengan karyawan dengan nama yang mengandung huruf 'K' dan yang gajinya lebih besar dari rata-rata gaji semua karyawan
```sql
SELECT nik, nama FROM karyawan
WHERE id_dept IN (SELECT id_dept FROM karyawan WHERE nama LIKE '%K%')
AND gaji_pokok > (SELECT AVG(gaji_pokok) FROM karyawan);
```
*Output :*

![](Foto/a5.png)
