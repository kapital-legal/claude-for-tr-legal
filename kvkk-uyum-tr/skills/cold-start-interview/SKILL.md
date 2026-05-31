---
name: cold-start-interview
description: >
  kvkk-uyum-tr ilk kurulum interview'u. Önce paylaşılan firma profilini
  (~/.claude/claude-for-tr-legal/firma-profili.md) kontrol eder;
  yoksa kullanıcıdan büro bilgisini alır ve yazar; varsa onaylatır. Sonra
  plugin-spesifik KVKK profilini doldurur. "KVKK plugin'i kur", "onboard",
  "yeniden ayarla", "profili güncelle" söylemlerinde tetiklenir.
argument-hint: "[--redo: tüm interview'u yeniden çalıştır] [--firma-guncelle: yalnızca paylaşılan firma profilini güncelle]"
---

# /kvkk-uyum-tr:cold-start-interview

> Bu skill iki ayrı dosyayı yönetir:
>
> 1. **Paylaşılan firma profili** — `~/.claude/claude-for-tr-legal/firma-profili.md`
>    Tüm `claude-for-tr-legal` plugin'leri buradan okur. İlk kurulan plugin'in cold-start'ı bu dosyayı oluşturur; sonraki plugin'lerin cold-start'ı bu dosyayı okuyup onaylatır — büro bilgilerini tekrar sormaz.
>
> 2. **Plugin-spesifik KVKK profili** — `~/.claude/plugins/config/claude-for-tr-legal/kvkk-uyum-tr/CLAUDE.md`
>    Yalnızca bu plugin'in KVKK pratiğine özel sorular ve cevaplar burada.

## Adım 0 — Paylaşılan Firma Profili Kontrolü (ZORUNLU, atlanmaz)

Plugin'in plugin-spesifik sorularına geçmeden önce, **mutlaka** önce paylaşılan firma profilini kontrol et:

```
Dosya: ~/.claude/claude-for-tr-legal/firma-profili.md
```

### Senaryo A: Firma profili VAR ve doluysa

1. Dosyayı oku.
2. Kullanıcıya tek satırlık özet göster:

   > "Profilinde **[İsim] (büro tipi: [tip])** olarak görünüyorsun, **[ana yargı çevresi]**'nde, **[sektör/uzmanlık]** alanında çalışıyorsun. Doğru mu?"
   >
   > 1. Evet, doğru → KVKK sorularına geç
   > 2. Hayır, güncellemem gerek → Adım 0.1'e dön

3. Kullanıcı onaylarsa **Adım 1**'e geç (KVKK soruları).
4. `--firma-guncelle` bayrağıyla çağrıldıysa direkt Adım 0.1'e geç.

### Senaryo B: Firma profili YOK veya placeholder içeriyor

Bu kullanıcı `claude-for-tr-legal`'ın **ilk plugin'ini** kuruyor. Önce büro bilgilerini al:

#### Adım 0.1 — Şirket/Büro Soruları

Template: `${CLAUDE_PLUGIN_ROOT}/../references/firma-profili-template.md` (repo kökünden okunur)

Sırayla sor:

1. **Pratik ortamı:** Bağımsız avukat / Hukuk bürosu / Şirket içi (in-house) / Üniversite kliniği / Kamu
2. **Büro/Şirket adı:** [Tam isim]
3. **Birincil yargı çevresi:** İl + ilçe (örn. İzmir, Konak)
4. **Sektör/uzmanlık alanı:** [Serbest format — örn. fikri mülkiyet + KVKK]
5. **Ölçek:** Solo / 2-5 / 6-20 / 21-50 / 50+
6. **Sınır ötesi iş var mı?** Evet/Hayır
7. **Bağlı baro:** [Hangi baro — örn. İzmir Barosu]
8. **TBB Meslek Kuralları kontrolünü açık tutmak ister misin?** Evet (her çıktıda) / Hassas konularda / Kapalı

#### Adım 0.2 — Firma Profilini Yaz

Cevapları template'e doldurup `~/.claude/claude-for-tr-legal/firma-profili.md` konumuna yaz. Parent klasörleri oluştur. Kullanıcıya bildir:

> ✓ Firma profilini kaydettim: `~/.claude/claude-for-tr-legal/firma-profili.md`
>
> Sonraki `claude-for-tr-legal` plugin'lerini (örn. fikri-mulkiyet-tr, sirketler-hukuku-tr) kurduğunda **bu sorular tekrar sorulmayacak.** Bu dosya tüm plugin'lerin ortak referansıdır.

## Adım 1 — Plugin-Spesifik KVKK Profili

Şimdi KVKK pratiğine özel sorulara geç.

### Plugin profili kontrolü

`~/.claude/plugins/config/claude-for-tr-legal/kvkk-uyum-tr/CLAUDE.md` dosyasını oku:
- **Yok** → Adım 1.1'e geç (interview başla).
- **`<!-- SETUP PAUSED AT: -->` içeriyor** → kullanıcıyı selamla, kaldığı bölümden devam etmek isteyip istemediğini sor.
- **`[PLACEHOLDER]` içeriyor (pause işareti yok)** → template doldurulmamış; sıfırdan başla veya placeholder'lardan devam et.
- **Dolu (placeholder yok, pause yok)** → KVKK profili mevcut; `--redo` bayrağı verilmediyse onaylama yap, gerek görmezsen skill bittiğini bildir.

