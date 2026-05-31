# Karar & Mevzuat Atıf Kuralları — Halüsinasyon Önleme Disiplini

> Bu dosya plugin geliştiricileri ve skill yazarları içindir. Her skill, **KVKK Kurul kararı, mahkeme kararı VEYA spesifik mevzuat madde numarası** atfı verirken **bu kurallara sıkı sıkıya uymak zorundadır.** Bu, plugin'in en kritik güvenlik katmanıdır.
>
> ⚠️ **2026-05-31 turn-5 update'i:** Bu disiplin **KVKK ve mahkeme kararı numaraları** kadar **TBB / Avukatlık Kanunu / Yönetmelik madde numaraları** için de geçerlidir. Yanlış madde numarası, yanlış karar numarası kadar zararlıdır — üstelik mevzuat numaraları skill çıktısında daha sabit/sık tekrarlanan unsurdur.

## Neden Bu Kural Var?

Plugin canlı KVKK karar veritabanına bağlanmaz. Model eğitim verisindeki karar bilgisi:
- Zaman içinde güncellenmemiş olabilir (yeni kararlar var, eski kararlar revize edilmiş olabilir)
- Karar numaraları ve tarihleri tipik halüsinasyon riski taşır (model "2023/86" yerine yanlışlıkla "2023/89" üretebilir)
- Karar içeriği yanlış hatırlanabilir

Bir avukatın **yanlış karar numarasıyla** müvekkilini veya KVKK'ya gelen savunmayı destekleme girişimi:
- TBB Meslek Kuralları m.5 (yanıltıcı bilgi yasağı) ihlali
- Müvekkil zararı (red, kabul edilmez)
- Avukatın itibar kaybı

Bu nedenle prompt-level bir disiplin gerekir.

## Sert Kural — Tüm Karar Atıfı Veren Skill'lerde

```
⚠️ KARAR ATIF KURALI (zorunlu)

Spesifik bir KVKK Kurul kararı (veya başka mahkeme kararı) numarası, tarihi
veya alıntısı üretirken:

1. ASLA [doğrulanmalı] etiketi olmadan spesifik karar numarası, tarihi
   veya kavramı verme. Bu etiket pazarlık edilemez.

2. Karar numarasının doğruluğundan TAM EMİN DEĞİLSEN, sayı uydurma.
   Bunun yerine:
   - Sadece konu başlığını yaz ("çalışan e-posta izleme aydınlatma
     yükümlülüğü konulu kararlar mevcuttur")
   - Numara/tarih verme.
   - Kullanıcıya şu mesajı ver:

     "Bu konuda KVKK Kurul kararları olduğunu hatırlıyorum ancak
     numarayı kesin doğrulayamadığım için yazmıyorum. kvkk.gov.tr →
     Karar Özetleri sayfasından '[konu]' araması yapmanızı ve ilgili
     kararı bana yapıştırmanızı öneririm. Yapıştırdığınızda
     /kvkk-uyum-tr:karar-analizi ile detaylı analiz yaparım."

3. Karar metninden alıntı yaparken yalnızca kullanıcının sağladığı
   metin dışında alıntı yapma. "Karar şöyle der..." gibi tırnak
   içinde uydurma metin asla verme.

4. Yargıtay/Danıştay karar numaraları için aynı kural geçerli.
   Bu plugin KVKK için tasarlandı; başka mahkeme kararlarına atıf
   yapmaktan kaçın. Gerekirse kullanıcıya yönlendir.

5. "En son", "geçen yıl", "yakın zamanda" gibi belirsiz zaman
   ifadelerinden kaçın. Model eğitim verisindeki son tarih bilinmez;
   "yakın zamanda" yorumu güvenilmez olur.

6. MEVZUAT MADDE NUMARALARI — kararlar kadar dikkatli ol:

   - KVKK madde numarası (m.10, m.11, m.12/5 vb.): eğitim verinden
     iyi hatırlanır ama yine de düzenli kontrol et. Bu plugin'in
     core mevzuat çerçevesi olduğu için kullanıcı doğruluğa güvenir.

   - TBB Meslek Kuralları + 1136 sayılı Avukatlık Kanunu: İKİ AYRI
     KAYNAK. m.36 hem Av.K.'da (sır saklama yasağı) hem TBB MK'da
     (yakın madde) geçer — KARIŞTIRMA RİSKİ YÜKSEK. Doğrulanmış:
     "TBB Meslek Kuralları m.37 = müvekkil sırrı". Doğrulanmamış
     numaraları [doğrulanmalı] etiketiyle ver.

   - TBB Reklam Yasağı Yönetmeliği: TBB Meslek Kuralları'ndan AYRI
     bir Yönetmeliktir. "TBB MK m.55-61 reklam" gibi atıflar
     güvenilmez — birincil kaynak Yönetmeliktir; ilgili TBB MK
     maddeleri m.7/8/12 civarı (Aralık 2023 değişikliği) ama
     numara veriyorsan [doğrulanmalı] etiketi şart.

   - Yargıtay/Danıştay madde atıfları: KVKK için tasarlanan bu
     pluginde gereksizdir; gerekirse kullanıcıyı yönlendir.
```

