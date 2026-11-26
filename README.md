# tubitak-2209a-inrush-detection
## 🎯 Araştırma Sorusu ve Proje Amacı

* **Problem:** Transformatörlerin ilk enerjilenme anında çekirdek doygunluğuna bağlı olarak nominal akımın katlarına ulaşan geçici demeraj akımları oluşur. Standart aşırı akım koruma sistemleri bu geçici durumu arıza olarak algılayıp gereksiz açma (false tripping) yapabilmektedir.
* **Araştırma Sorusu:** Düşük maliyetli bir STM32 mikrodenetleyici üzerinde koşan FFT tabanlı 2. harmonik analizi algoritması, transformatör demeraj akımı ile dahili kısa devre akımını milisaniyeler mertebesinde ne doğrulukla ayırt edebilir?
* **Başarı Kriteri:** Önerilen gömülü algoritmanın, farklı anahtarlama açılarında oluşan demeraj akımlarını **%95 ve üzeri doğrulukla** tespit ederek gereksiz koruma açmalarını engellemesi ve toplam tepki süresinin **20 ms (1 şebeke periyodu) altında** tutulması.

---

## 🛠️ Sistem Mimarisi ve Kullanılan Teknolojiler

### Donanım Bileşenleri
* **Mikrodenetleyici:** STM32F103C8T6 / STM32F4 Serisi (ARM Cortex-M3 / M4)
* **Akım Algılama:** Akım Trafosu (CT) / Hall-Effect Akım Sensörü ve Sinyal Koşullandırma (DC Offset + Anti-Aliasing Alçak Geçiren Filtre)
* **Test Düzeneği:** Tek fazlı izolasyon/güç transformatörü ve kontrollü anahtarlama devresi

### Yazılım ve Simülasyon Araçları
* **Gömülü Yazılım:** C, STM32CubeIDE, HAL Kütüphanesi, **ARM CMSIS-DSP** (Real FFT)
* **Simülasyon ve Modelleme:** MATLAB / Simulink, Python (NumPy, Matplotlib)
* **PCB ve Devre Tasarımı:** KiCad / Proteus

---

## ⚙️ Yöntem ve Çalışma Akışı

1. **Veri Toplama (ADC + DMA):** 50 Hz şebeke akımı sinyali, Nyquist kriterine uygun örnekleme frekansında (ör. 1.6 kHz veya 3.2 kHz) kesintisiz olarak DMA üzerinden tampon belleğe alınır.
2. **Frekans Uzayı Dönüşümü (FFT):** ARM CMSIS-DSP kütüphanesi kullanılarak zaman uzayındaki akım örneklerine Hızlı Fourier Dönüşümü (FFT) uygulanır.
3. **Harmonik Oran Hesabı:** Temel bileşen ($I_{50\text{Hz}}$) ile ikinci harmonik bileşenin ($I_{100\text{Hz}}$) genlik oranı hesaplanır:
   $$k_{2h} = \frac{I_{100\text{Hz}}}{I_{50\text{Hz}}}$$
4. **Karar Mekanizması:** Hesaplanan $k_{2h}$ oranı belirlenen eşik değerin (literatürde tipik %15–%20) üzerindeyse durum **Demeraj (Inrush)** olarak sınıflandırılır ve açma sinyali bloke edilir; altındaysa **Arıza (Fault)** kararı üretilir.
