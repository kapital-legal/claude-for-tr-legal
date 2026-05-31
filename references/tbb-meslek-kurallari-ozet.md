# TBB Meslek Kuralları — Plugin Geliştirici Özeti

> Bu dosya, plugin yazarken hangi TBB Meslek Kurallarına dikkat edilmesi gerektiğine dair kısa bir referanstır. Tam metin için: https://www.barobirlik.org.tr/

## Plugin Geliştirme Açısından Kritik Maddeler

### Müvekkil Sırlarının Korunması
- **TBB MK m.36** ve devamı: Avukat müvekkili ile arasındaki bilgileri açıklayamaz.
- **Plugin etkisi:** Hiçbir skill, müvekkil ismi/dosya bilgisi içeren çıktıyı dış servise göndermemeli. yargi-mcp **resmi devlet API'lerine** bağlı olduğu için doğrudan kullanım sorun değil; ancak müvekkilin spesifik ismini sorguya katmak avukatlık etiğine aykırıdır.
- **Test:** Skill çıktısında "Müvekkil [A]", "Şirket [X]" gibi anonim referanslar kullanılmalı. Plugin gerçek müvekkil ismiyle sorgu yaparken kullanıcıyı uyarmalı.

### Çıkar Çatışması
- **TBB MK m.34-35:** Avukat çıkar çatışması bulunan iki tarafı temsil edemez.
- **Plugin etkisi:** Litigation / commercial-legal plugin'leri "matter intake" sırasında çıkar çatışması kontrolü içermelidir. Önceki dosyalarla otomatik karşılaştırma.

### Hukuki Tavsiyenin Niteliği
- **TBB MK m.4:** Avukatlık tekeli — avukat olmayan kişi hukuki tavsiye veremez.
- **Plugin etkisi:** Plugin çıktıları **"avukat onayına hazır taslak"** olarak nitelendirilmelidir, "hukuki tavsiye" olarak değil. Her skill bu disclaimer'ı içermelidir.

### Doğru ve Tam Bilgi
- **TBB MK m.5, m.27:** Avukat müvekkiline yanıltıcı bilgi vermez, gerçeği saklamaz.
- **Plugin etkisi:** Plugin "[verify]" etiketiyle belirsiz/training data kaynaklı bilgileri işaretlemelidir. Belirsizliği gizlememelidir.

### Reklam ve Tanıtım
- **TBB MK m.55-61:** Avukatın kendisini tanıtırken sınırları var.
- **Plugin etkisi:** LinkedIn/blog için içerik üreten skill'ler (linkedin-agency entegrasyonu) reklam sınırlarını gözetmelidir — sonuç garantisi, övgü ifadeleri, müvekkil isimleriyle tanıtım yasak.

### Vekalet Ücreti
- **TBB MK m.42-54 + Avukatlık Asgari Ücret Tarifesi:** Vekalet ücretinin alt sınırları belirli.
- **Plugin etkisi:** "Ücret hesaplayıcı" skill'leri her yıl güncellenen tarife referansı içermelidir.

### Saklama ve Arşivleme
- **TBB MK + KVKK uyumu:** Müvekkil dosyaları belirli süre saklanmalı, sonra imha.
- **Plugin etkisi:** Document management skill'leri saklama süresini gözetmelidir.

---

## Plugin Çıktı Kontrol Listesi

Her skill çıktısında:

- [ ] **Avukat onayı disclaimer'ı** mevcut mu? ("Bu bir taslaktır, hukuki tavsiye değildir.")
- [ ] **Müvekkil ismi/PII** içermiyor mu? (Anonim referanslar mı?)
- [ ] **Kaynak gösterimi** her madde/karar atfında var mı?
- [ ] **Belirsizlik etiketi `[verify]`** training data kaynaklı bilgilerde kullanılmış mı?
- [ ] **Reklam unsuru** içermiyor mu? (Sonuç garantisi, övgü)
- [ ] **TBB Meslek Kuralları** ile çelişen önerme yok mu?

Bu kontroller bir hook ile otomatize edilebilir — `hooks/check-tbb-compliance.md` (planlı).

---

## Disclaimer Şablonu (Tüm Skill Çıktılarında Olması Gereken)

```
⚠️ Bu çıktı bir TASLAKTIR. Hukuki tavsiye değildir.

- Avukat onayına hazır şekilde sunulmuştur; nihai imza/gönderim avukatın sorumluluğundadır.
- Kaynak gösterilen tüm karar ve madde atıfları doğrulanmalıdır.
- Plugin hata yapabilir; her çıktı eleştirel gözle okunmalıdır.
- Türkiye Barolar Birliği Meslek Kuralları gözetilerek üretilmiştir.
```

---

## Güncellemeler

TBB Meslek Kuralları'nda değişiklik olduğunda bu dosya güncellenmelidir. Son güncelleme: 2026-05-26.
