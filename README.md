# Claude for TR Legal

[![License: Apache 2.0](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](LICENSE)
[![Status: P1 Pilot](https://img.shields.io/badge/Status-P1_Pilot-orange.svg)](#yol-haritası)
[![Plugins](https://img.shields.io/badge/Plugins-1_aktif_/_12_planlı-blue.svg)](#mevcut-pluginler)

> Türk hukukuna uyarlanmış Claude Code plugin koleksiyonu. Anthropic'in [claude-for-legal](https://github.com/anthropics/claude-for-legal) projesinin Türkiye versiyonu.

**🌐 Repo:** https://github.com/kapital-legal/claude-for-tr-legal
**📅 İlk yayın:** 2026-05-31 (P1 Pilot)
**🏛️ Sponsor & Maintainer:** [Kapital Legal](https://www.kapitallegal.com)
**📜 Lisans:** Apache 2.0
**🤝 Topluluk katkısına açıktır.**

---

## Neden Bu Repo?

Anthropic 12 Mayıs 2026'da 12 hukuk plugin'i yayımladı — fakat tamamı ABD/UK hukukuna göre kurulu (FMLA, USPTO, DMCA, GDPR-CCPA, ABA). Türk avukatı bu plugin'leri direkt kullanamaz: SMK 6769'u, KVKK'yı, TTK'yi, UYAP'ı, TÜRKPATENT'i, TBB Meslek Kurallarını bilmeyen bir asistan Türkiye'de iş yapmaz.

Bu repo, **Anthropic'in iskeletini Türk hukukuna giydirir.** Plugin patternleri (cold-start interview, paylaşılan şirket profili, "draft for attorney review" disclaimer) korunur; içerik, terminoloji ve hukuki çerçeve Türkçeleşir.

## Mevcut Plugin'ler

| Plugin | Alan | Durum |
|---|---|---|
| `kvkk-uyum-tr` | KVKK / Veri Koruma | 🚧 **Pilot (v0.1.0)** |

## Yol Haritası

| Faz | Plugin'ler | Hedef |
|---|---|---|
| **P1 (1-2 hafta)** | `kvkk-uyum-tr`, `fikri-mulkiyet-tr` | Pilot — büro içi kullanım |
| **P2 (3-6 hafta)** | `sirketler-hukuku-tr`, `resmi-gazete-tr`, `dava-yonetimi-tr` | Günlük kullanım |
| **P3 (7-12 hafta)** | `is-hukuku-tr`, `ticari-sozlesme-tr`, `tuketici-tr`, `yz-yonetisim-tr` | 9 plugin'e ulaşma, kamuya açık launch |
| **P4 (13+ hafta)** | `tr-hukuk-hub`, `hukuk-ogrencisi-tr`, `hukuk-klinigi-tr` | Topluluk + üniversite işbirliği |

## Hızlı Başlangıç

> ⚠️ **Önkoşul:** Claude Code veya Claude Cowork erişimi (Pro/Max abonelik).

### 1. Marketplace'i ekle

Claude Code'da:

```
/plugin marketplace add kapital-legal/claude-for-tr-legal
```

### 2. Plugin yükle

```
/plugin install kvkk-uyum-tr@claude-for-tr-legal
```

> Kapsam (scope) sorulursa **`user`** seç — proje dışındaki dosyalara da erişim için gerekli.

### 3. Claude Code'u yeniden başlat
Plugin yüklendikten sonra kapatıp açmadan komutlar görünmez.

### 4. Cold-start interview çalıştır
```
/kvkk-uyum-tr:cold-start-interview
```

Bu, sizin bürodaki KVKK pratiğini öğrenir ve `~/.claude/plugins/config/claude-for-tr-legal/kvkk-uyum-tr/CLAUDE.md` dosyasına yazar. Diğer tüm skill'ler buradan okur.

### 5. Bir görev çalıştır
```
/kvkk-uyum-tr:aydinlatma-metni-uretici
/kvkk-uyum-tr:ilgili-kisi-basvurusu
/kvkk-uyum-tr:karar-analizi
```

## Önemli Yasal Uyarı

> ⚠️ **Tüm çıktılar avukat tarafından gözden geçirilmesi gereken taslaklardır. Hukuki tavsiye değildir.**
>
> - Skill'ler "avukat onayına hazır taslak" üretir
> - Karar/madde atıfları, model eğitim verisinden geldiklerinde `[doğrulanmalı]` etiketi taşır — kullanıcı resmi kaynaktan (kvkk.gov.tr, Resmi Gazete, vb.) **mutlaka doğrulamalıdır**
> - Belirsiz/öznel hukuki kararlar konservatif varsayılan ile sunulur
> - Hiçbir plugin Kapital Legal'in veya başka bir kurumun hukuki pozisyonunu temsil etmez

## Karar/Mevzuat Veri Kaynakları

Bu repo **kendi başına resmi karar veritabanı entegrasyonu içermez**. Plugin'ler:

- **Model eğitim verisinden** genel KVKK çerçevesi ve örnek kararları sunar (her zaman `[doğrulanmalı]` etiketli)
- **Kullanıcının manuel sağladığı** karar URL/metinlerini analiz eder (örn. kvkk.gov.tr'den kopyaladığınız bir karar metni)
- İsteğe bağlı olarak **kullanıcının kendi makinesinde kurduğu** üçüncü taraf araçlarla (örn. Anthropic'in resmi Marka-Patent connector'ı, kullanıcının kendi seçtiği KVKK arama araçları) çalışabilir — bu araçlar bu repo'nun parçası **değildir**, kullanıcı sorumluluğundadır

**Resmi karar/mevzuat kaynakları:**
- KVKK Kurul kararları: https://www.kvkk.gov.tr
- TÜRKPATENT marka/patent: https://www.turkpatent.gov.tr
- Resmi Gazete: https://www.resmigazete.gov.tr
- UYAP Emsal Karar: https://emsal.uyap.gov.tr

## Bağımlılıklar

- **Claude Code** veya Claude Cowork (Pro/Max abonelik) — **tek zorunluluk**
- (Opsiyonel) Anthropic ekosisteminin sunduğu Marka-Patent / Google Drive / Gmail connector'ları, kullanıcı isterse Claude Cowork üzerinden ayrı olarak bağlanabilir

Bu repo, herhangi bir üçüncü taraf MCP sunucusunu zorunlu kılmaz veya gömmez.

## Katkı

PR'lara açığız. Detay: [KATKI.md](KATKI.md).

Önceliklerimiz:
1. P1/P2 plugin'lerinin tamamlanması (KVKK + Marka/Patent + Şirketler)
2. TBB Meslek Kuralları uyum kontrolünü her plugin'e hook olarak yerleştirmek
3. Türkçe karakter/yargı terminolojisi hataları (örn. "ilgili kişi başvurusu" terimini doğru kullanmak)
4. Anthropic claude-for-legal güncellemelerini iskelet düzeyinde takip etmek

## TBB / Baro Meslek Kuralları

Bu repo bir hukuk bürosu sponsorluğunda yayımlanmıştır. TBB Meslek Kuralları ve barolarımızın reklam/tabela düzenlemeleri çerçevesinde:

- Repo, müvekkil işi veya hukuki tavsiye sunmaz — yalnızca avukatların kullanabileceği bir araç koleksiyonudur.
- Sponsor olan Kapital Legal'in tanıtım amacıyla kurulmadığı, ortak meslek altyapısına katkı amacıyla yayımlandığı açıkça belirtilmiştir.
- Yine de **launch öncesi ve sonrasında bağlı baronuza danışılması önerilir.** Reklam/tabela kuralları zaman içinde yorumlanmakta ve revize edilmektedir.

## Anthropic ile İlişki

Bu repo **Anthropic'in resmi ürünü değildir.** Topluluk projesidir; Apache 2.0 lisansı altında Anthropic'in [claude-for-legal](https://github.com/anthropics/claude-for-legal) yapısından esinlenmiştir.

Plugin'ler olgunlaştığında Anthropic'in `external_plugins/` klasörüne PR olarak sunulması hedeflenir.

---

## Geliştirme Notları (Maintainer İçin)

Kapital Legal ekibi veya katkıda bulunmak isteyenler için lokal geliştirme:

```bash
git clone https://github.com/kapital-legal/claude-for-tr-legal.git
cd claude-for-tr-legal
# Claude Code'da yerel marketplace eklemek için:
/plugin marketplace add ./
# veya tam path ile (klasörü Claude Code terminaline sürükle-bırak):
/plugin marketplace add /tam/yol/claude-for-tr-legal
```

Bu **yalnızca geliştirme** içindir — son kullanıcılar 1. adımdaki GitHub kısayolunu kullanmalıdır.

---

## English Summary

**Claude for TR Legal** is a community port of [Anthropic's claude-for-legal](https://github.com/anthropics/claude-for-legal) for Turkish law practice. While Anthropic's plugins target US/UK law (FMLA, USPTO, GDPR/CCPA), this repo adapts the same patterns to Turkish regulations: **KVKK** (privacy), **SMK 6769** (trademarks/patents), **TTK** (commercial code), **TBB** professional rules.

The repo preserves Anthropic's architectural patterns (cold-start interview, shared company profile, "draft for attorney review" disclaimers) while translating terminology and Turkish legal framing. **This repo does not bundle or require any third-party MCP servers.** Users may optionally connect their own tools (such as Anthropic's official Trademark/Patent connector via Claude Cowork) at their own discretion. Apache 2.0 licensed. Maintained by Kapital Legal (İzmir).

**Current status:** Pilot — `kvkk-uyum-tr` (KVKK compliance) v0.1.0. See roadmap above.
