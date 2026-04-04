# 01 — Arduino Uno

---

## Bu nedir?

Arduino Uno, küçük bir **bilgisayar kartıdır** — ama normal bilgisayardan çok farklıdır.

Normal bilgisayar: internete girer, video oynatır, çok iş yapar.  
Arduino Uno: sadece **sen ne söylersen onu yapar**. Tek bir görev için programlanır ve onu tekrar tekrar yapar.

Düşün ki Arduino bir **robot beyin**. Robota "her 2 saniyede bir LED'i yak-söndür" diyorsun. Arduino bunu sonsuza kadar yapar, durmaz, şikayet etmez.

---

## Nasıl çalışır?

İçinde **ATmega328P** adında bir mikrodenetleyici çip var. Bu çip:

- Senin yazdığın kodu hafızasına kaydeder
- Elektrik verildiği sürece o kodu çalıştırır
- Pinler aracılığıyla dış dünyayla konuşur (sensör okur, LED yakar, motor döndürür)

### Güç kaynakları

Arduino'ya 3 farklı şekilde güç verebilirsin:

| Yöntem | Nasıl? | Ne zaman kullanılır? |
|--------|--------|----------------------|
| USB kablosu | Bilgisayara bağla | Geliştirme sırasında |
| DC barrel jack | 7–12V adaptör | Bağımsız proje çalıştırırken |
| Vin pini | Breadboard'dan 7–12V | Harici güç, breadboard üzerinden |

> **Tavsiye (2024):** Geliştirme sırasında her zaman USB kullan. Pil ile çalıştırmak istiyorsan 9V pil + barrel jack konnektörü en güvenli seçenek.

---

## Kartın üzerindeki her şey ne işe yarar?

```
        [USB Port]   [DC Jack]
        |                |
        v                v
   ┌────────────────────────────┐
   │  [RST]              [ICSP] │
   │                            │
   │  Digital Pinler (0-13)     │
   │  ┌──┬──┬──┬──┬──┬──┬──┐   │
   │  │0 │1 │2 │3 │4 │5 │6 │   │
   │  └──┴──┴──┴──┴──┴──┴──┘   │
   │  ┌──┬──┬──┬──┬──┬──┬──┐   │
   │  │7 │8 │9 │10│11│12│13│   │
   │  └──┴──┴──┴──┴──┴──┴──┘   │
   │                            │
   │  [ATmega328P]  [16MHz]     │
   │                            │
   │  Analog Pinler (A0-A5)     │
   │  ┌──┬──┬──┬──┬──┬──┐      │
   │  │A0│A1│A2│A3│A4│A5│      │
   │  └──┴──┴──┴──┴──┴──┘      │
   │                            │
   │  [GND][5V][3.3V][Vin]     │
   └────────────────────────────┘
```

### Digital Pinler (0–13)

