# 02 — Breadboard (Deney Tahtası)

---

## Bu nedir?

Breadboard, **lehim kullanmadan** elektronik devre kurmanı sağlayan plastik bir tahta.

Normalde iki elektronik bileşeni birbirine bağlamak için lehim makinesiyle telleri birbirine kaynatman gerekir. Bu hem zaman alır hem de hata yapınca geri dönmek çok zordur.

Breadboard ile hiçbir şeyi kaynatmıyorsun — bileşenleri deliklere takıyorsun, çıkarmak istersen çekip alıyorsun. Düşün ki **LEGO gibi**: yaparsın, beğenmezsin, sökersin, yeniden yaparsın.

---

## Nasıl çalışır? — İçindeki gizli bağlantılar

Breadboard'un dışarıdan sadece delikler göründüğünü görürsün. Ama içinde **metal klipsler** var — ve bu klipsler bazı delikleri birbirine bağlıyor.

İşte burada çoğu kişi karışıyor. Hangi delikler birbirine bağlı, hangileri bağlı değil?

```
  BREADBOARD İÇ BAĞLANTI HARİTASI
  
  (+) ────────────────────────────  ← Güç rayı (tüm delikler yatay bağlı)
  (-) ────────────────────────────  ← GND rayı  (tüm delikler yatay bağlı)
  
       a  b  c  d  e    f  g  h  i  j
  1  [ ][ ][ ][ ][ ]  [ ][ ][ ][ ][ ]
  2  [ ][ ][ ][ ][ ]  [ ][ ][ ][ ][ ]
  3  [ ][ ][ ][ ][ ]  [ ][ ][ ][ ][ ]
  4  [ ][ ][ ][ ][ ]  [ ][ ][ ][ ][ ]
       ↑↑↑↑↑↑↑↑↑↑      ↑↑↑↑↑↑↑↑↑↑
    bu 5'ler dikey    bu 5'ler dikey
       bağlı             bağlı
    (a1-b1-c1-d1-e1)  (f1-g1-h1-i1-j1)
    ORTADAKĠ KANAL İKİ TARAFI AYIRIR!

  (-) ────────────────────────────  ← GND rayı
  (+) ────────────────────────────  ← Güç rayı
```

### Kural 1 — Orta alan: dikey bağlantı

Harfler (a, b, c, d, e) aynı satır numarasındaysa birbirine bağlı.

Yani `a1`, `b1`, `c1`, `d1`, `e1` — hepsi birbirine bağlı. Birine bir şey bağlarsan diğerlerine de iletilir.

Ama `a1` ile `a2` **bağlı değil**. Satır numarası farklı olduktan sonra bağlantı yok.

### Kural 2 — Orta kanal iki tarafı ayırır

Breadboard'un tam ortasında bir boşluk (kanal) var. Bu kanal fiziksel olarak iki tarafı birbirinden ayırır.

`e1` ile `f1` **bağlı değil** — ortadaki kanaldan dolayı.

Bu kanalın amacı: entegre devre (IC) çiplerini tam ortaya oturtmak. Çipin iki tarafındaki pinler birbirine kısa devre yapmaz.

### Kural 3 — Güç rayları: yatay bağlantı

Kenar boyunca uzanan kırmızı (+) ve mavi/siyah (-) şeritler tamamen farklı çalışır: **yatay bağlı**.

Kırmızı rayın herhangi bir deliğine 5V bağlarsan, o rayın tüm delikleri 5V olur. Mavi/siyah ray GND için aynı şekilde çalışır.

> ⚠️ **Dikkat:** Bazı büyük breadboard'larda güç rayı ortadan ikiye bölünmüştür — iki yarı birbirine bağlı değildir! Rayın tam ortasında küçük bir boşluk olup olmadığını kontrol et.

---

## Breadboard türleri

| Tür | Delik sayısı | Ne için? |
|-----|-------------|----------|
| Mini (half-size) | 400 delik | Küçük, basit devreler |
| Normal (full-size) | 830 delik | Standart projeler — başlangıç için ideal |
| Büyük (double) | 1660+ delik | Karmaşık, çok bileşenli devreler |

> **Tavsiye (2025):** 830 delikli tam boy breadboard al. Mini olanlar hızla dolup taşıyor ve bileşenler sıkışıyor.

---

## Jumper kablo renk geleneği

Breadboard ile birlikte jumper kablo kullanırsın. Renklerin teknik bir zorunluluğu yok — ama herkesin uyduğu bir gelenek var:

| Renk | Kullanım |
|------|----------|
| Kırmızı | 5V / güç hattı |
| Siyah | GND / toprak |
| Sarı / Turuncu | Sinyal hattı |
| Mavi / Yeşil | Diğer bağlantılar |

> Bu geleneğe uymak zorunda değilsin — ama uyarsan devreni hem sen hem başkası çok daha kolay okur. Özellikle kırmızı = güç, siyah = GND kuralını asla bozma.

---

## İlk bağlantı — Adım adım

Arduino ile breadboard'u bağlamanın standart yolu:

```
Arduino          Breadboard
   5V ──────────► (+) kırmızı ray
  GND ──────────► (-) mavi/siyah ray
```

