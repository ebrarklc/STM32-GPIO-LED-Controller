# STM32 GPIO ile Dijital Kontrol ve LED Animasyonu 

Bu proje, STM32F407 mikrodenetleyicisinin temel taşı olan **GPIO (General Purpose Input Output)** biriminin hem "Dijital Çıkış" (LED kontrolü) hem de "Dijital Giriş" (Anahtar okuma) yeteneklerini donanım seviyesinde incelemek amacıyla tasarlanmıştır[cite: 7]. 

Gömülü sistem programlamanın klasik "Kara Şimşek" efekti, sisteme eklenen anlık kullanıcı girdileriyle (Hızlandırma, Durdurma, Tümünü Yakma) dinamik bir yapıya kavuşturulmuştur[cite: 7].

## 🚀 Öne Çıkan Özellikler (Highlights)

*   **Push-Pull Donanım Sürme:** Harici devrelere (Örn: LED) güç sağlayabilmek için çıkış pinleri donanımsal olarak **Push-Pull (İt-Çek)** modunda konfigüre edilmiştir[cite: 7].
*   **Pull-Up/Pull-Down Kavramları:** Giriş pinlerinde "Floating (Yüzen Pin)" tehlikesini önlemek amacıyla donanımsal mantık kurulmuştur[cite: 7]. (ArmApp-18 setinde harici dirençler bulunduğu için yazılımda `No Pull-up/No Pull-down` tercih edilmiştir)[cite: 7].
*   **Dinamik Animasyon Kontrolü:** Kod algoritması; `HAL_GPIO_ReadPin` ile anlık anahtar durumlarını okuyarak animasyonu durdurabilen (Pause), hızlandırabilen veya tüm LED'leri SET edebilen durum makineleri barındırır[cite: 7].

## 🛠️ Donanım ve Pin Yapılandırması

Sistemdeki dış birimlerin STM32 üzerindeki pin atamaları ve yapılandırmaları aşağıdaki gibidir[cite: 7]:

| Bileşen | Pin Kodu | Kullanım Modu | Açıklama |
| :--- | :--- | :--- | :--- |
| **LED 0-7** | `PE0-PE7` | GPIO_Output | 8 adet kırmızı LED'in sürülmesi için çıkış pinleri[cite: 7]. |
| **Switch 0 (SW0)** | `PE13` | GPIO_Input | Tüm LED'leri yakma (Set All) giriş pini[cite: 7]. |
| **Switch 1 (SW1)** | `PE14` | GPIO_Input | Animasyonu olduğu yerde dondurma (Pause) giriş pini[cite: 7]. |
| **Switch 2 (SW2)** | `PE15` | GPIO_Input | Animasyon hızını artırma (Speed Up) giriş pini[cite: 7]. |

## 📂 Yazılım Mimarisi (Algoritma)

Sistem sonsuz bir döngü (`while(1)`) içerisinde çalışır ve donanımı kontrol etmek için temel HAL kütüphanesi fonksiyonlarını (`HAL_GPIO_WritePin`, `HAL_GPIO_ReadPin`, `HAL_Delay`) kullanır[cite: 7].

1.  **İleri Yön Döngüsü:** LED0'dan LED7'ye doğru sırayla yanar[cite: 7]. Her adımdan önce anahtarlar (SW0, SW1, SW2) kontrol edilir[cite: 7].
2.  **Geri Yön Döngüsü:** LED7'den LED0'a doğru sırayla yanar[cite: 7]. Her adımdan önce anahtarlar (SW0, SW1, SW2) kontrol edilir[cite: 7].
3.  **Kullanıcı Girdileri:**
    *   **SW0 (PE13):** SET durumundayken `while` döngüsüne girilip tüm LED'lere `GPIO_PIN_SET` gönderilir[cite: 7].
    *   **SW1 (PE14):** SET durumundayken kod satırında sonsuz bir bekleme yaratılarak animasyon durdurulur[cite: 7].
    *   **SW2 (PE15):** SET durumundayken `beklemeSuresi` değişkeni `50ms`'ye düşürülerek animasyon hızlandırılır[cite: 7].

## 💻 Nasıl Çalıştırılır?

1.  Projeyi `STM32CubeIDE` ile açın.
2.  Dahili osilatör (HSI) ayarlarının yapıldığından emin olun (Proje varsayılan 16 MHz ile çalışmaktadır)[cite: 7].
3.  Kodu derleyin (`Build`) ve ArmApp-18 üzerindeki STM32F407 kartına yükleyin[cite: 7].
4.  Varsayılan durumda LED animasyonu yavaş hızda (250ms) çalışacaktır[cite: 7].
5.  Geliştirme seti üzerindeki `SW0`, `SW1` ve `SW2` anahtarlarını açıp kapatarak yazılan algoritmik kontrol mekanizmalarını test edebilirsiniz[cite: 7].

<img width="1024" height="683" alt="image" src="https://github.com/user-attachments/assets/c7a9daa7-8b9e-49f8-b151-c10c74709786" />
