---
name: aydinlatma-metni-uretici
description: >
  KVKK m.10'a uygun aydınlatma metni üretir. Sektör, veri kategorisi, işleme amacı,
  aktarım taraf, saklama süresine göre özelleştirilir. "Aydınlatma metni yaz",
  "KVKK metni üret", "informed consent metni", "veri sahibi bilgilendirme",
  "privacy notice yaz" söylemlerinde tetiklenir.
argument-hint: "[--sektör <sektör>] [--kanal web|sözleşme|İK|müvekkil] [--dil tr|en|her-ikisi]"
---

# /kvkk-uyum-tr:aydinlatma-metni-uretici

## Cold-start kontrolü

`~/.claude/plugins/config/claude-for-tr-legal/kvkk-uyum-tr/CLAUDE.md` dosyasını oku:
- Yoksa → `/kvkk-uyum-tr:cold-start-interview` çalıştırmasını öner, devam etme.
- Placeholder içeriyorsa → eksik bilgiyi tamamlamayı öner.

## İş Akışı

### 1. Bağlamı topla

Kullanıcıdan veya profilden:
- **Kanal:** Web sitesi / üyelik / sözleşme / İK formu / müvekkil tutma sözleşmesi / mobil uygulama / başka
- **Veri sorumlusu:** Şirket adı, MERSİS, adres (paylaşılan profilden çek)
- **İşlenen veri kategorileri:** Profilden öneri sun, kullanıcı düzenlesin
- **İşleme amaçları:** Tek tek listele
- **Hukuki sebep (KVKK m.5/6):**
  - Açık rıza (m.5/1) — son çare
  - Kanunlarda öngörülme (m.5/2-a)
  - Sözleşmenin kurulması/ifası (m.5/2-c)
  - Hukuki yükümlülük (m.5/2-ç)
  - Meşru menfaat (m.5/2-f) — denge testi gerekir
  - Özel nitelikli için m.6
- **Aktarım taraflar:** Hangi 3. taraflara, hangi amaçla (yurtiçi/yurtdışı)
- **Saklama süresi:** Kategori bazında
- **İlgili kişi hakları:** KVKK m.11'deki 7 hak

### 2. Şablon seç

`references/aydinlatma-metni-sablonlari/` altından kanal'a uygun şablonu yükle (gelecek sürümde eklenecek; mevcutta Claude jenerik akışla üretir):
- `web-sitesi.md` — site ziyaretçisi için
- `is-iliskisi.md` — çalışan/aday için
- `muvekkil-tutma.md` — avukat-müvekkil ilişkisi
- `sozlesme-genel.md` — B2B/B2C sözleşme tarafı
- `mobil-uygulama.md` — app kullanıcısı

### 3. KVKK m.10 zorunlu unsurları kontrol et

Her aydınlatma metni şunları içermelidir (eksiklik = KVKK ihlali):

- [ ] Veri sorumlusu kimliği (ünvan, MERSİS, adres)
- [ ] (Varsa) Veri sorumlusu temsilcisi
- [ ] İşlenen kişisel veri kategorileri
- [ ] Veri toplama yöntemleri
- [ ] İşleme amaçları (her bir veri kategorisi için)
- [ ] Hukuki sebepler (her amaç için — m.5 veya m.6)
- [ ] Aktarım yapılan taraflar + aktarım amacı
- [ ] (Yurtdışı aktarım varsa) m.9 dayanağı
- [ ] Saklama süreleri
- [ ] KVKK m.11 hakları (7 hak)
- [ ] Başvuru kanalı (e-posta, KEP, posta adresi)

### 4. Risk işaretleri

Üretim sırasında şunları flag'le:

- **Açık rıza tek dayanak olarak kullanılıyorsa** → uyar; açık rıza yetersiz olabilir (geri çekilebilir, koşula bağlanamaz)
- **Özel nitelikli veri işleniyor ama m.6 dayanağı net değil** → uyar
- **Sınır ötesi aktarım var ama m.9 dayanağı belirsiz** → KVKK'nın güvenli ülkeler listesi düzenlemesine dikkat
- **Saklama süresi belirtilmemiş** → "ihtiyaç süresi" gibi belirsiz ifadeler yerine somut süre öner

### 5. Çıktı

İki sürüm üret:

**A) Web/PDF için (uzun metin):**
```markdown
# Kişisel Verilerin İşlenmesi Hakkında Aydınlatma Metni

## 1. Veri Sorumlusu
[Şirket bilgileri]

## 2. İşlenen Kişisel Veri Kategorileri ve Toplama Yöntemleri
...

## 3. İşleme Amaçları ve Hukuki Sebepleri
[Her amaç için ayrı tablo: amaç + veri + m.X dayanağı]

## 4. Aktarım Tarafları
...

## 5. Saklama Süreleri
[Tablo: veri kategorisi → süre → mevzuat dayanağı]

## 6. İlgili Kişi Hakları (KVKK m.11)
[7 hakkı tek tek say]

## 7. Başvuru
...
```

**B) Onay metni (kısa, açık rıza kutusu için):**
```
"Yukarıdaki Aydınlatma Metnini okudum, anladım ve [şu işleme] yönelik kişisel verilerimin işlenmesine açık rıza veriyorum."
```

### 6. Karar referansı (opsiyonel)

**ZORUNLU KURAL:** Üretilen metnin riskli alanları varsa (açık rıza, özel nitelikli, sınır ötesi), yalnızca **konu başlıklarını** sun — karar numarası, tarihi veya alıntısı asla uydurma.

```
📚 Şu konularda KVKK Kurul kararları olabilir (numaraları doğrulanmamış):
- Çalışan kurumsal e-posta izleme + aydınlatma yükümlülüğü
- Eski çalışan hesabına erişim
- Açık rıza ile pazarlama ileti / cookie / çerez ayrımı
- [vb. konu başlıkları]

⚠️ Spesifik karar numarası/tarihi vermiyorum — halüsinasyon riski yüksek.
   kvkk.gov.tr → Karar Özetleri sayfasında bu konuları ara,
   bulduğun kararı /kvkk-uyum-tr:karar-analizi ile bana yapıştır,
   detaylı analiz yaparım.
```

Bu disiplin sert prompt-level kuraldır. Detay: `references/karar-atif-kurallari.md`

## Çıktı Formatı

```
✓ Aydınlatma metni üretildi (2 sürüm)

📄 Tam metin: [yukarıdaki uzun versiyon]

📝 Onay kutusu: [yukarıdaki kısa versiyon]

📋 KVKK m.10 kontrol listesi: 11/11 ✓

⚠️ Dikkat edilmesi gereken noktalar:
- [her flag bir satır]

📚 Konu başlıkları (numarasız — doğrulanmamış):
- [Sadece konu adı yaz, "2023/86" gibi sayı verme]
- kvkk.gov.tr/Icerik'ten ilgili konuyu ara, kararı bul, /kvkk-uyum-tr:karar-analizi ile yapıştır

⚠️ Bu çıktı bir TASLAKTIR. Hukuki tavsiye değildir.
Avukat onayı + müvekkilin durumuna uyum kontrolü gereklidir.
Karar atıfları kvkk.gov.tr'den doğrulanmalıdır.
```

## TBB Meslek Kuralları

- Müvekkil ismi/PII kullanılmaz; örneklerde "Şirket [X]" şeklinde anonim.
- Sonuç garantisi içeren ifadeler ("KVKK denetiminden geçer", "ceza almazsınız") kullanılmaz.
- Aydınlatma metninin KVKK Kurulu'nun kabul edeceğini iddia etmez — taslak avukat onayına sunulur.
