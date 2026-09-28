<<<<<<< HEAD
# Blameless Postmortem - Insiden Kegagalan Deployment Manual

## Ringkasan Insiden
=======
>>>>>>> 360691eee935165b78155ec5ea81b341d05accbd
Tim Operations mengalami kegagalan saat mencoba menjalankan layanan aplikasi secara manual akibat tidak adanya berkas pengunci dependensi (`requirements.txt`) dan instruksi penyiapan virtual environment.

## Kronologi (timeline)
* Developer menyelesaikan penulisan kode `src/app.py` di lingkungan lokalnya.
* Developer menyerahkan folder kerja kepada Operations hanya dengan panduan teks singkat.
* Operations mencoba mengeksekusi aplikasi menggunakan perintah manual.
* Terjadi galat `ModuleNotFoundError: No module named 'flask'` yang menghentikan eksekusi.
* Tim menghabiskan waktu untuk proses troubleshooting manual sebelum aplikasi akhirnya bisa berjalan.

## Dampak (waktu terbuang, jumlah kegagalan)
* Jumlah kegagalan: 1 kali (kegagalan deployment pertama di mesin Operations).
* Waktu terbuang (Wait Time / Troubleshooting): ± 10–15 menit.

## Akar Masalah pada SISTEM (bukan pada orang)
Kegagalan ini berakar dari sistem serah-terima bergaya silo (*Wall of Confusion*). Tidak ada standardisasi pengemasan dependensi lingkungan kerja secara deklaratif, sehingga memunculkan masalah "it works on my machine".

## Tindakan Perbaikan (action items) + penanggung jawab peran
1. Membekukan seluruh dependensi ke dalam berkas `requirements.txt` (Penanggung Jawab: Developer).
2. Menyusun skrip otomasi `setup.sh` dengan penanganan galat `set -euo pipefail` untuk menggantikan eksekusi manual (Penanggung Jawab: Developer & Operations).
3. Menerapkan verifikasi *health check* secara otomatis setelah aplikasi berjalan (Penanggung Jawab: Operations).

## Pelajaran yang Diambil
Otomasi alur kerja (The First Way) jauh lebih andal daripada mengandalkan panduan instruksi manusia. Menerapkan infrastruktur sebagai kode sejak awal akan mengeliminasi miskomunikasi dan mempercepat *Lead Time* secara signifikan.
