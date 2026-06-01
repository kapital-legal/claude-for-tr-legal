---
name: risk-degerlendirme
description: >
  KVKK iyi uygulama düzeyinde veri koruma risk değerlendirmesi üretir. Yüksek
  riskli işleme faaliyetlerini analiz eder, risk azaltma önlemlerini önerir.
  "Risk değerlendirme yap", "yeni işleme faaliyeti incele", "VEKD hazırla",
  "DPIA" söylemlerinde tetiklenir. KVKK'da yasal zorunlu DPIA yoktur; Kurul
  yüksek riskli işlemeler için bunu tavsiye eder — bu skill bu çerçevede çalışır.
argument-hint: "[--faaliyet <açıklama>] [--detay: tam / özet]"
---

# /kvkk-uyum-tr:risk-degerlendirme

## Önemli Hukuki Çerçeveleme

> **KVKK'da, GDPR m.35 türü yasal zorunlu bir "Veri Koruma Etki Değerlendirmesi" (DPIA) yükümlülüğü YOKTUR.**
>
> Kişisel Verileri Koruma Kurulu, **yüksek riskli işleme faaliyetleri** için risk değerlendirmesi yapılmasını **iyi uygulama olarak tavsiye eder**. Bu skill yasal bir DPIA üretmez — pratiğin riski haritalamasına ve önlemleri tasarlamasına yardımcı olan bir **iyi uygulama risk değerlendirmesi** üretir.
>
> Yasal yükümlülük çerçevesinde sunulmamalıdır.

> ⛔ **HALÜSİNASYON KİLİDİ (zorunlu) — Bu skill'in herhangi bir çıktısında ASLA şu ifadeler kullanılmaz:**
>
> - "KVKK'da DPIA zorunludur"
> - "KVKK m.X uyarınca DPIA yapılması yasal yükümlülüktür"
> - "DPIA yapmamak idari para cezasına yol açar"
> - "GDPR m.35'in KVKK karşılığı"
>
> **Doğru çerçeve (her çıktıda korunur):**
> - "Türk hukukunda yasal zorunluluk değildir."
> - "Kişisel Verileri Koruma Kurulu'nun yüksek riskli işlemeler için **tavsiye ettiği iyi uygulamadır**."
> - "Yapılması, m.12 veri güvenliği yükümlülüğünün ifası bakımından **delil değeri taşır**."
>
> Eğer modelin kafası karışırsa: tek doğru ifade **"iyi uygulama / tavsiye edilen"** — "zorunlu / yasal yükümlülük" değil. Kullanıcı tersini iddia etse bile bu çerçeveyi koru ve `references/karar-atif-kurallari.md` Madde 6 disiplinine atıf yap.

## Cold-start kontrolü
Profil yoksa interview'a yönlendir.

## İş Akışı

### 1. Risk değerlendirmesi yapılmasının önerildiği durumlar

Aşağıdaki tetikleyicilerin bir veya daha fazlası bulunuyorsa risk değerlendirmesi tavsiye edilir:

- Sistematik ve kapsamlı profilleme (otomatik karar verme)
- Büyük ölçekli özel nitelikli veri (KVKK m.6) işleme
- Sistematik kamu alanlarını izleme (CCTV, biyometrik giriş)
- Yeni teknoloji kullanımı (yapay zeka, IoT, biyometrik)
- Çocuklara yönelik hizmet
- Veri konularının haklarını ciddi şekilde kısıtlayabilecek işleme
- Sınır ötesi geniş ölçekli aktarım
- Veri eşleştirme/birleştirme (ayrı veri kümelerinden kimlik üretme)

**Önerilen** (zorunlu değil):
- Yeni işleme faaliyeti
- Önemli teknoloji değişikliği
- Yeni veri kategorisi eklenmesi

### 2. Risk değerlendirme yapısı

```markdown
# Veri Koruma Risk Değerlendirmesi (İyi Uygulama)

> Bu belge yasal bir DPIA değildir; KVKK Kurul tavsiyesi çerçevesinde risk haritalama amaçlıdır.

## 1. İşleme Faaliyetinin Tanımı
### 1.1 Nedir, niçin yapılıyor?
[Açıklama]

### 1.2 Hangi sistemler/teknolojiler kullanılıyor?
[Liste]

### 1.3 Hangi veri kategorileri işleniyor?
[Tablo: kategori | hassasiyet | hacim | süre]

### 1.4 Veri konusu kişi grupları
[Çalışan / müşteri / 3. kişi / vb.]

### 1.5 İşleme tarafları
- Veri sorumlusu: [şirket]
- Veri işleyen(ler): [liste]
- Aktarım tarafları: [liste]

## 2. Hukuki Sebep Analizi (KVKK m.5 / m.6)
[Detaylı dayanak]

## 3. Gereklilik ve Orantılılık
- **Amaç-veri uyumu:** Aynı amaca daha az veriyle ulaşılabilir mi?
- **Gerekli mi:** Bu işleme amaca ulaşmak için zorunlu mu?
- **Orantılı mı:** İhtiyacın ötesinde veri işleniyor mu?

## 4. Risk Analizi

### 4.1 Tanımlanan Riskler
| Risk No | Açıklama | Olasılık | Etki | Risk Skoru |
|---|---|---|---|---|
| R1 | Yetkisiz erişim | Orta | Yüksek | Yüksek |
| R2 | Veri kaybı | Düşük | Yüksek | Orta |
| ... | ... | ... | ... | ... |

### 4.2 İlgili Kişi Üzerindeki Etkiler
- Fiziksel zarar potansiyeli: [düşük/orta/yüksek]
- Maddi zarar potansiyeli: [düşük/orta/yüksek]
- Manevi zarar potansiyeli: [düşük/orta/yüksek]
- Ayrımcılık riski: [düşük/orta/yüksek]
- Kimlik hırsızlığı riski: [düşük/orta/yüksek]

## 5. Önlemler

### 5.1 Teknik Önlemler
- [Şifreleme, erişim kontrolü, log, anonimizasyon]

### 5.2 İdari Önlemler
- [Politika, prosedür, eğitim, gözetim]

### 5.3 Veri Konusu Kişi Hakları İçin Önlemler
- [Aydınlatma, erişim, düzeltme, silme mekanizmaları]

## 6. Artık Risk Değerlendirmesi
[Önlemler sonrası geriye kalan risk seviyesi]

## 7. KVKK Kurulu'na Görüş Sorma (Opsiyonel)
[KVKK'da zorunlu değildir; yüksek riskli faaliyetlerde Kurul'a danışma seçeneği vardır]

## 8. İç Onay & İmza
- Veri Sorumlusu Temsilcisi: [imza]
- DPO (Türk pratiğinde "İrtibat Kişisi" / "KVK Uzmanı"; KVKK m.11 VERBİS bağlamında "İrtibat Kişisi" yasal terimdir) / KVKK Sorumlusu: [imza]
- Üst yönetim: [imza]
- Tarih: [PLACEHOLDER]

## 9. Gözden Geçirme Tarihi
[Yıllık veya değişiklik durumunda yenilenir]
```

