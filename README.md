
## **Judul Proyek**
Kontrol Multithreading LED dengan RTOS pada STM32

---

## **Deskripsi Proyek**
Proyek ini menunjukkan cara menggunakan FreeRTOS untuk mengontrol beberapa LED pada mikrokontroler STM32. Implementasi ini mencakup tiga thread terpisah yang mengontrol masing-masing LED (Hijau, Merah, dan Kuning), mengelola sumber daya bersama menggunakan semaphore, dan menangani potensi konflik akses sumber daya bersama.

---

## **Kebutuhan Perangkat Keras**
1. Mikrokontroler STM32 (misalnya, seri STM32F4)
2. Empat LED (Hijau, Merah, Kuning, dan Biru)
3. Breadboard dan kabel jumper
4. Lingkungan pengembangan: STM32CubeIDE atau Keil uVision

---

## **Konfigurasi Perangkat Lunak**
1. **FreeRTOS**: Sistem operasi waktu nyata untuk penjadwalan tugas dan sinkronisasi sumber daya.
2. **CMSIS-OS API**: Untuk membuat thread, semaphore, dan mengelola komponen RTOS.

---

## **Cara Membuat dan Menjalankan**
1. Unduh kode sumber atau kloning repositori ini.
2. Buka proyek di STM32CubeIDE.
3. Konfigurasikan sambungan perangkat keras sesuai pin berikut:
   - LED Hijau: GPIOA Pin 0
   - LED Merah: GPIOA Pin 1
   - LED Kuning: GPIOA Pin 2
   - LED Biru: GPIOA Pin 3 (digunakan untuk menunjukkan konflik akses sumber daya)
4. Bangun (build) proyek dan flash ke mikrokontroler STM32.
5. Perhatikan perilaku LED pada perangkat keras.

---

## **Penjelasan Kode**
### **Komponen Utama**
1. **Thread**:
   - **Green LED Task**: Menyalakan/mematikan LED Hijau sambil mencoba mengakses sumber daya bersama.
   - **Red LED Task**: Menyalakan/mematikan LED Merah sambil mencoba mengakses sumber daya bersama.
   - **Yellow LED Task**: Menyalakan/mematikan LED Kuning dengan frekuensi tetap, tanpa mengakses sumber daya bersama.

2. **Semaphore**:
   - Semaphore biner (`CriticalResourceSemaphoreHandle`) memastikan hanya satu thread yang dapat mengakses sumber daya bersama dalam satu waktu. Jika thread lain mencoba mengakses saat sumber daya sedang digunakan, konflik ditunjukkan dengan menyalakan LED Biru.

3. **Sumber Daya Bersama**:
   - Disimulasikan melalui fungsi `accessSharedData()`, yang memanipulasi variabel `startFlag`. Fungsi ini menambahkan delay untuk mensimulasikan sumber daya nyata, seperti membaca/menulis file atau berinteraksi dengan sensor.

---

### **Alur Kerja Kode**
1. **Inisialisasi**:
   - Sistem clock dan pin GPIO diinisialisasi.
   - Thread RTOS dan semaphore dibuat.

2. **Eksekusi Thread**:
   - **Green LED Task** dan **Red LED Task**:
     - Menyalakan LED masing-masing.
     - Menggunakan semaphore untuk mengakses sumber daya bersama.
     - Melepaskan semaphore setelah selesai.
     - Mematikan LED dan menunggu delay.
   - **Yellow LED Task**: Menghidupkan/mematikan LED Kuning dengan frekuensi 10 Hz tanpa melibatkan semaphore.

3. **Fungsi Semaphore**:
   - Semaphore memastikan mutual exclusion (akses eksklusif) untuk sumber daya bersama.
   - Jika thread gagal mendapatkan semaphore, LED Biru dinyalakan untuk menunjukkan konflik.
   - Setelah selesai, semaphore dilepaskan untuk memungkinkan thread lain mengakses sumber daya.

---

## **Fungsi Semaphore dalam Kode Ini**
Dalam proyek ini, **semaphore** digunakan untuk melindungi akses ke sumber daya bersama (`accessSharedData()`) yang digunakan oleh Green LED Task dan Red LED Task. Berikut penjelasannya:

1. **Mengambil Semaphore**:
   - Thread menunggu semaphore menggunakan `osSemaphoreWait()`. Jika semaphore tersedia, thread melanjutkan akses ke sumber daya bersama.
   - Jika semaphore tidak tersedia, thread akan menunggu hingga sumber daya bebas.

2. **Melepaskan Semaphore**:
   - Setelah menggunakan sumber daya, thread melepaskan semaphore menggunakan `osSemaphoreRelease()`, sehingga thread lain dapat menggunakannya.

3. **Penanganan Konflik**:
   - Jika `startFlag` menunjukkan bahwa sumber daya sedang digunakan (walaupun semaphore berhasil diperoleh), konflik disimulasikan dengan menyalakan LED Biru.
   - Konflik ini dapat terjadi akibat kesalahan akses atau pelanggaran pengaturan sumber daya.

---

## **Pengamatan Utama**
1. Penggunaan semaphore mencegah akses simultan ke sumber daya bersama, sehingga menghindari potensi kerusakan data.
2. Konflik akses sumber daya divisualisasikan menggunakan LED Biru, membantu dalam proses debugging dan memahami perilaku RTOS.

---



https://github.com/user-attachments/assets/90f9637f-cc29-466f-80e0-ad6b7c8d01b2

