---
name: cold-start-interview
description: >
  kvkk-uyum-tr ilk kurulum interview'u. Büronun/şirketin KVKK pratiğini öğrenir,
  CLAUDE.md profilini yazar. "KVKK plugin'i kur", "onboard et", "yeniden ayarla",
  "profili güncelle" söylemlerinde tetiklenir. Diğer tüm skill'ler bu profili okur.
argument-hint: "[--redo: yeniden çalıştır] [--update-profile: paylaşılan büro profilini güncelle]"
---

# /kvkk-uyum-tr:cold-start-interview

## Cold-start kontrolü

`~/.claude/plugins/config/claude-for-tr-legal/kvkk-uyum-tr/CLAUDE.md` dosyasını oku:
- **Yok** → interview'a başla.
- **`<!-- SETUP PAUSED AT: -->` içeriyor** → kullanıcıyı selamla, kaldığı bölümden devam etmek isteyip istemediğini sor.
- **`[PLACEHOLDER]` içeriyor (pause işareti yok)** → template doldurulmamış; sıfırdan başla veya placeholder'lardan devam et.
- **Dolu (placeholder yok, pause yok)** → kullanıcı profili mevcut; `--redo` bayrağı verilmediyse onaylama yap.

## Paylaşılan büro profili kontrolü

`~/.claude/plugins/config/claude-for-tr-legal/company-profile.md` dosyasına bak:
- **Varsa:** Oku, tek satırlık özet göster ("Sen [isim], [büro] avukatı, [yargı çevresi]'nde çalışıyorsun. Doğru mu? Değişiklik için 'güncelle' de.") — onaylanırsa şirket soruları atla.
- **Yoksa:** Bu kullanıcı, claude-for-tr-legal'in ilk plugin'ini kuruyor. Orientation + şirket soruları + plugin-spesifik sorular sırayla atılır. `references/company-profile-template.md` şablonu kullanılır.

## Kurulum kapsam (scope) kontrolü

Çalışma dizini home directory değilse ve plugin yüklenirken kapsam belirtilmemişse:
> **Dikkat — plugin proje-kapsamlı yüklenmiş olabilir.** Bu durumda sadece şu dizindeki dosyaları okuyabilirim: [mevcut dizin]. Müvekkil dosyaların Downloads/Documents/iCloud'da ise QUICKSTART.md'deki yönergelerle **user scope'a** yeniden yükleyin.

Kullanıcı onaylayana kadar devam etme.

## Interview Akışı

### Açılış (3-4 satır)
> **`kvkk-uyum-tr` KVKK programını yöneten kişiler için — aydınlatma metni, ilgili kişi başvurusu, VERBİS, ihlal triajı, risk değerlendirmesi.** Sen avukat mısın, KVK uzmanı mı, DPO mu? Farklı bir rol arıyorsan: `/legal-builder-hub:related-skills-surfacer` (gelecekte).
>
> **2 dakika** = rol, hangi tarafta olduğun (sorumlu/işleyen), birincil yargı çevresi + güvenli varsayılanlar. **15 dakika** = playbook poziisyonların, risk değerlendirme şablonun, veri envanteri seedi, sektör detayı.

### Hızlı modu (2 dk) — şu soruları sor:
1. Rolün?
2. Veri sorumlusu mu, veri işleyen mi, ikisi de mi?
3. Birincil sektör?
4. VERBİS kayıtlı mı? (Evet/Hayır/Bilmiyorum)
5. Özel nitelikli veri işliyor musun? (Evet/Hayır)
6. Sınır ötesi aktarım var mı? (Evet/Hayır)

### Detaylı mod (15 dk) — yukarıdakilere ek olarak:
- Mevcut aydınlatma metni varsa PDF/URL paylaş, oku.
- Mevcut risk değerlendirme şablonu varsa oku.
- Veri envanteri (varsa) oku.
- İlgili kişi başvurularını nasıl yönettiğin
- İhlal müdahale planı
- Önemli müvekkil sektörleri / hassas alanlar
- TBB Meslek Kuralları kontrolünü açık tutmak ister mi?

## Entegrasyon Kontrolü (Opsiyonel)

Kullanıcı kendisi bir araç eklediyse availability sun. **Skill bu araçları zorunlu kılmaz** — yoksa skill'ler model + kullanıcı girdisi modunda çalışır.

| Araç tipi | Test | Sonuç |
|---|---|---|
| WebFetch (URL okuma — Claude'un built-in) | Test sorgu | ✓ varsayılan |
| Google Drive bağlayıcı (Claude Cowork) | Bağlayıcı listesinden kontrol | ✓/⚪/✗ |
| Kullanıcının kurduğu Türk hukuk MCP araçları | Listeden tara | ✓/⚪/✗ |

**Önemli:** Sadece **gerçekten çağrılıp başarılı olan** araçları ✓ işaretle. Konfigürasyon dosyasında tanımlı olduğu için ✓ verme — kullanıcıyı yanıltır. Yapılandırılmış ama test edilmemiş = ⚪.

## CLAUDE.md Yazımı

1. `${CLAUDE_PLUGIN_ROOT}/CLAUDE.md` template'ini şablon olarak kullan.
2. Interview cevaplarıyla doldur.
3. `~/.claude/plugins/config/claude-for-tr-legal/kvkk-uyum-tr/CLAUDE.md` konumuna yaz (parent klasörleri oluştur).
4. Eski cache path'ten (varsa) migration: `~/.claude/plugins/cache/claude-for-tr-legal/kvkk-uyum-tr/*/CLAUDE.md` → config path'e kopyala.
5. Kullanıcıya özet göster:
   ```
   ✓ KVKK profilin kaydedildi: <yol>
   ✓ Mevcut araçlar: WebFetch ✓ | (varsa diğer)

   İlk komutun:
   - /kvkk-uyum-tr:aydinlatma-metni-uretici
   - /kvkk-uyum-tr:karar-analizi
   ```

## Paylaşılan Şirket Profili Yazımı

Eğer `company-profile.md` yoksa:
1. Şirket sorularını sor (referans şablon: `references/company-profile-template.md`)
2. `~/.claude/plugins/config/claude-for-tr-legal/company-profile.md` konumuna yaz
3. Kullanıcıya bildir: "Şirket profilini kaydettim — diğer plugin'ler (kuracağın zaman) bu soruları sormayacak."

## Hata Durumları

- **Kullanıcı bir araç kurmadıysa:** Hata değil — skill'ler 1+2 modunda (model + kullanıcı girdisi) çalışır.
- **`user` scope değil:** Uyar, reinstall öner.
- **Eksik bilgi:** `[PLACEHOLDER]` bırak, kullanıcı sonra `--redo` ile dönebilir.

## TBB Meslek Kuralları

Bu interview'da:
- Müvekkil ismi/dosya numarası sormaz.
- Hassas finansal bilgi sormaz.
- Kapital Legal sponsoru olmasının dışında 3. partiye veri göndermez.
