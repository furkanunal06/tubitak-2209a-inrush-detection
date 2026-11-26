# STM32 Tabanlı Trafo Inrush Akımının 2. Harmonik Analizi ile Tespiti

Bu depo, TÜBİTAK 2209-A projesi kapsamındaki simülasyon kodlarını, STM32 gömülü yazılım dosyalarını ve literatür notlarını barındırmaktadır.

## Araştırma sorusu ve başarı ölçütü

* **Araştırma Sorusu:** Laboratuvar ölçekli tek fazlı bir transformatörde oluşan inrush ve kısa devre arıza akımları, düşük maliyetli STM32 mikrodenetleyicisi üzerinde çalışan 2. harmonik ($I_2/I_1$) tabanlı FFT algoritması ile bir şebeke çevrimi (20 ms) içinde hangi doğruluk oranıyla ayırt edilebilir?
* **Başarı Ölçütü ve Hedef Değer:** Doğru sınıflandırma oranının **en az %90** olması (50 inrush ve 50 kısa devre test sinyalinden oluşan toplam 100 denemede 20 ms içinde en az 90 doğru tespit).
