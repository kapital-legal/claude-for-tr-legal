# Katkı Rehberi

Türk avukatlık camiası için açık kaynak bir araç inşa ediyoruz. Katkılar memnuniyetle karşılanır.

## Ne Tür Katkılara Açığız?

### Yüksek Öncelik
1. **P1/P2 plugin'leri** — KVKK, fikri mülkiyet, şirketler, regülasyon, dava yönetimi skill'leri
2. **Opsiyonel araç destekleri** — Kullanıcının kendi seçtiği araçlarla skill'lerin daha iyi çalışmasını sağlayacak iyileştirmeler (zorunluluk eklemeden)
3. **TBB Meslek Kuralları kontrolleri** — her plugin'de uyum gözeticisi
4. **UDF dilekçe çıktısı** — udf-cli entegrasyonu (Tamamen tipik avukat workflow'u)
5. **Test senaryoları** — `references/test-cases/` altında gerçek vakaya benzer (anonim) örnekler

### Düşük Öncelik (şimdilik kabul etmiyoruz)
- Frontend/UI değişiklikleri (Claude Code/Cowork zaten arayüz sağlıyor)
- Türk hukuku dışındaki içerikler
- Müvekkil dosyası/PII içeren örnekler

## Yeni Plugin Ekleme

### 1. İskelet kopyala
```bash
cp -r kvkk-uyum-tr/ <yeni-plugin>-tr/
cd <yeni-plugin>-tr/
```

### 2. `plugin.json`'u güncelle
```json
{
  "name": "<yeni-plugin>-tr",
  "version": "0.1.0",
  "description": "<kısa açıklama, hangi mevzuata göre çalışıyor, hangi yargı kaynaklarını kullanıyor>",
  "author": {
    "name": "<senin adın veya kurumun>"
  }
}
```

### 3. Skill'leri yaz
- `skills/cold-start-interview/SKILL.md` — interview soruları bu plugin için özelleştir
- En az 3-5 skill ekle (cold-start dahil)
- Her skill'in YAML frontmatter'ı eksiksiz olsun (`name`, `description`, isteğe bağlı `argument-hint`)

### 4. `CLAUDE.md` template'i ayarla
- Firma profili paylaşılan (`firma-profili.md`) — repo kökünde template, kullanıcı makinesinde `~/.claude/plugins/config/claude-for-tr-legal/firma-profili.md`
- Plugin-spesifik konular (örn. KVKK için: VERBİS kayıt, sektörel veri tipi)

### 5. `marketplace.json`'a ekle
Repo root'undaki `.claude-plugin/marketplace.json` dosyasının `plugins` array'ine yeni girdiyi ekle.

### 6. PR aç
Başlık: `feat: <plugin-adı> P<faz> pilot/initial release`
Açıklama:
- Hangi mevzuata göre yazıldı (kanun, KHK, yönetmelik numaraları)
- Hangi veri kaynaklarını kullanıyor (model eğitim verisi / kullanıcı girdisi / opsiyonel kullanıcı araçları)
- Hangi TBB Meslek Kuralları kontrolü içeriyor
- Test edilmiş skill listesi

## Yeni Skill Ekleme (Mevcut Plugin'e)

1. `<plugin>/skills/<skill-adı>/SKILL.md` oluştur
2. CLAUDE.md'deki şablonu kullan
3. `references/` altına gerekli template dosyaları ekle (örn. örnek dilekçe, yasal çerçeve özeti)
4. Plugin'in `README.md` skill listesine ekle
5. PR aç

## Kod Stili

- **Markdown** ana format — JSON/YAML metadata için
- **Türkçe** ana dil — variable/komut isimleri ASCII (filesystem)
- **Atıf zorunlu** — her madde/karar göndermesi kaynak göstersin
- **"draft for attorney review"** her skill çıktısında belirtilsin
- **Müvekkil ismi/PII yok** — örneklerde anonim ("Müvekkil A", "X Şirketi")

## Hukuki Sorumluluk

Bu repo'ya katkı yapan kişi/kuruluş:
1. Apache 2.0 lisansı altında kodunu/içeriğini paylaştığını kabul eder.
2. Müvekkil bilgisi, mahremiyet ihlali veya TBB Meslek Kurallarına aykırı içerik göndermez.
3. Yazdığı plugin/skill'in **avukat onayına hazır taslak** ürettiğini, nihai hukuki tavsiye olmadığını disclaimer'ında belirtir.

## İletişim

- **Issues:** Hata bildirimi, özellik isteği için GitHub Issues kullanın.
- **Discussions:** Mimari kararlar, plugin önerileri için Discussions.
- **E-mail:** İş birlikleri, sponsorluk için `infokapitallegal@gmail.com`.

## Code of Conduct

Üye olduğun TBB ve barolarımızın etik kuralları bu repo'da da geçerlidir. Saygılı, yapıcı, profesyonel iletişim — bunun dışına çıkan PR/Issue/yorumlar kapatılır.
