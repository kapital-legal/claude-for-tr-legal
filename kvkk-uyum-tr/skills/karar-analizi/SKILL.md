---
name: karar-analizi
description: >
  Kullanıcının sağladığı KVKK Kurulu karar metnini (URL veya yapıştırılmış metin)
  Türk hukuku perspektifinden analiz eder. Kararın özünü çıkarır, ilgili maddeleri
  haritalar, kullanıcının pratiğine etkisini değerlendirir. "Karar analiz et",
  "kararı incele", "şu karar bizim büroyu etkiler mi?", "emsal karar yorumla"
  söylemlerinde tetiklenir. Resmi karar veritabanına otomatik bağlanmaz —
  kullanıcı karar metnini sağlar.
argument-hint: "[url <karar-url>] [veya kullanıcı yapıştırılmış metin ile]"
---

# /kvkk-uyum-tr:karar-analizi

> **Bu skill kullanıcının sağladığı kararı analiz eder.** Plugin **canlı KVKK veritabanına bağlanmaz**. Resmi kaynak: https://www.kvkk.gov.tr — kullanıcı buradan ilgili kararı bulur, URL veya metni bu skill'e verir.

## ⚠️ ZORUNLU KURAL — Yapıştırılan Metin Olmadan Çalışma

**Eğer kullanıcı bir karar URL'i, dosya yolu veya yapıştırılmış metin SAĞLAMAMIŞSA:**

1. Skill durur. Serbest hatırlamayla karar üretmez.
2. Kullanıcıya şu mesajı verir:

   > Bu skill bir karar metni veya URL üzerinde çalışır — kendi başına KVKK veritabanına bağlanmaz. Lütfen şunlardan birini yap:
   >
   > 1. **URL ver:** kvkk.gov.tr/Icerik/... gibi resmi karar URL'i yapıştır.
   > 2. **Metin yapıştır:** Karar tam metnini doğrudan buraya kopyala.
   > 3. **Dosya yolu:** PDF/MD/TXT karar dosyası varsa yolunu ver (örn. `~/Downloads/karar.pdf`).
   >
   > Eğer "şu konuda ne diyor?" sorum varsa kvkk.gov.tr → Karar Özetleri sayfasında ilgili konuyu ara, kararı bul, bana getir.

3. Bu durumda spesifik karar numarası, tarihi veya alıntı **asla** üretmez.

Bu kural sert prompt-level kuraldır; pazarlık edilemez. Detay: `references/karar-atif-kurallari.md`

## Cold-start kontrolü
Profil yoksa interview'a yönlendir.

## İş Akışı

### 1. Karar girdisini al

Kullanıcı üç şekilde karar sağlayabilir:

**Yöntem A — URL:**
```
/kvkk-uyum-tr:karar-analizi
> Karar URL: https://www.kvkk.gov.tr/Icerik/7593/2023-86
```
Plugin `WebFetch` ile sayfa içeriğini çeker, karar metnini çıkartır.

**Yöntem B — Yapıştırılmış metin:**
```
/kvkk-uyum-tr:karar-analizi
> [Kullanıcı kararı tamamen yapıştırır]
```

**Yöntem C — Dosya yolu:**
```
/kvkk-uyum-tr:karar-analizi --file ~/Downloads/karar-2023-86.pdf
```
Plugin dosyayı `Read` ile okur (PDF/MD/TXT).

### 2. Karar özetleme

Aşağıdaki yapıyla özet çıkar:

```markdown
# Karar Özeti

**Karar No:** [Kullanıcının sağladığı metinden çıkar — örnek vermem]
**Tarih:** [Kullanıcının sağladığı metinden çıkar — örnek vermem]
**Konu:** [Bir cümlede ne hakkında]
**URL:** [orijinal kaynak]

## Olay
[1 paragrafta olay özeti — anonim taraflar]

## Tartışılan KVKK Maddeleri
- m.X — [konunun ana ekseni]
- m.Y — [yan eksen]

## Kurul'un Kararı
[Karar sonucu net olarak]

## Kurul'un Gerekçesi
[Anahtar yorumları madde madde]
```

### 3. Pratik etki değerlendirmesi

