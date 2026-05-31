---
name: verbis-uyum-kontrolu
description: >
  VERBİS (Veri Sorumluları Sicil Bilgi Sistemi) kayıt durumunu kontrol eder, 
  şirketin kayıt zorunluluğunu hesaplar, 6 aylık veri envanteri güncellemesi
  yapar, eksik veya yanlış kayıtları flag'ler. "VERBİS kontrol", "veri envanteri
  güncelle", "VERBİS kayıt gerek mi?", "veri sorumlusu sicili" söylemlerinde
  tetiklenir.
argument-hint: "[--envanter <dosya>] [--karşılaştır: önceki envanter ile diff]"
---

# /kvkk-uyum-tr:verbis-uyum-kontrolu

## Cold-start kontrolü
Profil yoksa kullanıcıyı interview'a yönlendir.

## İş Akışı

### 1. Kayıt zorunluluğu hesabı

Kullanıcının profilinden veya soruyla:
- **Çalışan sayısı:** [PLACEHOLDER]
- **Yıllık ciro/bilanço:** [PLACEHOLDER]
- **Özel nitelikli veri işliyor mu:** [Evet/Hayır]
- **Sektör:** [Bankacılık / sağlık / dernek / kamu kurumu / diğer]

**Eşik değerleri (2026 itibariyle güncel hatırla):**
- Yıllık çalışan sayısı > 50 **veya** yıllık mali bilanço toplamı > [güncel eşik TL] → kayıt zorunlu
- Özel nitelikli veri işleyen → eşik aranmaksızın **zorunlu**
- Yurt dışında yerleşik veri sorumluları → ofis/şube varsa zorunlu
- Kamu kurum/kuruluşları, dernekler, vakıflar → istisna kategorileri kontrol et

Kullanıcı önceden kayıtlıysa skor: **Kayıtlı / Yenileme gerekli / Kayıtlı olmalıydı.**

> **Not:** Eşik değerleri her yıl KVKK tarafından güncellenir. Plugin training data'ya göre cevap verir; sonuç `[doğrulanmalı]` etiketi taşır. Doğrulama için: https://www.kvkk.gov.tr

### 2. Veri envanteri kontrol

Kullanıcı varolan VERBİS envanterini paylaşırsa (Excel, CSV, PDF):

```
Read(envanter-dosyası)
```

Her satır için kontrol:
- [ ] Veri kategorisi belirtilmiş mi?
- [ ] Veri konusu kişi grubu (örn. çalışan, müşteri, ziyaretçi) belirli mi?
- [ ] İşleme amacı net mi?
- [ ] Hukuki sebep (m.5/m.6) tanımlı mı?
- [ ] Saklama süresi belirtilmiş mi?
- [ ] Aktarım tarafları (varsa) listelenmiş mi?
- [ ] Yurt dışı aktarım varsa m.9 dayanağı var mı?
- [ ] Veri güvenliği önlemleri belirtilmiş mi?

Eksik alanları satır bazında raporla.

### 3. Sektörel zorunluluk uyarıları

- **Sağlık** sektörü → özel nitelikli zorunlu envanter
- **Bankacılık/finans** → BDDK/SPK ile çakışan zorunluluklar
- **Telekomünikasyon** → BTK + KVKK kesişimi
- **E-ticaret** → çerez politikası ek zorunluluk
- **Avukatlık büroları** → TBB Meslek Kuralları + KVKK kesişimi (müvekkil sırrı)

### 4. Güncelleme tetikleyicileri

VERBİS'te değişiklik gerektiren durumlar (KVKK'nın 6 ay önerisi):
- Yeni veri kategorisi işlemeye başladın
- İşleme amacı değişti
- Yeni aktarım tarafı eklendi (özellikle yurt dışı)
- Şirket adı/MERSİS değişti
- Veri sorumlusu temsilcisi değişti

Profil son güncelleme tarihinden 6 aydan fazla geçtiyse → 🟡 uyar.

### 5. Karşılaştırma modu (`--karşılaştır`)

İki envanter karşılaştırması:
- Yeni eklenen satırlar (yeşil)
- Çıkarılan satırlar (kırmızı)
- Değişen alanlar (sarı, eski → yeni)

Çıktı: Markdown diff tablosu + bir VERBİS güncelleme aksiyon listesi.

## Çıktı

```
🏛️ VERBİS Uyum Raporu

📌 Kayıt Zorunluluğu Durumu:
  ✓ Şirketinin kayıt zorunluluğu VAR
  Dayanak: Çalışan sayısı > 50 (74 çalışan) [doğrulanmalı]
  Mevcut durum: Kayıtlı (son güncelleme: 2025-09-12)

📋 Veri Envanteri (24 kayıt incelendi):
  ✓ 21 kayıt tam
  ⚠️  3 kayıt eksik:
    - Satır 7 (müşteri iletişim) — saklama süresi yok
    - Satır 14 (web ziyaretçi) — çerez detayı yok
    - Satır 19 (yurt dışı aktarım — ABD) — KVKK m.9 dayanağı belirsiz: yeterlilik kararı yok, uygun güvence (standart sözleşme/BCR/taahhütname) ya da arızi haller dayanağı mı?

⏰ Güncelleme Tetikleyicileri:
  ⚠️  Son güncellemeden 8 ay geçti — gözden geçirme önerilir
  ⚠️  Yurt dışı aktarım satırını KVKK m.9 yeni rejimine göre (12 Mart 2024 kanun değişikliği + Kurul'un 04.06.2024 tarih ve 2024/959 sayılı standart sözleşme kararı) revize et — uygun güvence veya arızi hal dayanağı kullan, açık rıza yalnızca arızi aktarımda

📚 Konuyla ilgili kararlar [doğrulanmalı]:
  - VERBİS eksik envanter idari para cezası kararları
  - [Kullanıcı kvkk.gov.tr'den ilgili kararı doğrulamalı]

💡 Önerilen Aksiyon Listesi:
  1. Eksik 3 satırı tamamla
  2. Yurt dışı aktarım için KVKK m.9 yeni rejimine uygun dayanak kur: yeterlilik kararı kapsamı yoksa standart sözleşme (Kurul 2024/959) / BCR / taahhütname; açık rıza yalnızca arızi durumlarda
  3. VERBİS güncellemesi yap
  4. 2026 sonbahar gözden geçirmesi için takvime ekle

⚠️ Bu rapor TASLAKTIR. Avukat onayı + KVKK eşik değerlerinin
   doğrulanması gereklidir. Eşikler her yıl güncellenir.
```

## TBB Meslek Kuralları

- Müvekkil/şirketin veri envanteri bilgisi hassastır — sadece kullanıcının açıkça paylaştığı içerikle çalışır.
- Plugin, envanter dosyasını üçüncü taraf servislere (Anthropic dışı) **aktarmaz**; ancak Claude'un içeriği analiz edebilmesi için dosya içeriği **Anthropic API'sine iletilir**. Müvekkil PII içeren envanteri kullanmadan önce avukat TBB m.37 / Av.K. m.36 çerçevesinde bu aktarımı kendi değerlendirir. Anonim veriyle çalışılması önerilir. Detay: `references/veri-isleme-bildirimi.md`
- VERBİS resmi sistem; **yanlış bildirim ihtimali** durumunda kullanıcıyı KVKK'ya başvurmaya yönlendirir.
