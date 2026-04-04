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

---

#### 🔌 Pin Modu Ayarlama — `pinMode()`

Bir pini kullanmadan önce ona "sen giriş misin, çıkış mısın?" diye sorman gerekir.

```cpp
pinMode(13, OUTPUT);       // Pin 13'ten elektrik VER (LED yak, buzzer çal)
pinMode(2,  INPUT);        // Pin 2'den elektrik OKU (buton, sensör)
pinMode(2,  INPUT_PULLUP); // Pin 2'den oku, ama dahili direnç ile sabitle
```

> **INPUT_PULLUP ne demek?**  
> Butonu doğrudan pine bağladığında pin "havada kalır" — ne 0 ne 1, belirsiz bir değer okur. INPUT_PULLUP bunu önler: buton basılı değilken pin HIGH okur, basılıyken LOW okur. Dışarıdan direnç bağlamana gerek kalmaz.

```cpp
// INPUT_PULLUP kullanımı — buton GND'ye bağlı
void setup() {
  pinMode(2, INPUT_PULLUP);
}
void loop() {
  if (digitalRead(2) == LOW) {   // LOW = butona basılı!
    // bir şey yap
  }
}
```

---

#### ⚡ Dijital Yazma — `digitalWrite()`

OUTPUT modundaki pine 5V (HIGH) veya 0V (LOW) uygular.

```cpp
digitalWrite(13, HIGH);  // Pin 13'ü 5V yap  → LED yanar
digitalWrite(13, LOW);   // Pin 13'ü 0V yap  → LED söner

// HIGH ve LOW yerine 1 ve 0 da yazabilirsin, aynı şey:
digitalWrite(13, 1);     // HIGH ile aynı
digitalWrite(13, 0);     // LOW ile aynı
```

---

#### 📖 Dijital Okuma — `digitalRead()`

INPUT modundaki pindeki voltajı okur. Sonuç ya `HIGH (1)` ya `LOW (0)`.

```cpp
int durum = digitalRead(2);  // Pindeki değeri oku

if (durum == HIGH) {
  Serial.println("Buton basılı değil");
} else {
  Serial.println("Butona basıldı!");
}
```

---

#### 🎛️ Analog Okuma — `analogRead()`

Sadece A0–A5 pinlerinde çalışır. 0V → 0, 5V → 1023 döndürür. Aradaki her değeri okuyabilir.

```cpp
int deger = analogRead(A0);   // 0 ile 1023 arası bir sayı gelir

// Voltaja çevirmek istersen:
float voltaj = deger * (5.0 / 1023.0);
Serial.println(voltaj);        // Örn: 2.47 (volt cinsinden)
```

```cpp
// Pratik örnek: Potansiyometre ile LED parlaklığı
void loop() {
  int pot = analogRead(A0);           // 0–1023 oku
  int parlaklik = pot / 4;            // 0–255'e küçült (1023/4 ≈ 255)
  analogWrite(9, parlaklik);          // PWM ile LED'e uygula
}
```

> **Neden 0–1023?** Arduino 10 bitlik ADC kullanır. 2^10 = 1024 farklı değer → 0'dan 1023'e kadar.

---

#### 〰️ Analog Yazma (PWM) — `analogWrite()`

Gerçek analog sinyal üretmez. Dijital pini çok hızlı açıp kapatarak analog **etkisi** yaratır. Buna **PWM (Pulse Width Modulation)** denir.

Sadece `~` işaretli pinlerde çalışır: **3, 5, 6, 9, 10, 11**

```cpp
analogWrite(9, 0);    // Tamamen kapalı (0V etkisi)
analogWrite(9, 127);  // Yarı güç (%50 — örn: LED soluk yanar)
analogWrite(9, 255);  // Tam güç (5V etkisi)
```

```cpp
// Soluyan LED örneği
void loop() {
  for (int i = 0; i <= 255; i++) {
    analogWrite(9, i);   // Kademeli parlat
    delay(10);
  }
  for (int i = 255; i >= 0; i--) {
    analogWrite(9, i);   // Kademeli söndür
    delay(10);
  }
}
```

> ⚠️ **Dikkat:** `analogWrite()` ile **pin modunu OUTPUT yapmana gerek yok** — fonksiyon bunu otomatik ayarlar. Ama yine de yazmak iyi alışkanlıktır.

---

#### ⏱️ Zaman Fonksiyonları — `delay()` ve `millis()`

**`delay(ms)`** — Kodu durdurur. Basit ama kör.

```cpp
delay(1000);    // 1 saniye dur (1000 milisaniye)
delay(500);     // 0.5 saniye dur
delay(50);      // 50 milisaniye dur
```

> ⚠️ **delay() kullanırken dikkat:** delay() çalışırken Arduino HİÇBİR ŞEY yapamaz. Buton okuyamaz, sensör kontrol edemez. Birden fazla iş aynı anda yapılacaksa `millis()` kullan.

**`millis()`** — Arduino açıldığından bu yana geçen milisaniyeyi verir. delay() kullanmadan zamanlama yapmanı sağlar.

