---
name: ihlal-triaji
description: >
  Veri ihlali olayını triyaj eder. KVKK'ya 72 saatlik bildirim yapılması gerekip
  gerekmediğini değerlendirir, ilgili kişilere bildirim eşiğini kontrol eder,
  bildirim metinlerini taslar, KVKK kararlarından emsal getirir. "Veri ihlali
  oldu", "data breach", "bildirim hazırla", "72 saat sayacı", "veri sızıntısı"
  söylemlerinde tetiklenir.
argument-hint: "[--olay <dosya>] [--ivedi: zaman kritik]"
---

# /kvkk-uyum-tr:ihlal-triaji

## Cold-start kontrolü
Profil yoksa interview'a yönlendir.

> ⏰ **Bu skill zaman kritiktir.** KVKK m.12/5 uyarınca veri ihlali "en kısa sürede" ve **72 saat** içinde bildirilmelidir. Olay tespit anına saatleri saymaya başla.

## İş Akışı

### 1. Olay bilgisi topla

Kullanıcıdan veya olay raporu dosyasından:
- **Olay tespit zamanı:** Tarih + saat (TR saati)
- **Olay özeti:** Ne oldu? (sızıntı / yetkisiz erişim / kayıp / hata / şifreleme bypass / saldırı)
- **Etkilenen veri:**
  - Veri kategorileri (kimlik, iletişim, finansal, sağlık, vb.)
  - Özel nitelikli mi? (artırılmış risk)
  - Etkilenen kişi sayısı (~yaklaşık)
- **Veri sorumlusu kim?** (siz mi, veri işleyen müşteri mi?)
- **Olay süresi:** Ne kadar zaman boyunca veri ifşa kaldı?
- **Erişen kim?** (iç tehdit / dış saldırgan / belirsiz)
- **Mevcut durum:** Hâlâ devam ediyor mu, durduruldu mu?

### 2. 72 saat sayacı

```
Olay tespiti: 2026-05-30 14:30
KVKK bildirim sınırı: 2026-06-02 14:30 (72 saat sonra)
Şu an: <çalıştırma anındaki tarih+saat>
Kalan süre: X saat Y dakika
```

Renk kodu:
- > 48 saat → 🟢
- 24-48 saat → 🟡
- < 24 saat → 🚨 KIRMIZI

### 3. Bildirim eşiği kararı

**KVKK m.12/5 + 2019/10 sayılı Kurul kararı:**
- Veri ihlali "kişilerin hak ve özgürlükleri açısından risk doğuruyorsa" KVKK'ya bildirilmeli.
- "Yüksek risk" varsa **ilgili kişilere de** bildirim yapılmalı (m.12/5).

**Yüksek risk göstergeleri:**
- Özel nitelikli veri (sağlık, biyometrik, ceza, din, etnik)
- Finansal veri (kart, hesap, kimlik birleşimi)
- Çocuk verisi
- Kimliği tek başına anonim olmayan kombinasyonlar
- Kişiyi tanımlanabilir kılan + ciddi sonuç doğurabilir kombinasyon

Karar tablosu:
| Durum | KVKK Bildirim | İlgili Kişi Bildirim |
|---|---|---|
| Düşük risk + az kişi | Belgele, gerekmeyebilir | Gerekmez |
| Düşük risk + çok kişi | Gerekli | Genelde gerekmez |
| Yüksek risk + az kişi | Gerekli | Gerekli |
| Yüksek risk + çok kişi | Gerekli + acil | Gerekli + acil |

### 4. KVKK bildirim taslağı

