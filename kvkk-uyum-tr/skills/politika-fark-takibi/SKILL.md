---
name: politika-fark-takibi
description: >
  Şirketin mevcut KVKK politikası, aydınlatma metni veya iç prosedürü ile
  kullanıcının sağladığı KVKK Kurulu kararları/güncel mevzuat arasındaki
  sapmaları (drift) tespit eder. Politika hangi konularda güncellenmesi
  gerektiğini, hangi yeni kararlara göre revize edileceğini gösterir.
  "Politika güncelle", "drift kontrol", "yeni kararlar politikamızı etkiler mi?",
  "KVKK uyum gap analizi" söylemlerinde tetiklenir.
argument-hint: "[--politika <dosya-yolu>] [--kararlar <dosya-yolu-veya-URLs>]"
---

# /kvkk-uyum-tr:politika-fark-takibi

> Bu skill **otomatik karar tarama** yapmaz. Kullanıcı incelenecek kararların URL/metnini sağlamalı, plugin politikayla karşılaştırarak fark üretir.

## Cold-start kontrolü
Profil yoksa interview'a yönlendir.

## İş Akışı

### 1. Politika dosyasını oku

```
Read(politika-dosyası)
```

Tipik politika dokümanları:
- Şirket içi KVKK Politikası
- Aydınlatma Metni (web/sözleşme)
- Veri Saklama ve İmha Politikası
- Erişim Yönetimi Prosedürü
- İhlal Müdahale Planı
- Çalışan Veri İşleme Politikası

Politikadaki ana konuları çıkar:
- Hangi veri kategorilerini işliyor?
- Hangi hukuki sebepleri dayanak gösteriyor?
- Saklama süreleri belirtilmiş mi?
- Çalışan/müşteri/ziyaretçi izlemeleri nasıl tarif ediliyor?
- İhlal müdahale süreci var mı?
- Yurt dışı aktarım maddesi var mı?

### 2. Karşılaştırılacak kararları al

Kullanıcı şu üç yoldan biriyle sağlar:
- **URL listesi:** `https://www.kvkk.gov.tr/Icerik/<XXXX>/<YYYY-NN>, https://...` (örnek format — kullanıcının kvkk.gov.tr'den getirdiği gerçek URL'lerle değiştir)
  Plugin `WebFetch` ile her birini çeker.
- **Yapıştırılmış metinler:** Kullanıcı 3-5 karar metnini bir bütün olarak yapıştırır.
- **Dosya yolu:** Kullanıcı kararları topladığı bir Markdown/PDF dosyası verir.

Eğer kullanıcı kararları sağlamamış ise, plugin önerir:

> Politika fark analizi için kararları senin sağlaman gerek. İki seçenek var:
> 1. kvkk.gov.tr → Karar Özetleri sayfasında son 12 ayın kararlarına bak, ilgili olanların URL'ini buraya yapıştır.
> 2. Eğer "şu konuda son ne çıkmış?" sorun varsa, önce `/kvkk-uyum-tr:karar-analizi` ile spesifik bir karar çek; bu skill ile daha sonra fark karşılaştırması yap.
>
> Plugin kendisi karar veritabanına bağlanmaz — bu, müvekkil mahremiyeti ve TBB Meslek Kuralları çerçevesinde bilinçli bir tasarım kararıdır.

### 3. Fark analizi (gap analysis)

Her politika konusu için:

| Politika Maddesi | Mevcut Durum | Karar Yorumu | Sapma? | Aksiyon |
|---|---|---|---|---|
| Çalışan e-posta izleme | "Şirket politikamızca izlenebilir" | Kullanıcının verdiği karar: aydınlatma + makul şüphe + sınırlı amaç gerekli | ⚠️ Sapma | Politikaya aydınlatma + amaç sınırı ekle |
| Saklama süresi | "Ticari ihtiyaç süresince" | [İlgili karar yorumu] | ⚠️ Belirsiz | Süreleri tablo halinde yaz |
| Yurt dışı (ABD) aktarım | "Açık rıza ile" | [Güncel karar yorumu] | ⚠️ Kritik | KVKK m.9 standart sözleşme imzala |
| ... | ... | ... | ... | ... |