## Pratik Test

Skill geliştirirken kendine sor:

| Soru | Cevap |
|---|---|
| Çıktıda "2023/86 sayılı karar" gibi spesifik numara var mı? | Var ise `[doğrulanmalı]` etiketi mutlaka eklenmiş mi? |
| Çıktıda "Kurul şöyle dedi: '...'" alıntı var mı? | Bu alıntı kullanıcının sağladığı metinden mi geliyor? |
| Skill kullanıcı karar sağlamamışsa serbest hatırlamayla mı çalışıyor? | Bu **kaçınılması gereken** durumdur — kullanıcıya doğrulama isteği geri ver. |
| Kullanıcı "şu konuda karar var mı?" diye sorduğunda doğrudan numara mı veriyorsun? | **Hayır.** Konu başlığı + "kvkk.gov.tr'den teyit edin" mesajı yeterli. |

## Skill-Spesifik Notlar

### `karar-analizi` Skill'i — Özel Disiplin

Bu skill **yapıştırılan metni analiz etme modunda kilitlenmiş** olmalıdır:
- Kullanıcı bir karar URL/metni sağlamazsa, plugin "Lütfen kararı paylaş" der ve serbest hatırlamayla çalışmaz.
- Eğer kullanıcı "şu konuda ne biliyorsun?" diye sorarsa, plugin: "Spesifik kararlar için kullanıcı sağlamış olmalı; konsept hakkında genel KVKK çerçevesi paylaşabilirim" der.

### `ihlal-triaji`, `risk-degerlendirme`, `aydinlatma-metni-uretici`, `ilgili-kisi-basvurusu` — Karar Referansı Bölümü

Bu skill'lerin "Karar referansı" çıktı bölümü şu yapıda olmalı:

```
📚 Konuyla ilgili kararlar [doğrulanmalı]:
   - [Konu başlığı 1 — numara yok, sadece konu]
   - [Konu başlığı 2]

⚠️ Karar numaraları ve tam metin için kvkk.gov.tr → Karar Özetleri.
   Spesifik bir karar metni paylaşırsan /kvkk-uyum-tr:karar-analizi
   ile detaylı yorumlayabilirim.
```

**Karar numarası yazılmaz.** Konu başlığı yeterli; kullanıcı kvkk.gov.tr'de o konuyu arar, doğru numarayla geri döner.

## Bu Disiplini Esnetmek

Topluluk katkısı olarak bu kuralı esnetmek isteyen biri olursa:
1. PR'da açık gerekçe verilmeli ("X koşulda Y'yi yapmak gerekiyor")
2. Maintainer onayı şart
3. Avukat olarak hukuki risk değerlendirmesi yapılmalı

Default olarak: **sert tutmak doğrudur.** Yanlış karar numarası vermenin maliyeti, "rahat" hissetmekten çok daha yüksektir.

Son güncelleme: 2026-05-31
