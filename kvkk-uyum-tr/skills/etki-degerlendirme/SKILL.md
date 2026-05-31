---
name: etki-degerlendirme
description: >
  KVKK için Veri Koruma Etki Değerlendirmesi (DPIA/VKED) hazırlar. Yüksek riskli
  işleme faaliyetlerini analiz eder, riski azaltma önlemlerini önerir, KVKK
  Kurulu'na danışma gerekip gerekmediğini değerlendirir. "DPIA yaz", "VKED
  hazırla", "etki değerlendirme", "yeni işleme faaliyeti incele", "risk analizi
  yap" söylemlerinde tetiklenir.
argument-hint: "[--faaliyet <açıklama>] [--detay: tam VKED / özet]"
---

# /kvkk-uyum-tr:etki-degerlendirme

## Cold-start kontrolü
Profil yoksa interview'a yönlendir.

## İş Akışı

### 1. Faaliyetin VKED gerektirdiğini doğrula

KVKK Kurulu'nun "Veri Koruma Etki Değerlendirmesi Rehberi" kriterleri:

VKED **zorunlu** durumlar:
- [ ] Sistematik ve kapsamlı profilleme (otomatik karar verme — m.11/g)
- [ ] Büyük ölçekli özel nitelikli veri (m.6) işleme
- [ ] Sistematik kamu alanlarını izleme (CCTV, biyometrik giriş)
- [ ] Yeni teknoloji kullanımı (yapay zeka, IoT, biyometrik)
- [ ] Çocuklara yönelik hizmet
- [ ] Veri konularının haklarını ciddi şekilde kısıtlayan işleme
- [ ] Sınır ötesi geniş ölçekli aktarım
- [ ] Veri eşleştirme/birleştirme (kimlikteki ayrı veri kümeleri)

VKED **önerilen** durumlar:
- Yeni işleme faaliyeti
- Önemli teknoloji değişikliği
- Yeni veri kategorisi eklenmesi

> İki veya daha fazla "zorunlu" kriter sağlanıyorsa: VKED + (gerekirse) KVKK'ya danışma m.7.

### 2. VKED yapısı

```markdown
# Veri Koruma Etki Değerlendirmesi (VKED)

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

## 2. Hukuki Sebep Analizi
[KVKK m.5 veya m.6 dayanağı detaylı]

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
[Önlemler sonrası geriye kalan risk seviyesi — kabul edilebilir mi?]

## 7. KVKK Kurulu Danışma Gereksinimi (m.7)
[Artık risk hâlâ yüksekse → Kurul'a danışma gerek]

## 8. Onay & İmza
- Veri Sorumlusu Temsilcisi: [imza]
- DPO/KVKK Sorumlusu: [imza]
- Üst yönetim: [imza]
- Tarih: [PLACEHOLDER]

## 9. Gözden Geçirme Tarihi
[VKED yıllık veya değişiklik durumunda yenilenir]
```

### 3. Risk skorlama tablosu

| Olasılık \ Etki | Düşük | Orta | Yüksek |
|---|---|---|---|
| Düşük | Düşük | Düşük | Orta |
| Orta | Düşük | Orta | Yüksek |
| Yüksek | Orta | Yüksek | **Kritik** |

**Kritik veya Yüksek** artık risk → KVKK m.7 danışmaya yönlendir.

### 4. Önlem önerileri

Plugin, faaliyetin türüne göre standart önlem havuzundan seçim yapar:

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

### 5. Emsal kontrolü

```
mcp__yargi-mcp__search_kvkk_decisions(keywords="<faaliyet türü> VKED")
mcp__yargi-mcp__search_kvkk_decisions(keywords="yüksek risk profilleme")
```

İlgili kararları VKED'in 4.2 bölümüne entegre et: "Benzer faaliyetlerde KVKK Kurulu şu yorumu yapmıştır..."

## Çıktı

```
✓ VKED hazırlandı (12 sayfa)

🎯 VKED Sonucu: ORTA artık risk
  Faaliyet: Çalışan e-posta içeriği izleme
  Etkilenen kişi: 247 çalışan
  Veri kategorisi: İletişim + içerik (özel nitelikli olabilir)

📌 KVKK Kurulu Danışma: GEREKMEZ (artık risk: orta)

⚠️ Tespit Edilen Yüksek Riskler:
  - R1: Özel nitelikli içerik tesadüfen ifşa olabilir
    → Önlem: Anahtar kelime filtresi + manuel inceleme
  - R3: Yetkisiz erişim kalıcı log riski
    → Önlem: Erişim logu + 2-faktör

📚 Referans Kararlar:
  - KVKK 2023/86 — Çalışan e-posta izleme (aydınlatma şart)
  - KVKK 2021/1187 — Eski çalışan e-posta erişimi (m.5 yetersiz)

📋 Aksiyon Listesi:
  1. VKED'i şirket içi yönetim kuruluna sun
  2. Önerilen 7 önlemi uygula
  3. Çalışan aydınlatma metnini güncelle
  4. 12 ay sonra VKED'i yenile

⚠️ Bu VKED bir TASLAKTIR. Avukat onayı + KVKK uzmanı incelemesi gereklidir.
   Risk skorları subjektif değerlendirmedir; kuruluşa özgü ek faktörler eklenmelidir.
```

## TBB Meslek Kuralları

- VKED **iç değerlendirme** belgesidir; dış servise gönderilmez.
- Plugin "VKED yeterlidir, ceza almazsınız" tarzı garanti vermez.
- Çıkar çatışması olabilecek vakalarda (örn. müvekkilin VKED'inde kendi büronun rolü) plugin durur ve manuel inceleme önerir.