### Kurulum kapsam (scope) kontrolü

Çalışma dizini home directory değilse ve plugin yüklenirken kapsam belirtilmemişse:

> ⚠️ **Dikkat — plugin proje-kapsamlı yüklenmiş olabilir.** Bu durumda sadece şu dizindeki dosyaları okuyabilirim: [mevcut dizin]. Müvekkil dosyaların Downloads/Documents/iCloud'da ise QUICKSTART.md'deki yönergelerle **user scope'a** yeniden yükleyin.

Kullanıcı onaylayana kadar devam etme.

## Adım 1.1 — KVKK Interview Akışı

### Açılış (3-4 satır)

> **`kvkk-uyum-tr` KVKK programını yöneten kişiler için — aydınlatma metni, ilgili kişi başvurusu, VERBİS, ihlal triajı, risk değerlendirmesi, karar analizi.** Sen avukat mısın, KVK uzmanı mı, DPO mu?
>
> **2 dakika** = rol, hangi tarafta olduğun (sorumlu/işleyen), birincil yargı çevresi + güvenli varsayılanlar. **15 dakika** = playbook pozisyonların, risk değerlendirme şablonun, veri envanteri seedi, sektör detayı.

### Hızlı mod (2 dk) — şu soruları sor:
1. KVKK ile ilgili rolün? (avukat / DPO / KVK uzmanı / başka)
2. Veri sorumlusu mu, veri işleyen mi, ikisi de mi?
3. VERBİS kayıtlı mı? (Evet / Hayır / Bilmiyorum)
4. Özel nitelikli veri işliyor musun? (Evet / Hayır)
5. Sınır ötesi aktarım var mı? (Evet / Hayır)

### Detaylı mod (15 dk) — yukarıdakilere ek olarak:
- Mevcut aydınlatma metni varsa PDF/URL paylaş, oku.
- Mevcut risk değerlendirme şablonu varsa oku.
- Veri envanteri (varsa) oku.
- İlgili kişi başvurularını nasıl yönettiğin
- İhlal müdahale planı
- Önemli müvekkil sektörleri / hassas alanlar

## Adım 2 — Mevcut Araç Kontrolü (Opsiyonel)

Kullanıcı kendisi bir araç eklediyse availability sun. **Skill bu araçları zorunlu kılmaz** — yoksa skill'ler model + kullanıcı girdisi modunda çalışır.

| Araç tipi | Test | Sonuç |
|---|---|---|
| WebFetch (Claude built-in) | Test sorgu | ✓ varsayılan açık |
| Kullanıcının kendi seçtiği üçüncü taraf KVKK/yargı arama araçları | Listeden tara | ✓/⚪/✗ |
| Google Drive bağlayıcı (Claude.ai/Cowork) | Bağlayıcı listesinden kontrol | ✓/⚪/✗ |

**Önemli:** Sadece **gerçekten çağrılıp başarılı olan** araçları ✓ işaretle. Konfigürasyon dosyasında tanımlı olduğu için ✓ verme — kullanıcıyı yanıltır. Yapılandırılmış ama test edilmemiş = ⚪.

## Adım 3 — Plugin Profilini Yaz

1. `${CLAUDE_PLUGIN_ROOT}/CLAUDE.md` template'ini şablon olarak kullan.
2. Adım 1.1 cevaplarıyla doldur.
3. `~/.claude/plugins/config/claude-for-tr-legal/kvkk-uyum-tr/CLAUDE.md` konumuna yaz (parent klasörleri oluştur).
4. Kullanıcıya özet göster:

```
✓ KVKK profilin kaydedildi.

📁 Dosyalar:
  - ~/.claude/claude-for-tr-legal/firma-profili.md (paylaşılan)
  - ~/.claude/plugins/config/claude-for-tr-legal/kvkk-uyum-tr/CLAUDE.md (KVKK)

✓ Mevcut araçlar: WebFetch ✓ [+ varsa diğerleri]

İlk komutun:
  - /kvkk-uyum-tr:aydinlatma-metni-uretici
  - /kvkk-uyum-tr:karar-analizi
```

## Bayraklar

| Bayrak | Etki |
|---|---|
| (yok) | Standart interview. Mevcut profili tespit eder, atlanması gerekenleri atlar. |
| `--redo` | Plugin-spesifik KVKK profilini sıfırdan doldurur. Paylaşılan firma profili dokunmaz. |
| `--firma-guncelle` | Yalnızca paylaşılan firma profilini yeniden doldurur. KVKK profili dokunmaz. |

## Hata Durumları

- **`user` scope değil:** Uyar, reinstall öner.
- **Eksik bilgi:** `[PLACEHOLDER]` bırak, kullanıcı sonra `--redo` ile dönebilir.
- **Yazma hatası (disk dolu vb.):** Hata mesajını net göster, dosya yolunu belirt.

## TBB Meslek Kuralları

Bu interview'da:
- Müvekkil ismi/dosya numarası sormaz.
- Hassas finansal bilgi sormaz.
- Yazılan dosyalar **yalnızca yerel makinede** saklanır; dış servise gönderilmez.
