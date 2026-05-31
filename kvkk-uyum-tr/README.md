# kvkk-uyum-tr

> KVKK uyumu için Claude Code plugin'i. Türkiye'deki veri sorumluları ve avukatlar için.

**Versiyon:** 0.1.0 (Pilot)
**Hukuki Çerçeve:** KVKK m.10/11/12/13, VERBİS düzenlemesi, Kurul kararları (eğitim verisi + kullanıcı sağladığı)
**Üçüncü taraf MCP bağımlılığı:** Yok

## Bu Plugin Ne Yapar?

KVKK pratiğindeki **6 ana iş akışını** otomatize eder; her çıktı avukat onayına hazır taslaktır:

1. **Aydınlatma metni üretimi** — KVKK m.10'a uygun, sektör/iş türü/veri kategorisine özgü
2. **İlgili kişi başvurusu yanıtlama** — KVKK m.13/2 30 günlük yasal süre takibi + gerekçeli yanıt
3. **VERBİS uyum kontrolü** — Kayıt durumu, 6 aylık veri envanteri, sektörel zorunluluk
4. **Veri ihlali triajı** — KVKK m.12/5 + Kurul'un 24.01.2019 tarih ve 2019/10 sayılı kararı uyarınca 72 saatlik bildirim zarfı, ilgili kişi bildirim gerekçesi
5. **KVKK iyi uygulama risk değerlendirmesi** — Yüksek riskli işleme faaliyetleri için (KVKK'da yasal DPIA zorunluluğu yoktur; bu skill iyi uygulama olarak risk haritalama yapar)
6. **Politika sapma takibi** — Şirket politikası vs kullanıcının sağladığı son kararlar arasındaki diff

**Yardımcı skill'ler:**
- `cold-start-interview` — İlk kurulum
- `karar-analizi` — Kullanıcının sağladığı karar metnini Türk hukuku perspektifinden inceler

## Hızlı Başlangıç

### Kurulum
```
/plugin marketplace add kapital-legal/claude-for-tr-legal
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

**Bir karar metnini analiz et:**
```
/kvkk-uyum-tr:karar-analizi
```
kvkk.gov.tr'den bir karar URL'i veya tam metnini yapıştır. Plugin, kararın özünü, ilgili maddeleri ve senin pratiğine etkisini çıkarır.

## Tüm Skill Listesi

| Skill | Komut | Ne Yapar |
|---|---|---|
| `cold-start-interview` | `/kvkk-uyum-tr:cold-start-interview` | İlk kurulum, profil oluşturma |
| `aydinlatma-metni-uretici` | `/kvkk-uyum-tr:aydinlatma-metni-uretici` | KVKK m.10 aydınlatma metni |
| `ilgili-kisi-basvurusu` | `/kvkk-uyum-tr:ilgili-kisi-basvurusu` | KVKK m.13/2 başvurusu yanıtı |
| `verbis-uyum-kontrolu` | `/kvkk-uyum-tr:verbis-uyum-kontrolu` | VERBİS kayıt + envanter |
| `ihlal-triaji` | `/kvkk-uyum-tr:ihlal-triaji` | 72 saatlik bildirim zarfı |
| `risk-degerlendirme` | `/kvkk-uyum-tr:risk-degerlendirme` | KVKK iyi uygulama risk haritalama |
| `politika-fark-takibi` | `/kvkk-uyum-tr:politika-fark-takibi` | Politika vs karar farkı |
| `karar-analizi` | `/kvkk-uyum-tr:karar-analizi` | Kullanıcının sağladığı karar metni analizi |

## Veri Kaynakları & Doğrulama İlkesi

Plugin **resmi KVKK karar veritabanına doğrudan bağlanmaz.** Üç katmanlı çalışır:

1. **Model eğitim verisi** — KVKK genel çerçevesi, m.10/11/12/13 yorumları, geçmiş Kurul kararı örnekleri. Her atıf `[doğrulanmalı]` etiketli.
2. **Kullanıcının sağladığı içerik** — Senin kvkk.gov.tr'den (veya başka resmi kaynaktan) kopyaladığın karar URL'i veya metni. Plugin bunu okur, analiz eder, çerçeveler.
3. **Opsiyonel — kullanıcının kendi araçları** — Eğer Claude Cowork'e veya Claude Code'a kendi seçtiğin bir Türk hukuk veritabanı aracı bağladıysan, plugin onu da kullanır. Plugin **kendisi** bu araçları gerektirmez.

**Resmi kaynaklar (her zaman doğrulama için):**
- KVKK Kurul Kararları: https://www.kvkk.gov.tr/Icerik/?CategoryId=87
- KVKK Mevzuat: https://www.kvkk.gov.tr/Icerik/?CategoryId=85
- VERBİS: https://verbis.kvkk.gov.tr

## Bu Plugin Neyi YAPMAZ

- **Otomatik karar arama** — Plugin canlı KVKK veritabanına bağlanmaz. Karar arama için kullanıcı kendisi kvkk.gov.tr'ye gider veya kendi seçtiği aracı kullanır.
- **GDPR yorumu** — Yalnızca KVKK içindir. GDPR için Anthropic'in `privacy-legal` plugin'ine bakın.
- **Müvekkilinizin müvekkiline danışmanlık** — Çıktılar avukatın imzaladığı taslaktır; doğrudan ilgili kişiye gönderilmez.
- **KVKK Kurulu nezdinde dava açma/şikayet** — Şikayet hazırlığı yapar ancak avukat onayı + sunum sürecini değil.
- **Otomatik politika güncellemesi** — Politika fark raporu üretir; politikayı değiştirme kararı avukatındır.

## DPIA / Risk Değerlendirmesi Hakkında Önemli Not

**KVKK'da GDPR m.35 türü yasal bir DPIA (Veri Koruma Etki Değerlendirmesi) zorunluluğu yoktur.** Kişisel Verileri Koruma Kurulu, yüksek riskli işleme faaliyetleri için risk değerlendirmesi yapılmasını **iyi uygulama olarak tavsiye eder.** Bu plugin'in `risk-degerlendirme` skill'i bu çerçevede çalışır — yasal yükümlülük olarak değil, **risk haritalama ve önlem önerisi** olarak.

GDPR'den otomatik çevirim yapılmamıştır; çerçeve KVKK'nın kendi sistematiğine göre kurulmuştur.

## Hukuki Disclaimer

⚠️ Plugin çıktıları **avukat onayına hazır TASLAK**'tır. Hukuki tavsiye değildir.
- KVKK Kurulu yorumları zaman içinde değişir; her atıf `[doğrulanmalı]` etiketiyle sunulur, resmi kaynaktan doğrulanmalıdır.
- Plugin hata yapabilir, kaynağı yanlış gösterebilir.
- TBB Meslek Kuralları çerçevesinde üretilmiştir; her çıktının uygunluğu son sorumluluk avukattadır.

## Lisans

Apache 2.0 — Kapital Legal sponsorluğunda, açık kaynak.