Kullanıcının profilinden (`CLAUDE.md`):
- Bu büronun/şirketin sektörüne uyuyor mu?
- Bu kararın işaret ettiği davranış pratikte yapılıyor mu?
- Politika veya prosedür değişikliği gerektirir mi?

Çıktı:

```markdown
## Senin Pratiğine Etkisi

**Doğrudan etki:** [düşük/orta/yüksek]
**Etkilenebilecek faaliyet:** [örn. "Çalışan e-posta izleme politikası"]
**Önerilen aksiyon:** [örn. "Aydınlatma metnine 'X' eklenmesi"]
**Daha fazla incelenmeli mi?** [evet/hayır]

İlgili skill önerisi:
- /kvkk-uyum-tr:politika-fark-takibi
- /kvkk-uyum-tr:aydinlatma-metni-uretici
```

### 4. Benzer konular (opsiyonel)

**ZORUNLU KURAL:** Spesifik karar numarası uydurma. Yalnızca **konu başlıkları** sun, numara verme.

```
📚 Aynı eksendeki konularda KVKK Kurul kararları mevcut olabilir
   (numaraları doğrulanmamış — kvkk.gov.tr'den arama önerisi):

- Eski çalışan kurumsal e-posta hesabına erişim
- Ticari e-posta toplama / pazarlama izni
- [vb. konu başlıkları]

⚠️ Numara/tarih kesin doğrulanmadan üretilmez. Konu üzerinden
   kvkk.gov.tr → Karar Özetleri sayfasında arama yap, ilgili kararı
   bana yapıştır; bu skill'i tekrar çağırırsan detaylı analiz yaparım.
```

Bu disiplin `references/karar-atif-kurallari.md` ile zorunlu kılınmıştır. Detaylı pratik test ve gerekçe orada.

### 5. TBB Meslek Kuralları kontrolü

Karar analizinde plugin şu durumlarda uyarı verir:
- Karar müvekkilinizin durumunu birebir ilgilendiriyorsa → "Bu durum müvekkilinize özgü hukuki tavsiye gerektirir, plugin sadece çerçeve çıkarır"
- Karar'ın etkisi avukatlık tekeline temas ediyorsa → "TBB Meslek Kuralları m.X dikkate alınmalı"

## Çıktı

```
✓ Karar analizi tamamlandı

📋 Karar: KVKK [karar no — kullanıcının verdiği metinden] ([tarih])
   Konu: [Kararın gerçek konusu — kullanıcının sağladığı metinden]

📄 Özet:
   [Yukarıdaki yapı]

🎯 Pratiğine Etkisi: ORTA
   - Aydınlatma metninin "e-posta izleme amacı" bölümü gözden geçirilmeli
   - İç politikada "makul şüphe" tetikleyicisi tanımı eksik olabilir

📚 İlgili kararlar [doğrulanmalı]:
   - [Liste — kvkk.gov.tr'den doğrula]

💡 Sıradaki adım önerileri:
   - /kvkk-uyum-tr:politika-fark-takibi (politikana karşı diff)
   - /kvkk-uyum-tr:aydinlatma-metni-uretici (güncelleme)

⚠️ Bu analiz TASLAKTIR. Karar metninin yorumu kullanıcının verdiği içeriğe
   dayalıdır; resmi kararın güncel hali kvkk.gov.tr'den teyit edilmelidir.
   Hukuki tavsiye değildir.
```

## Bu Skill Neyi YAPMAZ

- **Karar arama** — Plugin canlı KVKK veritabanına bağlanmaz. Kullanıcı kvkk.gov.tr'den arama yapar, kararı buraya getirir.
- **Müvekkil dosyası analiz** — Bu skill sadece kamuya açık kararları analiz eder; müvekkil dosyası için ayrı süreç gerekir (avukat müzakeresi).
- **Karar metninin doğruluğunu garanti** — Kullanıcının sağladığı metnin orijinal karara birebir uygunluğunu plugin doğrulayamaz; resmi kaynaktan teyit kullanıcıya aittir.

## TBB Meslek Kuralları

- Bu skill **kararı analiz eder**, hukuki tavsiye üretmez.
- Müvekkil ismi/PII içeren karar metni paylaşılırsa plugin uyarır ve anonimleştirme önerir.
- Plugin'in çıkardığı yorum, avukat onayı olmadan müvekkile aktarılmaz.
