# Claude for TR Legal — Geliştirici Notları

Bu dosya, repo üzerinde çalışan geliştiriciler (insan veya AI) için yönlendirme verir. Plugin kullanıcıları için ana doküman [README.md](README.md)'dir.

## Repo Felsefesi

1. **Anthropic'in iskeletini koru, içeriği Türkçeleştir.** Plugin yapısı (`.claude-plugin/plugin.json` + `skills/*/SKILL.md` + cold-start interview deseni) Anthropic ile birebir aynı kalır. Sadece hukuki içerik, terminoloji ve veri kaynakları Türkçeleşir.

2. **"Draft for attorney review" prensibi.** Hiçbir plugin nihai hukuki tavsiye üretmez. Her çıktı taslak halinde, kaynak gösterilerek, avukat onayına hazır şekilde sunulur. Belirsiz konularda konservatif varsayılan kullanılır.

3. **Veri kaynakları açık ve doğrulanabilir.** Her karar/madde atıfı kaynağıyla birlikte verilir. yargi-mcp gibi resmi devlet API'leri tercih edilir. Karar metinlerinin URL'i her zaman dahil edilmelidir.

4. **Paylaşılan şirket profili.** Anthropic'in `company-profile.md` deseni korunur: ilk plugin'i kuran kullanıcı şirket bilgilerini girer, sonraki plugin'ler okur. Bu Kapital Legal gibi çok partner'lı bürolar için kritik — Sinem KVKK plugin'i kurar, Muhittin marka plugin'i kurarken aynı büro bilgisi otomatik geçer.

5. **TBB Meslek Kuralları gözetilir.** Her plugin'in `references/` klasöründe ilgili TBB kurallarına atıf bulunur. Müvekkil mahremiyeti, çıkar çatışması, avukatlık tekeli prensipleri ihlal eden hiçbir çıktı üretilmez.

## Klasör Konvansiyonları

```
<plugin-adı>-tr/
  .claude-plugin/plugin.json     ← {name, version, description, author}
  CLAUDE.md                       ← practice profile template (cold-start doldurur)
  README.md                       ← plugin tanıtım + skill listesi
  skills/
    cold-start-interview/SKILL.md
    <skill-adı>/SKILL.md
  references/                     ← templateler, mevzuat özetleri, emsal kararlar
  hooks/                          ← (opsiyonel) lifecycle hooks
  agents/                         ← (opsiyonel) sub-agent tanımları
```

**İsimlendirme:**
- Plugin adları: küçük harf, tire ile ayrılmış, `-tr` soneki
- Skill adları: küçük harf, tire ile ayrılmış, Türkçe fiil tabanı tercih (örn. `aydinlatma-metni-uretici`, `emsal-karar-arayici`)
- Türkçe karakter (ı, ş, ğ, ü, ç, ö) **plugin/skill adlarında kullanılmaz** (filesystem uyumu) ama YAML/markdown içeriğinde elbette kullanılır

## SKILL.md Şablonu

```markdown
---
name: skill-adı
description: >
  Bu skill ne yapar (1-2 cümle). Hangi kullanıcı söyleminde tetiklenir.
  Örn: "ilgili kişi başvurusu", "DSAR yanıtla", "30 günlük süreyi takip et"
argument-hint: "[--bayrak açıklaması]"
---

# /<plugin>:<skill>

## Cold-start kontrolü
`~/.claude/plugins/config/claude-for-tr-legal/<plugin>/CLAUDE.md` dosyasını oku:
- Yoksa / placeholder içeriyorsa → kullanıcıyı cold-start'a yönlendir.

## İş akışı
1. Adım 1...
2. Adım 2...

## Çıktı formatı
- Avukat onayına hazır taslak
- Kaynak gösterimi: [SMK m.X], [Yargıtay 11.HD, X.X.2026 E.YYYY/N K.YYYY/N]
- Belirsizlik durumunda konservatif varsayılan + [verify] etiketi

## TBB Meslek Kuralları uyumu
İlgili kural numaraları + tetikleyici kontroller
```

## yargi-mcp Entegrasyonu

Her veri çekme skill'i `yargi-mcp` araçlarını kullanmalıdır:

| İşlem | yargi-mcp Aracı |
|---|---|
| KVKK kararı arama | `mcp__yargi-mcp__search_kvkk_decisions` |
| Yargıtay/Danıştay (Bedesten ayağa kalktığında) | `mcp__yargi-mcp__search_bedesten_unified` |
| BAM/Yerel mahkeme kararı | `mcp__yargi-mcp__search_emsal_detailed_decisions` |
| Anayasa Mahkemesi | `mcp__yargi-mcp__search_anayasa_unified` |
| Tam metin çekme | `mcp__yargi-mcp__get_<source>_document_markdown` |

**Kullanıcının yargi-mcp kurmuş olduğu varsayılır.** Skill başında availability check yapılır; yoksa kullanıcı README'ye yönlendirilir.

## Geliştirici Komutları

```bash
# Yeni plugin iskeleti oluştur (şablon kopyalama)
cp -r kvkk-uyum-tr/ <yeni-plugin>-tr/

# Skill ekle
mkdir -p <plugin>/skills/<skill-adı>
touch <plugin>/skills/<skill-adı>/SKILL.md

# Lokal test
# Claude Code'da: /plugin marketplace add <bu-repo-path>
```

## Versiyonlama

Her plugin'in `.claude-plugin/plugin.json` dosyasında semver kullanılır:
- **0.x.y** — geliştirme aşaması (P1 plugin'leri burada)
- **1.0.0** — büroda günlük kullanımda stabil
- **2.0.0** — kamu/topluluk launch

## Memory ve Çoklu Plugin Koordinasyonu

Kapital Legal'in memory sistemi (`~/.claude/projects/.../memory/`) bu repo'nun gelişiminden bağımsızdır ama bilgi paylaşır:
- `project_tr_hukuk_plugin_hub.md` — repo state, açık görevler, faz takvimi
- Her plugin tamamlandığında MEMORY.md index'ine eklenir
