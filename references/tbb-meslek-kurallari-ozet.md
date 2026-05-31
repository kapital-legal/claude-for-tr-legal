# TBB Meslek Kuralları & Avukatlık Kanunu — Plugin Geliştirici Özeti

> Bu dosya, plugin yazarken hangi düzenlemelere dikkat edilmesi gerektiğine dair kısa bir **referanstır**. Tam metin için: https://www.barobirlik.org.tr/
>
> ⚠️ **Önemli:** Aşağıdaki madde numaralarından bazıları reviewer feedback'i ile doğrulanmış, bazıları **`[doğrulanmalı]`** etiketli — kullanmadan önce barobirlik.org.tr veya mevzuat.gov.tr'den teyit edin. `references/karar-atif-kurallari.md` disiplini KVKK Kurul kararları kadar mevzuat madde numaraları için de geçerlidir.

## Kaynak Ayrımı — Çok Önemli

Türkiye'de avukatlık düzenlemeleri **iki ayrı kaynaktan** gelir; karıştırılmamalıdır:

1. **1136 sayılı Avukatlık Kanunu (Av.K.)** — Kanun seviyesi, yargısal yaptırım ağırlıklı
2. **TBB Meslek Kuralları (TBB MK)** — TBB Genel Kurulu tarafından kabul edilmiş, disiplin yaptırımı ağırlıklı

3. **TBB Reklam Yasağı Yönetmeliği** — TBB tarafından çıkarılan, reklam/tanıtım kısıtlamalarını detaylı düzenleyen ayrı düzenleme. Aralık 2023'te güncellenmiş; ilgili TBB MK maddeleri de (m.7, 8, 12 civarı) buna paralel revize edilmiştir.

## Plugin Geliştirme Açısından Kritik Maddeler

### Müvekkil Sırlarının Korunması (sır saklama yükümlülüğü)
- **TBB Meslek Kuralları m.37** — "Avukat meslek sırrı ile bağlıdır." (Disiplin seviyesi)
- **1136 sayılı Avukatlık Kanunu m.36** — Sır saklama yasağı (Kanun seviyesi)
- İki düzenleme birlikte uygulanır; disiplin kararları her ikisini ayrı ayrı zikreder.
- **Plugin etkisi:** Hiçbir skill müvekkil ismi/dosya numarası içeren çıktıyı üçüncü servislere geçirmemeli. **Anthropic API'sine veri iletildiği unutulmamalı** — `references/veri-isleme-bildirimi.md` çerçevesinde değerlendirilmeli. Anonim örnekler ("Şirket [X]", "Müvekkil [A]") tercih edilmeli.
- **Test:** Skill çıktısında PII varsa anonimleştirme önerilir. Plugin kullanıcının müvekkil ismi yazmasına izin verir ama uyarır.

### Avukatlık Tekeli (hukuki tavsiye yalnızca avukat tarafından)
- **1136 sayılı Avukatlık Kanunu m.35** [doğrulanmalı] — Avukatlık tekeli ve avukatın iş yapma yetkisi.
- Bu hüküm avukat olmayan kişinin hukuki tavsiye vermesinin sınırlarını çizer.
- **Plugin etkisi:** Plugin çıktıları **"avukat onayına hazır taslak"** olarak nitelendirilmelidir, "hukuki tavsiye" olarak değil. Her skill bu disclaimer'ı içermelidir.

### Reklam ve Tanıtım
- **TBB Reklam Yasağı Yönetmeliği** — birincil kaynak (Aralık 2023 değişikliği önemli)
- **TBB Meslek Kuralları m.7, m.8, m.12** [doğrulanmalı] — Reklam/tanıtım sınırlarını içeren maddeler (Aralık 2023 değişikliğiyle Yönetmelik paralel revize edilmiştir).
- **Plugin etkisi:** LinkedIn/blog içerik üreten skill'ler (örn. linkedin-agency entegrasyonu) reklam sınırlarını gözetmeli — sonuç garantisi, övgü ifadeleri, müvekkil isimleriyle tanıtım yasak. Spesifik madde gerektiğinde **avukat kullanıcı kendi baroştürüne ve güncel Yönetmelik metnine danışmalıdır.**

### Çıkar Çatışması
- **TBB Meslek Kuralları m.34-35** [doğrulanmalı]
- **1136 sayılı Av.K. m.38** [doğrulanmalı] — Yasaklar (çıkar çatışması dahil)
- **Plugin etkisi:** Litigation / commercial-legal plugin'leri "matter intake" sırasında çıkar çatışması kontrolü içermelidir.

### Doğru ve Tam Bilgi
- **TBB Meslek Kuralları m.5** [doğrulanmalı] — Avukat müvekkiline yanıltıcı bilgi vermez.
- **Plugin etkisi:** Plugin `[doğrulanmalı]` etiketiyle belirsiz/training data kaynaklı bilgileri işaretlemelidir.

### Vekalet Ücreti
- **1136 sayılı Av.K. m.163-168** [doğrulanmalı] — Vekalet ücreti hükümleri Kanun'da düzenlenmiştir.
- **Avukatlık Asgari Ücret Tarifesi** — Her yıl güncellenir (TBB tarafından).
- **Plugin etkisi:** "Ücret hesaplayıcı" skill'leri her yıl güncellenen tarife referansı içermelidir.

### Saklama ve Arşivleme
- **1136 sayılı Av.K. ilgili madde + KVKK uyumu** [doğrulanmalı]
- **Plugin etkisi:** Document management skill'leri saklama süresini gözetmelidir.

---

## Plugin Çıktı Kontrol Listesi

Her skill çıktısında:

- [ ] **Avukat onayı disclaimer'ı** mevcut mu? ("Bu bir taslaktır, hukuki tavsiye değildir.")
- [ ] **Müvekkil ismi/PII** içermiyor mu? (Anonim referanslar mı?)
- [ ] **Kaynak gösterimi** her madde/karar atfında var mı?
- [ ] **Belirsizlik etiketi `[doğrulanmalı]`** training data kaynaklı bilgilerde kullanılmış mı?
- [ ] **Reklam unsuru** içermiyor mu? (Sonuç garantisi, övgü)
- [ ] **TBB Meslek Kuralları + Avukatlık Kanunu** ile çelişen önerme yok mu?
- [ ] **Madde numarası doğrulaması:** Spesifik madde numaraları kullanıldıysa `[doğrulanmalı]` etiketi var mı veya numara teyit edildi mi?

Bu kontroller bir hook ile otomatize edilebilir — `hooks/check-tbb-compliance.md` (planlı).

---

## Disclaimer Şablonu (Tüm Skill Çıktılarında Olması Gereken)

```
⚠️ Bu çıktı bir TASLAKTIR. Hukuki tavsiye değildir.

- Avukat onayına hazır şekilde sunulmuştur; nihai imza/gönderim avukatın sorumluluğundadır.
- Kaynak gösterilen tüm karar ve madde atıfları doğrulanmalıdır.
- Plugin hata yapabilir; her çıktı eleştirel gözle okunmalıdır.
- Türkiye Barolar Birliği Meslek Kuralları ve Avukatlık Kanunu gözetilerek üretilmiştir.
```

---

## Güncellemeler

TBB Meslek Kuralları, Avukatlık Kanunu ve TBB Reklam Yasağı Yönetmeliği'nde değişiklik olduğunda bu dosya güncellenmelidir. Son güncelleme: 2026-05-31 — Turn 5 reviewer feedback'iyle madde numaraları gözden geçirildi; doğrulanmamış olanlar `[doğrulanmalı]` etiketiyle işaretlendi.
