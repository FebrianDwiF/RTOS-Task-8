Proyek Kontrol LED dengan FreeRTOS pada STM32
Deskripsi
Proyek ini menggunakan FreeRTOS pada mikrokontroler STM32 untuk mengendalikan LED dengan tugas (tasks) yang berjalan paralel. Tiga LED dikendalikan dengan frekuensi kedipan yang berbeda, dan semaphore digunakan untuk mengelola akses ke sumber daya bersama.

LED Hijau (Green): Kedip setiap 200ms.
LED Merah (Red): Kedip setiap 550ms.
LED Kuning (Yellow): Kedip dengan frekuensi 10Hz (setiap 50ms).
Akses Sumber Daya Bersama: Dikelola menggunakan semaphore untuk memastikan hanya satu tugas yang mengakses sumber daya tersebut pada waktu yang sama.
Proyek ini menggunakan STM32, FreeRTOS, dan pustaka HAL STM32.

Fitur
Tugas FreeRTOS: Membuat dan mengelola beberapa tugas untuk mengendalikan LED.
Semaphore: Memastikan hanya satu tugas yang dapat mengakses sumber daya bersama pada satu waktu.
Kontrol GPIO: Menggunakan pin GPIO STM32 untuk mengendalikan LED.
Komunikasi UART: (Opsional) Untuk komunikasi serial dengan perangkat eksternal.
Komponen yang Digunakan
Mikrokontroler STM32: Digunakan untuk proyek ini.
FreeRTOS: Sistem operasi waktu nyata untuk mengelola tugas-tugas.
LED: LED Hijau, Merah, dan Kuning yang dikendalikan oleh pin GPIO.
UART1: Digunakan untuk komunikasi serial (opsional).
Instalasi
Prasyarat
STM32CubeMX (untuk konfigurasi mikrokontroler).
STM32CubeIDE (untuk pengembangan dan pengujian).
Board STM32 yang sesuai.
USB to Serial Adapter (untuk komunikasi UART, jika diperlukan).
Langkah-langkah untuk Menyiapkan Proyek
Clone repositori ini:

bash
Salin kode
git clone https://github.com/username/STM32-FreeRTOS-LED-Control.git
Buka STM32CubeIDE dan buat proyek STM32 baru dengan memilih mikrokontroler yang sesuai.

Konfigurasi GPIO:

Setel pin GPIO untuk LED (misalnya, LED1_Pin, LED2_Pin, dll.) sebagai output.
Aktifkan FreeRTOS:

Di STM32CubeMX, aktifkan FreeRTOS sebagai middleware.
Buat tugas untuk mengendalikan LED dan setel prioritas masing-masing tugas.
Setel Semaphore:

Buat semaphore CriticalResourceSemaphore untuk mengelola akses mutual eksklusif ke sumber daya bersama.
Bangun dan Flash Firmware:

Bangun proyek dan flash ke mikrokontroler STM32.
Deskripsi Tugas
Tugas LED Hijau: LED hijau berkedip setiap 200ms. Tugas ini menggunakan semaphore untuk mengakses sumber daya bersama secara eksklusif.
Tugas LED Merah: LED merah berkedip setiap 550ms. Tugas ini juga menggunakan semaphore untuk mengakses sumber daya bersama.
Tugas LED Kuning: LED kuning berkedip dengan frekuensi 10Hz (setiap 50ms), tanpa perlu akses ke sumber daya kritis.
Penggunaan Semaphore
Semaphore CriticalResourceSemaphoreHandle digunakan untuk mengontrol akses ke sumber daya bersama dalam fungsi accessSharedData(). Ini memastikan hanya satu tugas yang dapat mengakses data tersebut pada satu waktu.

Potongan Kode Semaphore
c
Salin kode
if (osSemaphoreWait(CriticalResourceSemaphoreHandle, osWaitForever) == osOK)
{
    accessSharedData();
    osSemaphoreRelease(CriticalResourceSemaphoreHandle);
}
Struktur Proyek
main.c: Berisi fungsi utama untuk menginisialisasi dan menjalankan FreeRTOS, tugas LED, dan semaphore.
FreeRTOSConfig.h: Konfigurasi pengaturan FreeRTOS.
GPIO dan UART Setup: Konfigurasi pin GPIO untuk LED dan UART untuk komunikasi serial (opsional).
Pengaturan Hardware
LED Hijau: Terhubung ke pin GPIO GPIO_PIN_0.
LED Merah: Terhubung ke pin GPIO GPIO_PIN_1.
LED Kuning: Terhubung ke pin GPIO GPIO_PIN_2.
LED Merah (opsional): Terhubung ke pin GPIO GPIO_PIN_3 untuk indikasi akses sumber daya bersama.
Debugging
Output UART: Jika Anda ingin melakukan debugging atau komunikasi serial, Anda bisa menggunakan UART untuk mencetak pesan debug.
c
Salin kode
int __io_putchar(int ch)
{
    HAL_UART_Transmit(&huart1, (uint8_t*)&ch, 1, HAL_MAX_DELAY);
    return ch;
}



https://github.com/user-attachments/assets/207811c9-b1f1-4ce0-ad80-ee78ab23728d