Bu iki bağlantıyı yaptıktan sonra breadboard'daki her bileşene (+) rayından güç, (-) rayından GND verebilirsin. Her seferinde Arduino'ya dönmene gerek kalmaz.

### LED bağlama örneği

```
Arduino pin 13 ──► breadboard'da bir sıraya gir
                   aynı sıraya 220Ω direnç tak
                   direncin diğer ucu ──► LED'in uzun bacağı (anot)
                   LED'in kısa bacağı ──► (-) GND rayı
```

```
  PIN 13 ─── [220Ω] ─── [LED+] [LED-] ─── GND
```

---

## Breadboard üzerinde bileşen yerleştirme kuralları

**Entegre devre (IC/chip) takma:**
Çipi tam ortaya, kanalın üzerine gelecek şekilde otur. Bacaklar iki tarafa açılır, kısa devre olmaz.

```
  e  f
[ ][ ]   ← kanal
[IC ]    ← chip tam ortaya
[ ][ ]
```

**Direnç takma:**
Dirençlerin yönü önemli değil — iki yönde de çalışır. Satır satır yerleştir.

**LED takma:**
LED'in yönü önemli! Uzun bacak (+, anot) kaynağa; kısa bacak (-, katot) GND'ye.

**Kondansatör takma:**
Elektrolitik kondansatörlerin yönü var — artı bacağını (+) tarafa tak.

---

## Karıştırılabilecek noktalar — DİKKAT!

> ⚠️ **En yaygın hata: ortadaki kanalı görmemek**  
> `e` ile `f` aynı satırda olsa da bağlı değil! Birçok başlangıç seviyesindeki kişi bunu atlıyor ve devre çalışmıyor. Her zaman hangi deliklerin bağlı olduğunu zihninde canlandır.

> ⚠️ **Güç rayının ortadan bölündüğünü kontrol et**  
> Bazı 830 delikli breadboard'larda kırmızı ve mavi raylar tam ortada kesilir. Üst yarı ve alt yarı bağlı değildir. Her iki yarıya da güç vermeyi unutursan devrenin bir kısmı çalışmaz.

> ⚠️ **Bileşen bacakları tam oturmalı**  
> Deliğe yarım giren bacak temas etmeyebilir. Tıklama sesi duyana kadar hafifçe bastır — ama zorla itme, breadboard içindeki metal klips bozulabilir.

> ⚠️ **Kalın tel kullanma**  
> Breadboard delikleri ince tel için tasarlanmış. Çok kalın tel sıkışır, klipsi genişletir ve o delik bir daha düzgün temas etmez.

> ⚠️ **Uzun süreli kullanımda delikler gevşer**  
> Breadboard bir süre sonra yıpranır, delikler gevşer ve temas sorunları başlar. Özellikle ucuz klonlarda bu çok hızlı olur. Temas sorunu şüpheliysen jumper kabloyu biraz hareket ettir — sorun kayboluyorsa delik gevşektir.

> ⚠️ **Breadboard akımı sınırlar**  
> Her delik yaklaşık 1A akım taşıyabilir. Motor gibi yüksek akım çeken bileşenler için doğrudan kablo kullan, breadboard üzerinden besleme.

---

## Kolay hatırlama notları

💡 **"Aynı satır = aynı nokta"**  
a3, b3, c3, d3, e3 — bunların hepsi elektriksel olarak aynı yerdir. Biri 5V ise hepsi 5V'tur.

💡 **"Kanal = nehir — iki yakayı birbirinden ayırır"**  
Ortadaki boşluk bir nehir gibi düşün. e tarafı bir yakada, f tarafı öbür yakada. Köprü (jumper kablo) kurmadan geçiş olmaz.

💡 **"Kırmızı = kan (güç), siyah = toprak (GND)"**  
Kırmızı damardan kan (elektrik) gelir, siyah ise toprağa iner. Bu renk geleneğini asla tersine çevirme.

💡 **"Breadboard = taslak kağıdı"**  
Devreyi önce breadboard'da test et, çalışınca kalıcı hale getir (PCB, lehim). Hata yapmaktan korkma — zaten bunun için var.

💡 **"IC'yi kanalın ortasına"**  
Entegre devre çipleri her zaman tam ortaya, iki tarafı kanala denk gelecek şekilde takılır. Aklında tut.

---

## Breadboard alırken dikkat edilecekler (2025)

- **Marka:** Elegoo, Elenco, Adafruit güvenilir markalar. Çok ucuz isimsiz klonlardan kaçın — delikler gevşek olur.
- **Kenar bantlı mı?** Altında yapışkanlı bant olan modeller masaya yapışır, kayıp bileşen arama derdinden kurtarır.
- **Renk kodlu ray mı?** Kırmızı/mavi raylı olanlar tercih et — yanlış bağlantı riskini azaltır.
- **Jumper kablo seti ile gelen paketler** daha ekonomik — ayrı almana gerek kalmaz.

---

*Önceki komponent: [01 — Arduino Uno](01_arduino_uno.md)*  
*Sonraki komponent: [03 — Jumper Kablo](03_jumper_kablo.md)*