- Sadece iki durumu var: **HIGH (5V)** veya **LOW (0V)**
- Yani: açık ya da kapalı. 1 ya da 0.
- Pin 13: Kartın üzerinde **dahili LED** var! Bağlantı gerektirmez, test için mükemmel.
- **~** işareti olan pinler (3, 5, 6, 9, 10, 11): **PWM** yapabilir → sanki analog gibi davranabilir (örn: LED'i %50 parlaklığa ayarlamak)

### Analog Pinler (A0–A5)

- 0 ile 1023 arasında **değer okur** (10-bit çözünürlük)
- Potansiyometre, LDR, sıcaklık sensörü gibi elemanları buraya bağlarsın
- A4 ve A5: ayrıca **I2C iletişimi** için kullanılır (LCD, sensör modülleri)

### Güç Pinleri

| Pin | Voltaj | Ne için? |
|-----|--------|----------|
| 5V | 5 Volt | Sensörlere, modüllere güç ver |
| 3.3V | 3.3 Volt | Bazı hassas modüller için |
| GND | 0 Volt (toprak) | Her devrenin eksi ucuna bağlanır |
| Vin | Girdi voltajı | Barrel jack'ten gelen voltaj buradan çıkar |

### Diğer bileşenler

- **RST (Reset) butonu:** Kodu baştan başlatır. Hafızayı SİLMEZ, sadece yeniden çalıştırır.
- **TX/RX LED'leri:** Seri port üzerinden veri gidip gelirken yanıp söner.
- **ICSP başlığı:** İleri seviye programlama için (şimdilik görmezden gel).
- **Kristal osilatör (16 MHz):** Kartın "kalbi". Her saniye 16 milyon adım atar.

---

## Teknik özellikler

| Özellik | Değer |
|---------|-------|
| Mikrodenetleyici | ATmega328P |
| Çalışma voltajı | 5V |
| Giriş voltajı (önerilen) | 7–12V |
| Dijital I/O pini | 14 (6 tanesi PWM) |
| Analog giriş pini | 6 |
| Pin başına max akım | 40 mA |
| Flash bellek | 32 KB (0.5 KB bootloader) |
| SRAM | 2 KB |
| EEPROM | 1 KB |
| Saat hızı | 16 MHz |

---

## Programlama

Arduino'yu **Arduino IDE** ile programlarsın. 2025 itibarıyla **Arduino IDE 2.x** kullanmanı öneririm — otomatik tamamlama ve hata gösterimi çok daha iyi.

### Temel kod yapısı

Her Arduino kodunun iki zorunlu fonksiyonu var:

```cpp
void setup() {
  // Başlangıçta BİR KEZ çalışır
  // Pin modlarını burada ayarlarsın
  pinMode(13, OUTPUT);
}

void loop() {
  // setup() bittikten sonra SONSUZA KADAR döner
  // Asıl iş buraya yazılır
  digitalWrite(13, HIGH);  // LED yak
  delay(1000);              // 1 saniye bekle
  digitalWrite(13, LOW);   // LED söndür
  delay(1000);              // 1 saniye bekle
}
```

> **Programlama dili:** C++ tabanlı, ama sadeleştirilmiş hali. Python biliyorsan sözdizimi farklı gelecek ama mantık aynı.

### Sık kullanılan fonksiyonlar

```cpp
pinMode(pin, OUTPUT);        // Pin'i çıkış yap
pinMode(pin, INPUT);         // Pin'i giriş yap
pinMode(pin, INPUT_PULLUP);  // Dahili pull-up dirençli giriş

digitalWrite(pin, HIGH);     // Pin'i 5V yap
digitalWrite(pin, LOW);      // Pin'i 0V yap
digitalRead(pin);            // Pin durumunu oku (0 veya 1)

analogRead(pin);             // Analog değer oku (0–1023)
analogWrite(pin, value);     // PWM yaz (0–255, sadece ~ pinlerde)

delay(ms);                   // Milisaniye bekle
millis();                    // Başlangıçtan bu yana geçen ms
Serial.begin(9600);          // Seri port aç
Serial.println("Merhaba");   // Seri porta yaz
```

---

## Güncel tavsiyeler (2025)

### Hangi Arduino'yu almalısın?

| Kart | Tavsiye |
|------|---------|
| **Arduino Uno R3** | Başlangıç için klasik seçim, en çok kaynak var |
| **Arduino Uno R4 Minima** | 2023'te çıktı, daha güçlü, aynı form factor |
| **Arduino Uno R4 WiFi** | WiFi + LED matrix dahili, IoT projeleri için |
| Klon Uno (CH340 çipli) | Çok ucuz, öğrenme için yeterli ama sürücü kurulumu gerekli |

> **Öneri:** Eğer bütçen kısıtlıysa CH340 çipli Uno klonu al (20-50 TL). Eğer orijinal istiyorsan **Uno R4 Minima** al — R3 ile aynı fiyata daha güçlü.

### IDE kurulumu

1. [arduino.cc/en/software](https://www.arduino.cc/en/software) adresinden **Arduino IDE 2.x** indir
2. Kart bağla, IDE'de `Tools > Board > Arduino Uno` seç
3. `Tools > Port` menüsünden doğru portu seç (CH340 klonu ise önce sürücü kur)
4. `File > Examples > 01.Basics > Blink` aç → Upload et → PIN 13'teki LED yanıp sönmeye başlar

### Kütüphane yönetimi

IDE 2.x'te `Tools > Manage Libraries` ile kütüphane ekleyebilirsin. Sık kullanılan başlangıç kütüphaneleri:

- `DHT sensor library` (Adafruit) — sıcaklık sensörü
- `Servo` — servo motor
- `LiquidCrystal I2C` — LCD ekran
- `NewPing` — ultrasonik mesafe sensörü

---

## Karıştırılabilecek noktalar — DİKKAT!

> ⚠️ **5V pininden fazla akım çekme!**  
> Arduino'nun 5V pini toplamda **200 mA** verebilir. Fazla sensör/modül bağlarsan kart zarar görür. Çok sayıda modül kullanacaksan harici güç kaynağı ekle.

> ⚠️ **Pin başına 40 mA sınırı**  
> Tek bir pine doğrudan motor bağlama! Motor çok akım çeker, pini yakar. Motor için transistör veya motor sürücü kullan.

> ⚠️ **3.3V ve 5V'u karıştırma**  
> Bazı sensörler (ESP8266, bazı OLED'ler) 3.3V ile çalışır. 5V verirsen yanar. Her modülün çalışma voltajını kontrol et.

> ⚠️ **TX (pin 0) ve RX (pin 1)'i kullanırken dikkat**  
> Bu pinler USB seri iletişimi için de kullanılır. Upload sırasında bu pinlere bir şey bağlıysa yükleme başarısız olur.

> ⚠️ **Klon kartlarda CH340 sürücüsü gerekebilir**  
> Bilgisayar kartı tanımıyorsa: [wch-ic.com](http://www.wch-ic.com/downloads/CH341SER_ZIP.html) adresinden sürücü indir.

---

## Kolay hatırlama notları

💡 **"Arduino = ahmak ama çok çalışkan işçi"**  
Kendisi düşünmez ama sen ne söylersen onu mükemmel yapar. Bir işi 10 milyon kez yaptırsan bile yorulmaz.

💡 **"Digital = ışık anahtarı, Analog = dimmer"**  
Digital pinler ya açık ya kapalı. Analog pinler 0 ile 1023 arasında her değeri okuyabilir — tıpkı bir dimmer'ın ışığı kademeli kısması gibi.

💡 **"setup() ev toplama, loop() nefes alma"**  
setup() bir kez çalışır, hazırlık yapar. loop() ise Arduino açık olduğu sürece durmadan döner — nefes almak gibi.

💡 **"Pin 13 = ücretsiz test LED'i"**  
Karta LED, direnç bağlamadan `digitalWrite(13, HIGH)` yazarsan kartın üzerindeki LED yanar. İlk testler için biçilmiş kaftan.

💡 **"GND olmadan hiçbir şey çalışmaz"**  
Devreyi ne kadar doğru kursan kur, GND bağlamayı unutursan çalışmaz. Her devrenin eksi ucu GND'ye gitmeli.

---

## Başlangıç için önerilen ilk 5 proje

1. **Blink** — Pin 13 LED'ini yak-söndür (tek satır kod yeterli)
2. **Buton ile LED** — Butona basınca LED yansın
3. **Seri port mesajı** — `Serial.println()` ile bilgisayara mesaj gönder
4. **Potansiyometre oku** — Analog değeri seri porta yazdır
5. **Fade (solma)** — PWM ile LED'i kademeli yak-söndür

---

*Sonraki komponent: [02 — Breadboard](02_breadboard.md)*
