---
name: ilgili-kisi-basvurusu
description: >
  İlgili kişinin KVKK m.11 başvurusunu analiz eder, KVKK m.13/2 uyarınca 30 günlük
  yasal süre içinde gerekçeli yanıt taslar. Talep kategorisini (erişim, silme,
  düzeltme, itiraz, vb.) sınıflandırır, m.28 reddi gerektiğinde dayanağıyla
  işaretler. "İlgili kişi başvurusu", "veri sahibi talebi", "kişisel veri silme
  talebi", "veriye erişim talebi" söylemlerinde tetiklenir.
argument-hint: "[--basvuru <dosya-yolu>] [--ivedi: 30 günlük süre yaklaşıyor]"
---

# /kvkk-uyum-tr:ilgili-kisi-basvurusu

## Cold-start kontrolü
Profil yoksa kullanıcıyı `/kvkk-uyum-tr:cold-start-interview`'a yönlendir.

## İş Akışı

### 1. Başvuru dilekçesini oku

- Kullanıcı dosya yolu verdiyse `Read` ile aç.
- Vermediyse: "Başvurunun metnini paylaş ya da dosya yolu ver" diye sor.
- KVKK m.13'ün şekil şartlarını kontrol et: yazılı, e-posta veya başvuru kanalı üzerinden yapılmış mı? Kimlik teyidi mevcut mu?

### 2. Süre takibi

Başvuru tarihini al (dilekçeden veya kullanıcıdan). **KVKK m.13/2: 30 günlük cevap süresi**:
- ≤ 5 gün kaldıysa → 🚨 KIRMIZI: hızlandırılmış cevap modu
- ≤ 15 gün kaldıysa → 🟡 SARI: dikkat
- > 15 gün → 🟢 NORMAL

Süreyi başlığa yaz, kalan gün sayısını söyle.

### 3. Talep kategorisi sınıflandırması

KVKK m.11 hakları üzerinden işaretle (birden fazla olabilir):

| Hak | Açıklama | Reddedilebilir mi? |
|---|---|---|
| (a) İşlenip işlenmediği | "Verim işleniyor mu?" | Hayır — bilgi verme zorunlu |
| (b) İşlenmişse bilgi | "Hangi veriler, niye?" | Sınırlı (m.28 istisnaları) |
| (c) İşleme amacı uyumu | "Amacına uygun mu işleniyor?" | Hayır |
| (ç) Aktarım tarafları | "Kime aktarıldı?" | Sınırlı |
| (d) Eksik/yanlış düzeltme | Düzeltme talebi | Hayır — yerine getirilmeli |
| (e) Silme/yok etme | "Verimi sil" | Bazı durumlarda (m.7) |
| (f) Aktarım iptali | Üçüncü tarafa bildirim | Bazı durumlarda |
| (g) İtiraz (otomatik karar) | "Otomatik karar tarafıma aleyhe" | Sınırlı |
| Tazminat | (m.11 dışı) Tazminat talebi | Yargılama gerektirir |

### 4. Reddi gerektirebilecek durumlar (KVKK m.28)

Şu durumlarda kanun talebi reddetmeyi gerektirebilir:
- Diğer kişilerin haklarını ihlal eden talep
- Şirket ticari sırrı ihlali
- Devam eden hukuki süreç
- Hukuki yükümlülük (saklama mecburiyeti)
- Kamu güvenliği
- Suç işlenmesinin önlenmesi/soruşturma

Tespit edilirse net dayanakla flag'le.

### 5. Yanıt taslağı üret

Standart yapı:

```markdown
# İlgili Kişi Başvurusuna Cevap

**Sayın [ad/anonimize edilmiş],**

[Başvuru tarihi] tarihli, KVKK m.11 kapsamında ileri sürdüğünüz başvurunuz tarafımıza ulaşmıştır. KVKK m.13/2 uyarınca 30 günlük yasal süre içinde aşağıdaki cevabı sunarız.

## 1. Talep Özeti
[Madde madde her talep]

## 2. Cevaplar
### Talep 1: [konu]
**Cevap:** [olumlu/olumsuz/kısmi]
**Dayanak:** [KVKK m.X — uygulanan hüküm]
[Detay]

### Talep 2: ...

## 3. (Varsa) Reddedilen Talepler
**Dayanak:** KVKK m.28/[fıkra]
**Açıklama:** [Net gerekçe]

## 4. İtiraz Yolu
Bu cevabımızdan tatmin olmazsanız, KVKK m.14 uyarınca cevabımızın tebliğinden itibaren 30 gün, başvuru tarihinden itibaren 60 günü geçmemek üzere Kişisel Verileri Koruma Kurulu'na şikayet hakkınız vardır (https://kvkk.gov.tr).

Saygılarımızla,
[Veri Sorumlusu] / [Yetkili Kişi]
```

### 6. Karar referansı (opsiyonel)

Talebin kategori veya konu özelinde **eğitim verimden hatırladığım** Kurul karar başlıklarını listele:

```
📚 Benzer talep kategorilerinde ilgili olabileceği değerlendirilen kararlar [doğrulanmalı]:
- [Karar başlığı] — [kısa not]
- ...

⚠️ Bu liste yön gösterici niteliktedir. Karar numaralarını ve içerikleri 
   https://www.kvkk.gov.tr 'den teyit edin.
```

Kullanıcı bir karar URL veya metni paylaşırsa `/kvkk-uyum-tr:karar-analizi` skill'ini öner.

### 7. Süre takip uyarısı

Cevap taslağı sonunda:
```
⏰ Süre takip:
- Başvuru tarihi: [tarih]
- Yasal süre sonu (KVKK m.13/2): [+30 gün tarih]
- Bugün: [tarih] — [X] gün kaldı

Önerilen aksiyon:
- [X] gün içinde imza alın
- KEP/e-posta ile cevabı gönderin
- Cevap gönderim tarihini kayıt altına alın (denetim için)
```

## Çıktı

```
✓ İlgili kişi başvurusu analiz edildi

📋 Sınıflandırma:
  - 3 talep: erişim (a, b), düzeltme (d), silme (e)
  - 1 talep reddi gerektirebilir (m.28 — devam eden ceza soruşturması)

📄 Cevap taslağı: [yukarıdaki yanıt]

⏰ 18 gün kaldı (yasal süre: 30 gün)

📚 Konuyla ilgili olabileceği değerlendirilen kararlar [doğrulanmalı]:
  - [Liste — kvkk.gov.tr'den doğrula]

⚠️ Bu cevap TASLAKTIR. Avukat onayı gerektirir.
   Kullanılan KVKK madde atıfları doğrulanmalıdır.
   Karar atıfları kvkk.gov.tr'den teyit edilmelidir.
```

## TBB Meslek Kuralları

- İlgili kişinin **ismi/PII** çıktıda anonim tutulur (sadece taslakta yer alır).
- Reddedilen talep için **kesinleşmiş bir hukuki tavsiye** olarak değil, taslak olarak sunulur.
- Müvekkil/şirketin ticari sırrı veya çıkar çatışması varsa avukat bilgilendirilmeden cevap üretilmez.