```cpp
// millis() ile LED yakıp söndürme — delay() olmadan
unsigned long oncekiZaman = 0;
int ledDurumu = LOW;

void loop() {
  unsigned long simdikiZaman = millis();

  if (simdikiZaman - oncekiZaman >= 1000) {  // 1 saniye geçti mi?
    oncekiZaman = simdikiZaman;
    ledDurumu = (ledDurumu == LOW) ? HIGH : LOW;
    digitalWrite(13, ledDurumu);
  }
  // Buraya başka kodlar da yazılabilir — Arduino bloklanmaz!
}
```

**`micros()`** — millis() gibi ama mikrosaniye cinsinden. Çok hassas zamanlama gerektiğinde kullanılır.

```cpp
unsigned long sure = micros();  // Mikrosaniye cinsinden zaman
```

---

#### 📡 Seri Port — `Serial`

Arduino ile bilgisayar arasında metin tabanlı iletişim. Debug (hata ayıklama) için olmazmazın olmazı.

```cpp
void setup() {
  Serial.begin(9600);   // 9600 baud hızında seri port aç
                        // IDE ile aynı hız olmalı!
}

void loop() {
  Serial.print("Değer: ");        // Satır sonu OLMADAN yaz
  Serial.println(analogRead(A0)); // Değer yaz + satır sonu ekle
  Serial.println("---");
  delay(500);
}
```

**Sık kullanılan Serial fonksiyonları:**

```cpp
Serial.begin(9600);          // Portu aç (setup'ta bir kez)
Serial.print("metin");       // Yaz, satır atma
Serial.println("metin");     // Yaz + satır at (\n ekler)
Serial.println(sayi);        // Sayı yazdır
Serial.println(sayi, HEX);   // Hexadecimal yazdır (örn: FF)
Serial.println(sayi, BIN);   // Binary yazdır (örn: 11001100)
Serial.available();          // Okunmayı bekleyen byte sayısı
Serial.read();               // Bir byte oku
```

```cpp
// Bilgisayardan Arduino'ya komut gönderme
void loop() {
  if (Serial.available() > 0) {     // Gelen veri var mı?
    char gelen = Serial.read();      // Bir karakteri oku
    if (gelen == 'a') {
      digitalWrite(13, HIGH);        // 'a' gelirse LED yak
    } else if (gelen == 's') {
      digitalWrite(13, LOW);         // 's' gelirse söndür
    }
  }
}
```

> **Serial Monitor nasıl açılır?**  
> Arduino IDE'de sağ üstteki büyüteç ikonuna tıkla veya `Ctrl+Shift+M`. Alt kısımdan baud hızını seçmeyi unutma — `Serial.begin()` içine yazdığınla aynı olmalı.

---

#### 🗺️ Değer Dönüştürme — `map()`

Bir sayı aralığını başka bir aralığa dönüştürür. Çok sık kullanılır.

```cpp
// map(değer, eskiMin, eskiMax, yeniMin, yeniMax)

int pot = analogRead(A0);              // 0–1023 geliyor
int parlaklik = map(pot, 0, 1023, 0, 255);  // 0–255'e çevir
analogWrite(9, parlaklik);

// Başka örnek: sensör değerini açıya çevir
int aci = map(analogRead(A0), 0, 1023, 0, 180);
```

---

#### 📐 Sınırlama — `constrain()`

Bir değerin belirli aralığın dışına çıkmasını önler.

```cpp
int deger = analogRead(A0);
deger = constrain(deger, 100, 900);  // 100'den küçükse 100, 900'den büyükse 900 yap

// map() ile birlikte kullanımı
int temiz = constrain(analogRead(A0), 0, 1023);
int cikis = map(temiz, 0, 1023, 0, 255);
```

---

#### 🔢 Matematik Fonksiyonları

```cpp
abs(-5);           // Mutlak değer → 5
min(3, 7);         // Küçüğü seç → 3
max(3, 7);         // Büyüğü seç → 7
sq(4);             // Karesi → 16
sqrt(16);          // Karekök → 4.0
pow(2, 8);         // Üs alma → 256.0
random(10);        // 0–9 arası rastgele sayı
random(5, 10);     // 5–9 arası rastgele sayı
```

---

#### 💾 Değişken Tipleri (hızlı referans)

Arduino'da değişken tipi seçmek bellek açısından önemlidir — SRAM sadece 2 KB!

```cpp
int     sayi    = 42;         // -32768 ile 32767 arası tam sayı (2 byte)
long    buyuk   = 100000;     // Çok büyük tam sayı (4 byte)
float   ondalik = 3.14;       // Ondalıklı sayı (4 byte, yavaş)
bool    durum   = true;       // true veya false (1 byte)
char    harf    = 'A';        // Tek karakter (1 byte)
String  metin   = "Merhaba";  // Metin (dinamik, bellek yer)
byte    kucuk   = 255;        // 0–255 arası (1 byte, tasarruflu)

// ⚠️ String yerine char[] kullanmak belleği daha verimli kullanır:
char metin[] = "Merhaba";
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