### 3. Risk skorlama tablosu

| Olasılık \ Etki | Düşük | Orta | Yüksek |
|---|---|---|---|
| Düşük | Düşük | Düşük | Orta |
| Orta | Düşük | Orta | Yüksek |
| Yüksek | Orta | Yüksek | **Kritik** |

**Kritik veya Yüksek** artık risk → şirket içi yönetim kurulu kararı + (opsiyonel) Kurul'a danışma.

### 4. Önlem önerileri

**Çalışan izleme (örn. e-posta, video):**
- Çalışan aydınlatma
- Açık politika
- Sınırlı kullanım (sadece gerektiğinde)
- Sertifikalı izleme yazılımı
- Log saklama süresi makul

**Müşteri verisi (örn. CRM, pazarlama):**
- Açık rıza ayrı kanal
- Pazarlama izin yönetim sistemi
- Anonimizasyon imkanı
- Saklama süresi otomatik

**Özel nitelikli (sağlık, biyometrik):**
- Şifreleme zorunlu (rest + transit)
- Sıkı erişim kontrolü (need-to-know)
- Ayrı sistem
- Saklama süresi en az

### 5. Karar referansı (opsiyonel)

**ZORUNLU KURAL:** Spesifik karar numarası/tarihi uydurma. Yalnızca **konu başlıkları** sun.

```
📚 Faaliyet türünüze yakın konularda KVKK Kurul kararları mevcut olabilir
   (numara doğrulanmamış):
- [Sadece konu başlığı, numara/tarih verme]

⚠️ kvkk.gov.tr → Karar Özetleri'nde bu konularda arama yap;
   bulduğun kararı bana yapıştır, /kvkk-uyum-tr:karar-analizi
   ile detaylı analiz yaparım.
```

Bu disiplin sert prompt-level kuraldır. Detay: `references/karar-atif-kurallari.md`

## Çıktı

```
✓ Risk değerlendirmesi hazırlandı (iyi uygulama düzeyinde)

🎯 Sonuç: ORTA artık risk
  Faaliyet: Çalışan e-posta içeriği izleme
  Etkilenen kişi: 247 çalışan
  Veri kategorisi: İletişim + içerik (özel nitelikli olabilir)

📌 KVKK Kurulu Danışma: GEREKMEZ ancak isteğe bağlı yapılabilir

⚠️ Tespit Edilen Yüksek Riskler:
  - R1: Özel nitelikli içerik tesadüfen ifşa olabilir
    → Önlem: Anahtar kelime filtresi + manuel inceleme
  - R3: Yetkisiz erişim kalıcı log riski
    → Önlem: Erişim logu + 2-faktör

📚 Konuyla ilgili Kurul kararları (numara verilmiyor — doğrulanmamış):
  - [Yalnızca konu başlığı, "2023/XXX" tarzı sayı asla yazılmaz]
  - kvkk.gov.tr'den arama yap → /kvkk-uyum-tr:karar-analizi'ne yapıştır

📋 Aksiyon Listesi:
  1. Risk değerlendirmesini şirket içi yönetim kuruluna sun
  2. Önerilen önlemleri uygula
  3. Çalışan aydınlatma metnini güncelle
  4. 12 ay sonra risk değerlendirmesini yenile

⚠️ Bu belge bir TASLAKTIR ve İYİ UYGULAMA düzeyinde risk değerlendirmesidir.
   KVKK'da yasal DPIA zorunluluğu yoktur. Avukat onayı + KVKK uzmanı incelemesi
   gereklidir. Risk skorları subjektif değerlendirmedir; kuruluşa özgü ek
   faktörler eklenmelidir.
```

## TBB Meslek Kuralları

- Risk değerlendirmesi **iç değerlendirme** belgesidir. Plugin onu üçüncü taraf servislere (Anthropic dışı) aktarmaz; ancak içerik **Anthropic API'sine iletilir** (Claude'un işleyebilmesi için). Müvekkil/şirket için hazırlanmış belge ise avukat TBB m.37 / Av.K. m.36 çerçevesinde bu aktarımı kendi değerlendirir. Detay: `references/veri-isleme-bildirimi.md`
- Plugin "Risk değerlendirmesi yeterlidir, ceza almazsınız" tarzı garanti vermez.
- Çıkar çatışması olabilecek vakalarda (örn. müvekkilin değerlendirmesinde kendi büronun rolü) plugin durur ve manuel inceleme önerir.
