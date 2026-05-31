---
name: ihlal-triaji
description: >
  Veri ihlali olayını triyaj eder. KVKK'ya 72 saatlik bildirim yapılması gerekip
  gerekmediğini değerlendirir (KVKK m.12/5 + Kurul'un 24.01.2019 tarih ve 2019/10
  sayılı kararı), ilgili kişilere bildirim eşiğini kontrol eder, bildirim
  metinlerini taslar. "Veri ihlali oldu", "data breach", "bildirim hazırla",
  "72 saat sayacı", "veri sızıntısı" söylemlerinde tetiklenir.
argument-hint: "[--olay <dosya>] [--ivedi: zaman kritik]"
---

# /kvkk-uyum-tr:ihlal-triaji

## Cold-start kontrolü
Profil yoksa interview'a yönlendir.

> ⏰ **Bu skill zaman kritiktir.** KVKK Kurulu'nun 2019/10 sayılı kararı uyarınca veri ihlali "en kısa sürede" ve **72 saat** içinde bildirilmelidir. Olay tespit anına saatleri saymaya başla.

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

**Hukuki dayanak:** KVKK m.12/5 + Kurul'un 24.01.2019 tarih ve 2019/10 sayılı kararı.

- Veri ihlali "kişilerin hak ve özgürlükleri açısından risk doğuruyorsa" KVKK'ya bildirilmeli.
- "Yüksek risk" varsa **ilgili kişilere de** bildirim yapılmalı.

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
**Bildirim Süresi:** 72 saat sınırı içinde / dışında (Kurul'un 2019/10 sayılı kararı dayanağı)

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

[Tarih] tarihinde, [şirket]'in sistemleri üzerinde yaşanan bir veri olayı sonucunda kişisel verilerinizin etkilenmiş olabileceğini bildirmek isteriz. KVKK Kurulu'nun 2019/10 sayılı kararı uyarınca, durumu en kısa sürede iletme yükümlülüğümüz çerçevesinde sizi bilgilendiriyoruz.

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

### 6. Karar referansı (opsiyonel)

**ZORUNLU KURAL:** Yalnızca konu başlığı + asıl çerçeve kararını ver. Spesifik para cezası kararı numaralarını uydurma.

```
📚 Asıl çerçeve:
  - KVKK Kurulu'nun 24.01.2019 tarih ve 2019/10 sayılı kararı
    (72 saat bildirim ilkesi — yapısal karar, doğrulanmıştır)

📚 İhlal bildirim cezası verilmiş diğer vakalar mevcuttur ancak:
  - Spesifik numaraları uydurmuyorum.
  - kvkk.gov.tr → Karar Özetleri'nde "veri ihlali bildirim" araması yap;
    ilgili kararı bana yapıştır, /kvkk-uyum-tr:karar-analizi ile detaylı analiz yaparım.

⚠️ 2019/10 dışındaki tüm numaralar doğrulanmalıdır.
```

Bu disiplin sert prompt-level kuraldır. Detay: `references/karar-atif-kurallari.md`

## Çıktı

```
🚨 VERİ İHLALİ TRİAJI

⏰ Sayaç: 53 saat kaldı (Kurul 2019/10: 72 saat)

📊 Risk Skoru: YÜKSEK
  Sebepler: Özel nitelikli sağlık verisi + 247 etkilenen kişi

📝 Kararlar:
  ✓ KVKK Bildirim: ZORUNLU — taslak hazır
  ✓ İlgili Kişi Bildirim: ZORUNLU — taslak hazır

📄 KVKK Bildirim Taslağı: [yukarıdaki]

📄 İlgili Kişi Bildirim Taslağı: [yukarıdaki]

📚 Çerçeve karar:
  - Kurul'un 24.01.2019 tarih ve 2019/10 sayılı kararı — 72 saat ilkesi
📚 Diğer ilgili kararlar (numarasız — kullanıcı kvkk.gov.tr'den doğrulayacak):
  - [Sadece konu başlığı yaz, numara verme]

🎯 Sıradaki Adımlar:
  1. KVKK bildirim taslağını avukat onayına götür (en geç 24 saat)
  2. İlgili kişi listesini IT ile teyit et
  3. KEP veya online sistem üzerinden bildirimi yap
  4. Olay sonrası gözden geçirme planla

⚠️ Bu çıktı TASLAKTIR. Veri ihlali kritiktir — avukat
   ve veri sorumlusu temsilcisi onayı vakit kaybetmeden alınmalıdır.
   Karar atıfları kvkk.gov.tr'den doğrulanmalıdır.
```

## TBB Meslek Kuralları

- Olay raporundaki PII anonimize edilir.
- Etkilenen kişi listesi üçüncü taraf servislere (Anthropic dışı) **aktarılmaz**; ancak Claude'un triaj yapabilmesi için içerik **Anthropic API'sine iletilir**. Veri ihlali bağlamında özellikle hassas — avukat, müvekkil sırrı (TBB m.37 / Av.K. m.36) çerçevesinde bu aktarımı kendi değerlendirir; **anonimleştirilmiş özet kullanılması** kesinlikle önerilir (gerçek ad/T.C./e-posta yerine "Müşteri 1", "Müşteri 2" gibi). Detay: `references/veri-isleme-bildirimi.md`
- Plugin "ceza almazsınız" tarzı vaatte bulunmaz; risk değerlendirmesi sunar.