```markdown
# Kişisel Veri İhlali Bildirimi

**Veri Sorumlusu:** [Şirket bilgisi]
**Bildirim Tarihi:** [Bugün + saat]
**Olay Tespit Tarihi:** [Olay zamanı]
**Bildirim Süresi:** 72 saat sınırı içinde / dışında (m.12/5 dayanağı)

## 1. Olayın Özeti
[Ne oldu, ne zaman, nasıl tespit edildi]

## 2. Etkilenen Veriler
- Kategoriler: [liste]
- Özel nitelikli: Evet/Hayır
- Etkilenen kişi sayısı: ~X
- Etkilenen kişi grupları: [müşteri / çalışan / ziyaretçi / diğer]

## 3. Olası Sonuçlar
[Risk analizi — kimlik hırsızlığı, dolandırıcılık, ayrımcılık, ifade zararı, finansal zarar]

## 4. Alınmış / Alınacak Önlemler
- Anında müdahale: [liste]
- Süreklilik önlemleri: [liste]
- Tekrar önleme: [liste]

## 5. İlgili Kişi Bildirimi
[Yapılacak / Yapılmayacak — dayanak]

## 6. İletişim Noktası
KVKK irtibat kişisi: [İsim, e-posta, telefon]

## Ekler
- Olay tespit ekran görüntüsü/logu
- Etkilenen kişi listesi (özet, anonimize)
- Önlem uygulama kanıtları
```

### 5. İlgili kişi bildirim taslağı (yüksek risk durumunda)

```markdown
Konu: Önemli Bilgilendirme — Kişisel Verileriniz Hakkında

Sayın [Müşteri Adı],

[Tarih] tarihinde, [şirket]'in sistemleri üzerinde yaşanan bir veri olayı sonucunda kişisel verilerinizin etkilenmiş olabileceğini bildirmek isteriz. KVKK m.12/5 uyarınca, durumu en kısa sürede iletme yükümlülüğümüz çerçevesinde sizi bilgilendiriyoruz.

## Etkilenen Veriler
[Hangi veriler — gizliliği gözeterek]

## Aldığımız Önlemler
[Anlaşılır dilde]

## Yapabilecekleriniz
1. [Risk azaltıcı somut adım — örn. şifre değiştirme]
2. [Bankaya bildirim, vb.]
3. KVKK m.11 haklarınızı kullanma yolu: [link]

## İletişim
Sorularınız için: [iletişim bilgileri]

Saygılarımızla,
[Şirket]
```

### 6. Emsal kontrolü

```
mcp__yargi-mcp__search_kvkk_decisions(keywords="veri ihlali bildirim cezası")
mcp__yargi-mcp__search_kvkk_decisions(keywords="72 saat geç bildirim")
```

İlgili 3 kararı özetle. Özellikle:
- Geç bildirim cezaları
- "Yüksek risk" yorumlama kararları
- İlgili kişi bildirimi tartışmalı vakalar

## Çıktı

```
🚨 VERİ İHLALİ TRİAJI

⏰ Sayaç: 53 saat kaldı (KVKK m.12/5: 72 saat)

📊 Risk Skoru: YÜKSEK
  Sebepler: Özel nitelikli sağlık verisi + 247 etkilenen kişi

📝 Kararlar:
  ✓ KVKK Bildirim: ZORUNLU — taslak hazır
  ✓ İlgili Kişi Bildirim: ZORUNLU — taslak hazır

📄 KVKK Bildirim Taslağı: [yukarıdaki]

📄 İlgili Kişi Bildirim Taslağı: [yukarıdaki]

📚 Referans Kararlar:
  - KVKK 2023/XXX — Geç bildirim 250.000 TL idari para cezası
  - KVKK 2022/YYY — "Yüksek risk" yorumlaması

🎯 Sıradaki Adımlar:
  1. KVKK bildirim taslağını avukat onayına götür (en geç 24 saat)
  2. İlgili kişi listesini IT ile teyit et
  3. KEP veya online sistem üzerinden bildirimi yap
  4. Olay sonrası gözden geçirme planla

⚠️ Bu çıktı TASLAKTIR. Veri ihlali kritiktir — avukat
   ve veri sorumlusu temsilcisi onayı vakit kaybetmeden alınmalıdır.
```

## TBB Meslek Kuralları

- Olay raporundaki PII anonimize edilir.
- Etkilenen kişi listesi **dış servise gönderilmez**; sadece lokal işlenir.
- Plugin "ceza almazsınız" tarzı vaatte bulunmaz; risk değerlendirmesi sunar.
