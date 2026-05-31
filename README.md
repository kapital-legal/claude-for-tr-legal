# Claude for TR Legal

> Türk hukukuna uyarlanmış Claude Code plugin koleksiyonu. Anthropic'in [claude-for-legal](https://github.com/anthropics/claude-for-legal) projesinin Türkiye versiyonu.

**Sponsor & Maintainer:** [Kapital Legal](https://www.kapitallegal.com)
**Lisans:** Apache 2.0
**Topluluk katkısına açıktır.**

---

## Neden Bu Repo?

Anthropic 12 Mayıs 2026'da 12 hukuk plugin'i yayımladı — fakat tamamı ABD/UK hukukuna göre kurulu (FMLA, USPTO, DMCA, GDPR-CCPA, ABA). Türk avukatı bu plugin'leri direkt kullanamaz: SMK 6769'u, KVKK'yı, TTK'yi, UYAP'ı, TÜRKPATENT'i, TBB Meslek Kurallarını bilmeyen bir asistan Türkiye'de iş yapmaz.

Bu repo, **Anthropic'in iskeletini Türk hukukuna giydirir.** Plugin patternleri (cold-start interview, paylaşılan şirket profili, "draft for attorney review" disclaimer) korunur; içerik, terminoloji ve veri kaynakları (yargi-mcp, Resmi Gazete, TÜRKPATENT, UYAP) Türkçeleşir.

## Mevcut Plugin'ler

| Plugin | Alan | Durum | Veri Kaynağı |
|---|---|---|---|
| `kvkk-uyum-tr` | KVKK / Veri Koruma | 🚧 **Pilot** | yargi-mcp KVKK |

## Yol Haritası

| Faz | Plugin'ler | Hedef |
|---|---|---|
| **P1 (1-2 hafta)** | `kvkk-uyum-tr`, `fikri-mulkiyet-tr` | Pilot — Kapital Legal/Patent içi kullanım |
| **P2 (3-6 hafta)** | `sirketler-hukuku-tr`, `resmi-gazete-tr`, `dava-yonetimi-tr` | Büro içi günlük kullanım |
| **P3 (7-12 hafta)** | `is-hukuku-tr`, `ticari-sozlesme-tr`, `tuketici-tr`, `yz-yonetisim-tr` | 9 plugin'e ulaşma, kamuya açık launch |
| **P4 (13+ hafta)** | `tr-hukuk-hub`, `hukuk-ogrencisi-tr`, `hukuk-klinigi-tr` | Topluluk + üniversite işbirliği |

## Hızlı Başlangıç

### 1. Marketplace'i ekle (yerel)

```bash
# Claude Code'da:
/plugin marketplace add /Users/av.haruneren/Desktop/Agent-dosyaları/claude-for-tr-legal
```

### 2. Plugin yükle

```bash
/plugin install kvkk-uyum-tr@claude-for-tr-legal
```

### 3. Claude Code'u yeniden başlat
Plugin yüklendikten sonra kapatıp açmadan komutlar görünmez.

### 4. Cold-start interview çalıştır
```bash
/kvkk-uyum-tr:cold-start-interview
```

Bu, sizin bürodaki KVKK pratiğini öğrenir ve `~/.claude/plugins/config/claude-for-tr-legal/kvkk-uyum-tr/CLAUDE.md` dosyasına yazar. Diğer tüm skill'ler buradan okur.

### 5. Bir görev çalıştır
```bash
/kvkk-uyum-tr:aydinlatma-metni-uretici
/kvkk-uyum-tr:ilgili-kisi-basvurusu
/kvkk-uyum-tr:emsal-karar-arayici
```

## Önemli Yasal Uyarı

> ⚠️ **Tüm çıktılar avukat tarafından gözden geçirilmesi gereken taslaklardır. Hukuki tavsiye değildir.**
>
> - Skill'ler "avukat onayına hazır taslak" üretir
> - Karar/madde atıfları kaynak gösterilerek verilir
> - Belirsiz/öznel hukuki kararlar konservatif varsayılan ile sunulur
> - Hiçbir plugin Kapital Legal'in hukuki pozisyonunu temsil etmez

## Bağımlılıklar

- **Claude Code** veya Claude Cowork (Pro/Max abonelik)
- **yargi-mcp** (önerilen) — gerçek zamanlı Türk yargı veritabanı erişimi: https://github.com/saidsurucu/yargi-mcp
- Python 3.11+ (yargi-mcp için)

## Katkı

PR'lara açığız. Detay: [KATKI.md](KATKI.md).

Önceliklerimiz:
1. P1/P2 plugin'lerinin tamamlanması (KVKK + Marka/Patent + Şirketler)
2. yargi-mcp entegrasyon iyileştirmesi
3. TBB Meslek Kuralları uyum kontrolü
4. UDF dilekçe üretimi (udf-cli entegrasyonu)

## Anthropic ile İlişki

Bu repo **Anthropic'in resmi ürünü değildir.** Topluluk projesidir; Apache 2.0 lisansı altında Anthropic'in [claude-for-legal](https://github.com/anthropics/claude-for-legal) yapısından esinlenmiştir.

Plugin'ler olgunlaştığında Anthropic'in `external_plugins/` klasörüne PR olarak sunulması hedeflenir.

---

## English Summary

**Claude for TR Legal** is a community port of [Anthropic's claude-for-legal](https://github.com/anthropics/claude-for-legal) for Turkish law practice. While Anthropic's plugins target US/UK law (FMLA, USPTO, GDPR/CCPA), this repo adapts the same patterns to Turkish regulations: **KVKK** (privacy), **SMK 6769** (trademarks/patents), **TTK** (commercial code), **UYAP** (court system), **TÜRKPATENT** registry, **TBB** professional rules.

The repo preserves Anthropic's architectural patterns (cold-start interview, shared company profile, "draft for attorney review" disclaimers) while translating terminology and connecting to Turkish data sources via `yargi-mcp`. Apache 2.0 licensed. Maintained by Kapital Legal (İzmir).

**Current status:** Pilot phase — `kvkk-uyum-tr` (KVKK compliance) plugin in active development. See roadmap above for upcoming plugins.
