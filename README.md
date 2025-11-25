# PROJECT-2---SISTEM-BACKUP-OTOMATIS---MELINDA


# Masuk ke Direktori Home :
[Deskripsi gambar] (https://drive.google.com/file/d/1dmGNM0TVTk-9q129_5rS6crJM8OThzZZ/view?usp=sharing)

~~~
cd
~~~


# Membuat Direktori :
[Deskripsi gambar] (https://drive.google.com/file/d/1MDQbStxO3WFYjgwdBhOYS4JGxf7ROzy8/view?usp=sharing)

~~~
mkdir -p ~/sumberfile
~~~

~~~
mkdir -p ~/backup_otomatis
~~~

Penjelasan :
- Folder pertama ini berisi file-file yang ingin kita backup, sedangkan folder kedua adalah tempat penyimpanan file backup yang sudah dikompres.

# Menambahkan dua contoh file ke dalam folder sumberfile :
[Deskripsi gambar] (https://drive.google.com/file/d/1SwS0hwv5ixOGDGVMC4A-ihWH59Dvs6ai/view?usp=sharing)

~~~
echo "contoh file txt" > ~/sumberfile/catatan.txt
~~~

~~~
echo "contoh file pdf" > ~/sumberfile/laporan.pdf
~~~

Penjelasan :
- Ini hanya contoh untuk membuktikan bahwa script saya bisa mencari file teks dan PDF sesuai kriteria tugas.

# Membuat script utama bernama backup.sh :
[Deskripsi gambar] (https://drive.google.com/file/d/18hGPBHI8UcLP1CKJlfLKtMFResLOw5ad/view?usp=sharing)

~~~
nano backup.sh
~~~

# ISI SCRIPT :
[Deskripsi gambar] (https://drive.google.com/file/d/15jdNRFsOZe6PF3j5A0hMH_pCLOIpJZXj/view?usp=sharing) (https://drive.google.com/file/d/1BPyqkPk59n0iPsbajF-WI9ru_DnWA06T/view?usp=sharing) 

~~~
#!/bin/bash

# ================================
# SISTEM BACKUP OTOMATIS
# ================================

# 1. Direktori sumber
SOURCE_DIR="$HOME/sumberfile"

# 2. Direktori tujuan backup
BACKUP_DIR="$HOME/backup_otomatis"

# Buat direktori backup jika belum ada
mkdir -p "$BACKUP_DIR"

# Timestamp (agar nama backup tidak tabrakan)
TIMESTAMP=$(date +'%Y%m%d_%H%M%S')

# Nama file backup
BACKUP_FILE="backup_${TIMESTAMP}.tar.gz"

# Lokasi file log
LOG_FILE="$BACKUP_DIR/backup.log"

echo "========================================" >> "$LOG_FILE"
echo "Backup dijalankan pada: $TIMESTAMP" >> "$LOG_FILE"

# 3. Cari file .txt dan .pdf yang dimodifikasi 7 hari terakhir
FILES=$(find "$SOURCE_DIR" -type f \( -name ".txt" -o -name ".pdf" \) -mtime -7)

# Jika tidak ada file, tulis log dan keluar
if [ -z "$FILES" ]; then
    echo "Tidak ada file yang memenuhi kriteria." >> "$LOG_FILE"
    echo "Backup gagal: tidak ada file yang sesuai kriteria."
    exit 1
fi

# 4. Kompres file menggunakan tar + gzip
tar -czf "$BACKUP_DIR/$BACKUP_FILE" $FILES

# Cek apakah backup berhasil
if [ $? -eq 0 ]; then
    echo "Backup berhasil: file $BACKUP_FILE dibuat." >> "$LOG_FILE"
    echo "Backup berhasil! File tersimpan sebagai: $BACKUP_FILE"
else
    echo "Backup gagal saat kompresi." >> "$LOG_FILE"
    echo "Backup gagal."
    exit 1
fi
~~~

Penjelasan :
- #!/bin/bash = sebagai shebang supaya sistem tahu bahwa script ini harus dijalankan menggunakan bash.
  
- SOURCE_DIR="$HOME/sumberfile" = direktori sumber file.
- BACKUP_DIR="$HOME/backup_otomatis" = sebagai direktori tempat hasil backup disimpan.
  
- mkdir -p "$BACKUP_DIR" = untuk memastikan bahwa direktori backup sudah ada. Kalau folder tersebut belum ada, maka otomatis akan dibuat oleh script.
  
- TIMESTAMP=$(date +'%Y%m%d_%H%M%S'). = untuk membuat nama file backup yang unik, jadi tidak akan saling menimpa jika script dijalankan beberapa kali.
  
- BACKUP_FILE="backup_${TIMESTAMP}.tar.gz = Nama file backup yang akan dibuat saya simpan di variabel.
  
- LOG_FILE="$BACKUP_DIR/backup.log = pencatatan log.
  
- echo "============================" >> "$LOG_FILE" = garis pemisah
- echo "Backup dijalankan pada : $TIMESTAMP" >> "$LOG_FILE" = informasi waktu eksekusi.
  
- FILES=$(find "$SOURCE_DIR" -type f \( -name "*.txt" -o -name "*.pdf" \) -mtime -7) = pencarian file sesuai kriteria tugas/mencari file berekstensi .txt atau .pdf yang dimodifikasi dalam 7 hari terakhir.
  
- if [ -z "$FILES" ]; then   DAN   exit 1  = Kalau hasilnya kosong, artinya tidak ada file yang memenuhi kriteria, maka script akan menulis log dan menghentikan proses.

- tar -czf "$BACKUP_DIR/$BACKUP_FILE" $FILES = mengompres file.  Opsi c untuk create, z untuk gzip compression, dan f untuk menentukan nama file output.

- if [ $? -eq 0 ] = mengecek apakah perintah terakhir berhasil. kalau berhasil, script menuliskan log bahwa backup berhasil, dan menampilkan pesan sukses di terminal. tetapi jika gagal, maka script mencatat bahwa proses kompresi gagal dan menghentikan eksekusi.

- Tambahan : CONTROL + O  dan  CONTROL + X  =  Menyimpan Script dan Keluar dari NANO.

# Memberi Premission/Izin agar script bisa dijalankan :
[Deskripsi gambar] (https://drive.google.com/file/d/1uEhbMGER-sNUcQojvlkZyI9Cc_sIqBSv/view?usp=sharing)

~~~
chmod +x backup.sh
~~~

# Jalankan scriptnya :
[Deskripsi gambar] (https://drive.google.com/file/d/1GDmurezcxElKdLOLjtj282WDMZG7G1GV/view?usp=sharing) 

~~~
./backup.sh
~~~

Penjelasan : Di terminal bisa terlihat bahwa proses berjalan. tar akan menampilkan beberapa informasi tentang kompresi, dan setelah selesai muncul pesan bahwa backup berhasil.

# Masuk ke Direktori Backup :
[Deskripsi gambar] (https://drive.google.com/file/d/1kD9kSPq7YntbNvJEFmTPI7Hr-1mrQPGw/view?usp=sharing)

~~~
cd ~/backup_otomatis
~~~

# Melihat Isi Folder :
[Deskripsi gambar] (https://drive.google.com/file/d/1c3NOt386_BMcPtjwFjl2Xv0Zo-gMVIuB/view?usp=sharing)

~~~
ls
~~~

- Tambahan : Di sini terlihat file backup dengan nama yang mengandung timestamp, misalnya backup_20251122_004602.tar.gz

# Menampilkan isi file log :
[Deskripsi gambar] (https://drive.google.com/file/d/1YLt5AioB6p9ePz5GGg6nq48qEihCVxZ8/view?usp=sharing)

~~~
cat backup.log
~~~

- Tambahan : terlihat catatan bahwa backup dijalankan pada jam tertentu dan prosesnya berhasil.



