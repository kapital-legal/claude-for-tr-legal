# kvkk-uyum-tr

> KVKK uyumu için Claude Code plugin'i. Türkiye'deki veri sorumluları ve avukatlar için.

**Versiyon:** 0.1.0 (Pilot)
**Veri Kaynakları:** yargi-mcp (KVKK Kurulu kararları), KVKK m.10/13 mevzuatı, TBB Meslek Kuralları

## Bu Plugin Ne Yapar?

KVKK pratiğindeki **6 ana iş akışını** otomatize eder; her çıktı avukat onayına hazır taslaktır:

1. **Aydınlatma metni üretimi** — KVKK m.10'a uygun, sektör/iş türü/veri kategorisine özgü
2. **İlgili kişi başvurusu yanıtlama** — 30 günlük yasal süre takibi + gerekçeli yanıt
3. **VERBİS uyum kontrolü** — Kayıt durumu, 6 aylık veri envanteri, sektörel zorunluluk
4. **Veri ihlali triajı** — 72 saatlik KVKK bildirim zarfı, ilgili kişi bildirim gerekçesi
5. **DPIA / Etki Değerlendirmesi** — Yüksek riskli işleme faaliyetleri için
6. **Politika sapma takibi** — Şirket politikası vs son KVKK kararları diff

**Bonus skill'ler:**
- `cold-start-interview` — İlk kurulum
- `emsal-karar-arayici` — yargi-mcp ile canlı KVKK kararı arama

## Hızlı Başlangıç

### Kurulum
```
/plugin marketplace add /Users/av.haruneren/Desktop/Agent-dosyaları/claude-for-tr-legal
/plugin install kvkk-uyum-tr@claude-for-tr-legal
```
Claude Code'u yeniden başlat.

### İlk Komut
```
/kvkk-uyum-tr:cold-start-interview
```
5-15 dakikada büronun KVKK profilini öğrenir ve `~/.claude/plugins/config/claude-for-tr-legal/kvkk-uyum-tr/CLAUDE.md` dosyasına yazar.

### Kullanım Örnekleri

**Aydınlatma metni üret:**
```
/kvkk-uyum-tr:aydinlatma-metni-uretici
```
Soracak: sektör, veri kategorileri, işleme amaçları, aktarılan üçüncü taraflar, saklama süresi. Çıktı: KVKK m.10'a uygun metin + revize edilebilir Word-formatlı sürüm.

**İlgili kişi başvurusuna yanıt:**
```
/kvkk-uyum-tr:ilgili-kisi-basvurusu
```
Başvuru dilekçesini oku, talep kategorisini sınıflandır (erişim, silme, düzeltme, vb.), 30 günlük süre içinde gerekçeli yanıt taslakla. Reddedilebilecek talepleri KVKK m.13 dayanağıyla işaretle.

**Son KVKK kararlarını ara:**
```
/kvkk-uyum-tr:emsal-karar-arayici
```
İstediğin konuda yargi-mcp ile canlı KVKK Kurulu kararları getirir. Örn: "çalışan e-posta izleme", "açık rıza ile aydınlatma farkı", "veri ihlali idari yaptırım".

## Tüm Skill Listesi

| Skill | Komut | Ne Yapar |
|---|---|---|
| `cold-start-interview` | `/kvkk-uyum-tr:cold-start-interview` | İlk kurulum, profil oluşturma |
| `aydinlatma-metni-uretici` | `/kvkk-uyum-tr:aydinlatma-metni-uretici` | KVKK m.10 aydınlatma metni |
| `ilgili-kisi-basvurusu` | `/kvkk-uyum-tr:ilgili-kisi-basvurusu` | İlgili kişi başvurusu yanıtı |
| `verbis-uyum-kontrolu` | `/kvkk-uyum-tr:verbis-uyum-kontrolu` | VERBİS kayıt + envanter |
| `ihlal-triaji` | `/kvkk-uyum-tr:ihlal-triaji` | 72 saatlik bildirim zarfı |
| `etki-degerlendirme` | `/kvkk-uyum-tr:etki-degerlendirme` | DPIA üretimi |
| `politika-fark-takibi` | `/kvkk-uyum-tr:politika-fark-takibi` | Politika vs karar farkı |
| `emsal-karar-arayici` | `/kvkk-uyum-tr:emsal-karar-arayici` | Canlı KVKK karar arama |

## Gereksinimler

- **yargi-mcp kurulu** — KVKK Kurulu kararlarına erişim için zorunlu. Yoksa `emsal-karar-arayici`, `politika-fark-takibi`, `ihlal-triaji` skill'leri training data ile çalışır (sonuçlar `[verify]` etiketli).
- **Claude Pro/Max** aboneliği

## Bu Plugin Neyi YAPMAZ

- **GDPR yorumu** — Bu plugin yalnızca KVKK içindir. GDPR için Anthropic'in `privacy-legal` plugin'ine bakın.
- **Müvekkilinizin müvekkiline danışmanlık** — Çıktılar avukatın imzaladığı taslaktır; doğrudan ilgili kişiye gönderilmez.
- **KVKK Kurulu nezdinde dava açma** — Şikayet hazırlığı yapar ancak avukat onayı + sunum sürecini değil.
- **Otomatik politika güncellemesi** — Politika fark raporu üretir; politikayı değiştirme kararı avukatındır.

## Hukuki Disclaimer

⚠️ Plugin çıktıları **avukat onayına hazır TASLAK**'tır. Hukuki tavsiye değildir.
- KVKK Kurulu yorumları zaman içinde değişir; her atıf doğrulanmalıdır.
- Plugin hata yapabilir, kaynağı yanlış gösterebilir.
- TBB Meslek Kuralları çerçevesinde üretilmiştir; her çıktının uygunluğu son sorumluluk avukattadır.

## Lisans

Apache 2.0 — Kapital Legal sponsorluğunda, açık kaynak.
