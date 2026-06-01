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

KVKK m.11'de sayılan haklar üzerinden işaretle (a–ğ bentleri; birden fazla olabilir):

| Bent | Hak | Açıklama | Reddedilebilir mi? |
|---|---|---|---|
| (a) | İşlenip işlenmediği | "Verim işleniyor mu?" | Hayır — bilgi verme zorunlu |
| (b) | İşlenmişse bilgi | "Hangi veriler, niye?" | Sınırlı (m.28 istisnaları çerçevesinde) |
| (c) | İşleme amacı ve uyumlu kullanım | "Amacına uygun mu işleniyor?" | Hayır |
| (ç) | Aktarılan üçüncü kişiler (yurt içi/dışı) | "Kime aktarıldı?" | Sınırlı (m.28) |
| (d) | Düzeltme | "Eksik/yanlış veriyi düzelt" | Hayır — yerine getirilmeli |
| (e) | Silme / yok etme | "Verimi sil" | Bazı durumlarda (m.7 şartlarına göre) |
| (f) | (d) ve (e) işlemlerinin aktarılan üçüncü kişilere bildirilmesi | "Düzeltme/silmeyi aktarılan taraflara da ilet" | Bazı durumlarda |
| (g) | Otomatik analize itiraz | "Sırf otomatik analiz aleyhe sonuç doğurmasın" | Sınırlı |
| (ğ) | Zararın giderilmesini talep (tazminat talebi) | "KVKK'ya aykırı işleme nedeniyle zarara uğradım, giderilmesini istiyorum" | Talep m.11 kapsamında başvuruya konu olur; tazminat **davası** ise KVKK m.14 saklı tuttuğu üzere genel mahkemede görülür |

> ⚠️ **Tazminat hakkının konumu:** Zararın giderilmesini talep etme hakkı KVKK m.11/1-(ğ)'dedir; veri sorumlusuna başvuruya konu olabilir. KVKK m.14, bu talebe karşılık tazminat **davasının** genel hükümlere göre açılma hakkını ayrıca saklı tutar. Plugin, "tazminat m.11 dışıdır" yanılgısını yapmaz.

### 4. Reddi gerektirebilecek/sınırlandıran durumlar (KVKK m.28)

KVKK m.28 iki ayrı fıkrayla işler. Plugin bu ikisini karıştırmaz:

**m.28/1 — Kanunun tamamen kapsam dışı tuttuğu işlemeler (istisna):**
- Madde 28/1 fıkrasında sayılan haller (örn. ulusal savunma, ulusal güvenlik, kamu güvenliği, soruşturma/kovuşturma kapsamındaki işleme vb.) Kanun'un kapsamı dışındadır.
- Bu hallerde "başvuru reddi" değil, **Kanun'un uygulanmadığı** durum söz konusudur.
- Plugin bu fıkraya ancak teknik gerekçeye işaret ederek atıf yapar; kullanıcıya "bu durumda m.28/1 kapsamı incelenmeli, başvuru yanıtının niteliği değişir" diyerek avukat müzakeresine yönlendirir.

**m.28/2 — Kanunun kısmen uygulanmadığı haller (kısmi istisna):**
- Madde 28/2 fıkrasında sayılan haller (örn. kişisel verilerin yetkili kişi/kuruluş tarafından yürütülen denetim faaliyetleri çerçevesinde işlenmesi, ticari sırrın korunması için gerekli olan işleme vb.).
- Bu hallerde belirli yükümlülükler (örn. m.10 aydınlatma, VERBİS) uygulanmaz ama Kanun tamamen kapsam dışı kalmaz.

**Pratik özet — başvuruya nasıl yansır:**
| Durum | Olası gerekçe |
|---|---|
| Diğer kişilerin haklarını ihlal | m.28 değil; ölçülülük + diğer mevzuat dayanakları |
| Şirket ticari sırrı | m.28/2 kapsamında kısmi istisna olabilir |
| Devam eden hukuki süreç | m.28/1 (soruşturma/kovuşturma) veya genel hükümler |
| Hukuki saklama yükümlülüğü | m.6/3 + ilgili sektörel mevzuat (örn. VUK) |
| Kamu güvenliği | m.28/1 |
| Suç önleme/soruşturma | m.28/1 |

Tespit edilirse plugin **net dayanakla** (m.28/1 mi m.28/2 mi, hangi bent) flag'ler ve avukat müzakeresine yönlendirir; tek başına ret kararı önermez.

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
Bu cevabımızdan tatmin olmamanız veya cevabımızı yetersiz bulmanız hâlinde, KVKK m.14 uyarınca **cevabımızı öğrendiğiniz tarihten itibaren 30 gün ve her hâlükârda başvuru tarihinden itibaren 60 günü geçmemek üzere** Kişisel Verileri Koruma Kurulu'na şikâyette bulunma hakkınız saklıdır (https://kvkk.gov.tr).

Saygılarımızla,
[Veri Sorumlusu] / [Yetkili Kişi]
```

### 6. Karar referansı (opsiyonel)

**ZORUNLU KURAL:** Spesifik karar numarası uydurma. Yalnızca **konu başlığı** sun.

```
📚 Bu talep kategorisinde KVKK Kurul kararları mevcut olabilir
   (numara/tarih doğrulanmamış):
- [Konu başlığı 1 — sadece konu, numara yok]
- ...

⚠️ Sayı uydurmuyorum. kvkk.gov.tr → Karar Özetleri'nde
   konuyu ara, ilgili kararı bana yapıştır;
   /kvkk-uyum-tr:karar-analizi ile detaylı yorumlayayım.
```

Bu disiplin sert prompt-level kuraldır. Detay: `references/karar-atif-kurallari.md`

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

📚 Bu konuda KVKK Kurul kararları mevcut olabilir (numarasız):
  - [Sadece konu başlığı, "2023/XXX" gibi sayı yok]
  - kvkk.gov.tr'den teyit edip /kvkk-uyum-tr:karar-analizi'ne yapıştırın

⚠️ Bu cevap TASLAKTIR. Avukat onayı gerektirir.
   Kullanılan KVKK madde atıfları doğrulanmalıdır.
   Karar atıfları kvkk.gov.tr'den teyit edilmelidir.
```

## TBB Meslek Kuralları

- İlgili kişinin **ismi/PII** çıktıda anonim tutulur (sadece taslakta yer alır).
- Reddedilen talep için **kesinleşmiş bir hukuki tavsiye** olarak değil, taslak olarak sunulur.
- Müvekkil/şirketin ticari sırrı veya çıkar çatışması varsa avukat bilgilendirilmeden cevap üretilmez.
