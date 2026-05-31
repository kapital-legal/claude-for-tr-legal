---
name: emsal-karar-arayici
description: >
  yargi-mcp üzerinden KVKK Kurulu kararlarını arar. Kullanıcının sorduğu konu/
  kavram için canlı veritabanından son kararları getirir, ilgili olanları
  özetler, post/dilekçe/danışman raporu için referans listesi sunar. "Emsal
  bul", "KVKK kararı ara", "X hakkında karar var mı?", "Kurul ne demiş?"
  söylemlerinde tetiklenir. Diğer skill'lerden de çağrılabilir.
argument-hint: "<konu> [--son-N-ay 6|12|24|tüm] [--detay 0|1|2: özetin derinliği]"
---

# /kvkk-uyum-tr:emsal-karar-arayici

## Ön Kontrol

yargi-mcp aracının erişilebilir olduğunu doğrula:
- `mcp__yargi-mcp__search_kvkk_decisions` mevcut mu?
- Test sorgusu: küçük bir dummy arama → cevap alındı mı?

Yoksa kullanıcıyı uyar:
> ⚠️ yargi-mcp kurulu değil veya erişilemiyor. Bu skill canlı KVKK Kurulu kararlarına ulaşamayacak. Training data'dan getirilen sonuçlar `[verify]` etiketi taşıyacaktır.
> 
> Kurulum: https://github.com/saidsurucu/yargi-mcp

## İş Akışı

### 1. Sorgu hazırlığı

Kullanıcının doğal dildeki sorusunu KVKK arama anahtar kelimelerine çevir.

**Örnekler:**
| Kullanıcı sorusu | yargi-mcp sorgusu |
|---|---|
| "Çalışan e-posta izleme yasal mı?" | `çalışan e-posta izleme` |
| "Eski çalışan hesabına girebilir miyim?" | `eski çalışan e-posta erişim` |
| "Pazarlama e-posta atabilir miyim?" | `ticari elektronik ileti açık rıza` |
| "Çocuk verisi nasıl korunur?" | `çocuk kişisel veri` |
| "Sağlık verisi açık rıza yeterli mi?" | `özel nitelikli sağlık veri rıza` |
| "ABD'ye veri aktarımı?" | `yurt dışı veri aktarımı ABD` |
| "Web sitesi çerez izni?" | `çerez açık rıza web sitesi` |

Çoklu sorgu gerekirse paralel çek.

### 2. yargi-mcp çağrısı

```
mcp__yargi-mcp__search_kvkk_decisions(
  keywords="<dönüştürülmüş sorgu>",
  page=1
)
```

İlk sayfa 10 sonuç verir. Kullanıcı "daha fazla" derse page=2, 3...

### 3. Tarih filtresi

`--son-N-ay` parametresi (varsayılan: tümünü göster):
- Sonuçlar arasından `publication_date` veya `decision_number`'a göre filtreleme yap.
- Karar numarası formatı `YYYY/N` — yıl bilgisini buradan da çıkarabilirsin.

### 4. Sonuçları sırala

Önceliklendirme:
1. **En yeni karar** önce (tarih DESC)
2. **Aynı konuya yakın anahtar kelimeler** sonra
3. **Belirsiz/genel** kararlar en sona

### 5. Detay seviyesi (`--detay`)

**Seviye 0 — Liste:**
```
- 2023/86 (19.01.2023) — Çalışan e-posta izleme aydınlatma yükümlülüğü
  URL: https://kvkk.gov.tr/Icerik/7593/2023-86
- 2021/1187 (25.11.2021) — Eski çalışan e-posta erişim m.5 yetersiz
  URL: ...
```

**Seviye 1 — Özet (varsayılan):**
```
## Karar 2023/86 (19.01.2023) — Çalışan E-posta İzleme

**Konu:** İşveren çalışanın kurumsal e-postasını izleyebilir mi?

**Kurul Tespiti:** Şirket çalışanlarının e-posta adresleri üzerinde işleme 
faaliyetlerinin (a) güvenlik, (b) iş amacı dışı kullanım tespiti, (c) makul 
şüpheli disiplin soruşturması, (d) hukuka aykırı eylemlerin önlenmesi 
amaçlarıyla yapılabileceği; ancak çalışanlara önceden aydınlatma yapılmış 
olması ve iş yeri kurallarına uygunluk şartı arandığı belirtilmiştir.

**Uygulama Notu:** Politikalarda izleme yetkisi yazmak yetmez; çalışanlar 
açıkça bilgilendirilmelidir.

**URL:** https://www.kvkk.gov.tr/Icerik/7593/2023-86

---
```

**Seviye 2 — Tam Metin:**
- `mcp__yargi-mcp__get_kvkk_document_markdown(id=...)` ile tam karar metnini çek.
- Markdown halinde sun (uzun olabilir, kullanıcıya uyarı ver).

### 6. Kullanım önerileri

Sonuç listesi sonunda:

```
💡 Bu kararları nasıl kullanabilirsin:

📝 İçerik için (LinkedIn/blog):
  - 2023/86 "Çalışan e-posta izleme" → Pillar 1 (ISO 27001) için B2B post
  
🔍 Dilekçe/danışman raporu için:
  - 2021/1187'de "eski çalışan" yaklaşımı, müvekkilinin senaryosunda dayanak

🎯 İç eğitim için:
  - 3 karar ile çalışan veri politikası eğitimi paketi
```

### 7. İlgili plugin skill'lerine yönlendirme

Konuya göre:
- Aydınlatma metni → `/kvkk-uyum-tr:aydinlatma-metni-uretici`
- İhlal triajı → `/kvkk-uyum-tr:ihlal-triaji`
- DPIA → `/kvkk-uyum-tr:etki-degerlendirme`
- Politika güncelleme → `/kvkk-uyum-tr:politika-fark-takibi`

## Çıktı

```
🔍 KVKK Kurul Kararı Araması: "çalışan e-posta izleme"

📊 7 ilgili karar bulundu (Mayıs 2025 - Mayıs 2026)

### 1. Karar 2023/86 (19.01.2023) ⭐ EN İLGİLİ
[Seviye 1 özet]

### 2. Karar 2021/1187 (25.11.2021)
[Seviye 1 özet]

### 3-7. ... 

💡 Önerilen Adımlar:
  - Aydınlatma metni güncellemek için: /kvkk-uyum-tr:aydinlatma-metni-uretici
  - Politika diff için: /kvkk-uyum-tr:politika-fark-takibi
  - LinkedIn içerik için: bu kararlar Pillar 1 ile uyumlu

⚠️ Karar metinleri ve URL'leri yargi-mcp aracılığıyla doğrudan KVKK Kurulu
   sitesinden çekilmiştir. Yine de **alıntı yapmadan önce mutlaka URL'den 
   teyit edin** — KVKK kararları zaman içinde güncellenebilir veya kaldırılabilir.
```

## TBB Meslek Kuralları

- Bu skill **hiçbir müvekkil ismi/PII**'yi yargi-mcp'ye geçirmez.
- Kullanıcı sorgu yazarken müvekkil ismi yazarsa plugin uyarır ve sorguyu anonimleştirme önerir.
- Karar atıfları her zaman **kaynak URL'i ile** sunulur — doğrulanabilirlik şart.
