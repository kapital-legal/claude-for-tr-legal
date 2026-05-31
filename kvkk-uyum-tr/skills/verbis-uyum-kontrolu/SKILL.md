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

> **Not:** Eşik değerleri her yıl KVKK tarafından güncellenir. Plugin training data'ya göre cevap verir; sonuç `[verify]` etiketi taşır. Doğrulama için: https://www.kvkk.gov.tr

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
  Dayanak: Çalışan sayısı > 50 (74 çalışan) [verify]
  Mevcut durum: Kayıtlı (son güncelleme: 2025-09-12)

📋 Veri Envanteri (24 kayıt incelendi):
  ✓ 21 kayıt tam
  ⚠️  3 kayıt eksik:
    - Satır 7 (müşteri iletişim) — saklama süresi yok
    - Satır 14 (web ziyaretçi) — çerez detayı yok
    - Satır 19 (yurt dışı aktarım — ABD) — m.9 dayanağı belirsiz

⏰ Güncelleme Tetikleyicileri:
  ⚠️  Son güncellemeden 8 ay geçti — gözden geçirme önerilir
  ⚠️  KVKK Mart 2026 ABD güvenli ülkeler kararına göre satır 19'u revize et

📚 Konuyla ilgili kararlar [doğrulanmalı]:
  - VERBİS eksik envanter idari para cezası kararları
  - [Kullanıcı kvkk.gov.tr'den ilgili kararı doğrulamalı]

💡 Önerilen Aksiyon Listesi:
  1. Eksik 3 satırı tamamla
  2. Yurt dışı aktarım için açık rıza ya da m.9 standart sözleşme imzala
  3. VERBİS güncellemesi yap
  4. 2026 sonbahar gözden geçirmesi için takvime ekle

⚠️ Bu rapor TASLAKTIR. Avukat onayı + KVKK eşik değerlerinin
   doğrulanması gereklidir. Eşikler her yıl güncellenir.
```

## TBB Meslek Kuralları

- Müvekkil/şirketin veri envanteri bilgisi hassastır — sadece kullanıcının açıkça paylaştığı içerikle çalışır.
- Plugin envanter dosyasını **dış servise göndermez**; sadece lokal işlem yapar.
- VERBİS resmi sistem; **yanlış bildirim ihtimali** durumunda kullanıcıyı KVKK'ya başvurmaya yönlendirir.
