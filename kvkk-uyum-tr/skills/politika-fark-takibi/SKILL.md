---
name: politika-fark-takibi
description: >
  Şirketin mevcut KVKK politikası, aydınlatma metni veya iç prosedürü ile son
  KVKK Kurulu kararları arasındaki sapmaları (drift) tespit eder. Politika
  hangi konularda güncellenmesi gerektiğini, hangi yeni kararlara göre revize
  edileceğini gösterir. "Politika güncelle", "drift kontrol", "yeni kararlar
  politikamızı etkiler mi?", "KVKK uyum gap analizi" söylemlerinde tetiklenir.
argument-hint: "[--politika <dosya-yolu>] [--son-N-ay 6|12|24]"
---

# /kvkk-uyum-tr:politika-fark-takibi

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

### 2. Son N ay KVKK kararlarını çek

`--son-N-ay` parametresine göre (varsayılan 12 ay):

```
mcp__yargi-mcp__search_kvkk_decisions(keywords="çalışan veri izleme")
mcp__yargi-mcp__search_kvkk_decisions(keywords="müşteri verisi açık rıza")
mcp__yargi-mcp__search_kvkk_decisions(keywords="yurt dışı aktarım")
mcp__yargi-mcp__search_kvkk_decisions(keywords="veri saklama süresi")
mcp__yargi-mcp__search_kvkk_decisions(keywords="ihlal bildirim")
mcp__yargi-mcp__search_kvkk_decisions(keywords="aydınlatma yetersiz")
```

Politika konularına göre hedefli sorgu listesi oluştur.

### 3. Fark analizi (gap analysis)

Her politika konusu için:

| Politika Maddesi | Mevcut Durum | KVKK 2025-2026 Yorumu | Sapma? | Aksiyon |
|---|---|---|---|---|
| Çalışan e-posta izleme | "Şirket politikamızca izlenebilir" | Aydınlatma + makul şüphe + sınırlı amaç (Karar 2023/86) | ⚠️ Sapma | Politikaya aydınlatma + amaç sınırı ekle |
| Saklama süresi | "Ticari ihtiyaç süresince" | Kesin süre + sebep (Karar 2024/XXX) | ⚠️ Belirsiz | Süreleri tablo halinde yaz |
| Yurt dışı (ABD) aktarım | "Açık rıza ile" | Mart 2026 ABD kararına göre yetersiz | ⚠️ Kritik | M.9 standart sözleşme imzala |
| ... | ... | ... | ... | ... |

Sapma seviyeleri:
- 🟢 **Uyumlu** — politika güncel kararla aynı yönde
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
> KVKK Karar [tarih-no] — [konunun özeti]
> URL: [karar URL'i]

### 5. Önceliklendirme

Risk × kullanım sıklığı matrisi:

| Konu | Risk | Kullanım | Öncelik |
|---|---|---|---|
| Yurt dışı aktarım | 🚨 Kritik | Günlük | **P1 — Bu hafta** |
| Çalışan izleme | ⚠️ Sapma | Sürekli | **P1 — Bu hafta** |
| Veri saklama | 🟡 Belirsiz | Sürekli | **P2 — Bu ay** |
| Çerez | 🟢 Uyumlu | Günlük | **P3 — Gözden geçirme** |

### 6. (Opsiyonel) Otomatik takip

Plugin, kullanıcı isterse her ay otomatik çalışacak şekilde:
- Son ayın KVKK kararlarını tarar
- Önceki politika ile karşılaştırır
- Yeni sapmalar varsa e-posta veya panel bildirimi gönderir

(Bu özellik P2'de eklenecek `managed-agent-cookbooks/kvkk-drift-watcher/` ile.)

## Çıktı

```
🔍 KVKK Politika Fark Analizi (12 aylık dönem)

📄 İncelenen Politikalar: 3 doküman, 47 madde
📚 Taranan KVKK Kararları: 156 karar (Mayıs 2025 - Mayıs 2026)

📊 Genel Skor: 23 madde uyumlu, 18 belirsiz, 5 sapma, 1 kritik

🚨 KRİTİK (1):
  1. Yurt dışı (ABD) aktarım açık rızaya dayanıyor
     → 2026/XXX kararı ile yetersiz
     → ACİL: m.9 standart sözleşmesi imzalanmalı

⚠️ SAPMA (5):
  1. Çalışan e-posta izleme — aydınlatma şartı eksik
     (Karar 2023/86)
  2. Eski çalışan hesap erişimi — m.5/2-f gerekçesiz kullanılıyor
     (Karar 2021/1187)
  3. ... 3 madde daha

🟡 BELİRSİZ (18): Detay raporda

📋 Aksiyon Planı (Öncelik Sıralı):
  P1 (Bu Hafta):
    1. Yurt dışı aktarım maddesini revize et
    2. Çalışan e-posta izleme politikasını güncelle
  P2 (Bu Ay):
    3. Saklama sürelerini tabloya dök
    4. ... 7 madde
  P3 (Gözden Geçirme):
    8. Çerez politikası (uyumlu, sadece dil iyileştir)

📚 Atıfta Bulunulan Kararlar:
  [Tam liste, URL'leriyle]

⚠️ Bu rapor TASLAKTIR. Politika değişiklikleri avukat onayı ve
   yönetim kurulu kararı gerektirir. Bu analiz, yalnızca KVKK
   Kurulu kararlarına dayalıdır; sektörel düzenlemeler (BDDK,
   SPK, RTÜK vb.) ayrıca incelenmelidir.
```

## TBB Meslek Kuralları

- Politika dosyaları **şirket içi gizli**dir; dış servise gönderilmez.
- Plugin politikanın "uyumlu hale getirildikten sonra KVKK kontrolünden geçeceğini" iddia etmez.
- Müvekkil/şirket avukatı için ürettiği analizdir; üçüncü taraf görüşü değildir.