Sapma seviyeleri:
- 🟢 **Uyumlu** — politika kararla aynı yönde
- 🟡 **Belirsiz** — politika konuyu netleştirmemiş
- ⚠️ **Sapma** — politika kararla çelişiyor veya eksik
- 🚨 **Kritik** — politikada KVKK ihlali riski var

### 4. Önerilen revizyonlar

Sapma tespit edilen her madde için:

**Mevcut metin:**
> [politikadan birebir alıntı]

**Önerilen metin:**
> [revize edilmiş hali]

**Dayanak:**
> KVKK Karar [tarih-no] [doğrulanmalı] — [konunun özeti]
> URL: [kullanıcının verdiği URL]

### 5. Önceliklendirme

Risk × kullanım sıklığı matrisi:

| Konu | Risk | Kullanım | Öncelik |
|---|---|---|---|
| Yurt dışı aktarım | 🚨 Kritik | Günlük | **P1 — Bu hafta** |
| Çalışan izleme | ⚠️ Sapma | Sürekli | **P1 — Bu hafta** |
| Veri saklama | 🟡 Belirsiz | Sürekli | **P2 — Bu ay** |
| Çerez | 🟢 Uyumlu | Günlük | **P3 — Gözden geçirme** |

## Çıktı

```
🔍 KVKK Politika Fark Analizi

📄 İncelenen Politikalar: 3 doküman, 47 madde
📚 Karşılaştırılan Kararlar: 12 karar (kullanıcı tarafından sağlandı)

📊 Genel Skor: 23 madde uyumlu, 18 belirsiz, 5 sapma, 1 kritik

🚨 KRİTİK (1):
  1. Yurt dışı (ABD) aktarım açık rızaya dayanıyor
     → Kullanıcının verdiği [Karar No] [doğrulanmalı] ile yetersiz
     → ACİL: m.9 standart sözleşmesi imzalanmalı

⚠️ SAPMA (5):
  1. Çalışan e-posta izleme — aydınlatma şartı eksik
     (Kullanıcının sağladığı karara göre — örn. "Karar [No]" yapıştırılan metinden alınır)
  ...

🟡 BELİRSİZ (18): Detay raporda

📋 Aksiyon Planı (Öncelik Sıralı):
  P1 (Bu Hafta):
    1. Yurt dışı aktarım maddesini revize et
    2. Çalışan e-posta izleme politikasını güncelle
  P2 (Bu Ay):
    3. Saklama sürelerini tabloya dök
    ...
  P3 (Gözden Geçirme):
    ...

📚 Atıfta Bulunulan Kararlar (kullanıcı tarafından sağlandı):
  [Tam liste, URL'leriyle, hepsi doğrulama bekleniyor]

⚠️ Bu rapor TASLAKTIR. Politika değişiklikleri avukat onayı ve
   yönetim kurulu kararı gerektirir. Bu analiz yalnızca kullanıcının
   sağladığı kararlara dayalıdır; sektörel düzenlemeler (BDDK,
   SPK, RTÜK vb.) ayrıca incelenmelidir.
```

## TBB Meslek Kuralları

- Politika dosyaları şirket içi gizli niteliktedir. Plugin onları üçüncü taraf servislere (Anthropic dışı) **aktarmaz**; ancak içerik **Anthropic API'sine iletilir** (Claude'un karşılaştırma yapabilmesi için). Avukat, TBB m.37 / Av.K. m.36 çerçevesinde bu aktarımı kendi değerlendirir; şirket politikasında "AI işleme" izni yoksa kullanılmadan önce iç onay alınmalıdır. Detay: `references/veri-isleme-bildirimi.md`
- Plugin politikanın "uyumlu hale getirildikten sonra KVKK kontrolünden geçeceğini" iddia etmez.
- Müvekkil/şirket avukatı için ürettiği analizdir; üçüncü taraf görüşü değildir.
