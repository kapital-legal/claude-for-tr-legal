# Veri İşleme Bildirimi — Anthropic API'ye İletim ve TBB m.37 Çerçevesi

> Bu dosya, plugin kullanıcılarının ve maintainer'ların **doğru anlaması gereken** teknik gerçekliği açıklar. Avukatlık meslek sırrı (TBB Meslek Kuralları m.37) açısından kritiktir.

## Teknik Gerçeklik

**Claude Code yerel çıkarım yapmaz.** Bir skill çağrıldığında:

1. Kullanıcının verdiği girdi (yapıştırılan metin, açılan dosya içeriği, profil bilgisi)
2. Skill'in talimatları (SKILL.md prompt)
3. Plugin'in profil dosyalarından okuduğu içerik (`firma-profili.md`, plugin-spesifik `CLAUDE.md`)

bütün bunlar **Anthropic'in API'sine iletilir** ve Claude tarafından oradan işlenir. Veri yerel makinede tutulan dosyalardan **okunur** ve API'ye **gönderilir** — modelin "lokal çıkarım yaptığı" söylemi teknik olarak yanlıştır.

## Bu Plugin'in Yaptığı vs Yapmadığı

### ✅ Plugin **YAPAR**
- Anthropic API'sine veri iletir (kaçınılmaz — model orada çalışır)
- Profil dosyalarını yerel diskinden okur ve API'ye gönderir
- Kullanıcının verdiği karar metni/URL içeriğini API'ye iletir

### ❌ Plugin **YAPMAZ**
- **Anthropic dışındaki üçüncü taraf servislere** veri göndermez (örn. AWS, Google, başka şirketler — bu repo hiçbirine entegre değildir)
- Kullanıcının açık talebi olmadan dosya yazmaz
- Kullanıcıdan gelen bilgileri kendi maintainer'ı Kapital Legal'e aktarmaz
- Anthropic'in API çağrısı dışında ağ trafiği oluşturmaz

## TBB Meslek Kuralları m.37 (Avukat Sır Saklama Yükümlülüğü)

TBB Meslek Kuralları m.37 uyarınca avukat, müvekkil sırlarını saklamakla yükümlüdür. Aynı yükümlülük 1136 sayılı Avukatlık Kanunu m.36'da (sır saklama yasağı) ayrıca düzenlenir; iki düzenleme birlikte uygulanır.

**Bu plugin'in TBB m.37 ile ilişkisi:**

- **Anthropic'e veri iletimi, üçüncü taraf aktarımıdır.** Plugin bu aktarımı kaçınılmaz olarak yapar (model orada çalışıyor).
- Bu aktarımın TBB m.37 ve Av.K. m.36 çerçevesinde "rıza" veya "kanuni izin" gerektirip gerektirmediği **avukatın değerlendirmesidir.** Anthropic'in Veri İşleme Sözleşmesi (DPA) ve gizlilik politikaları kullanıcının kendisi tarafından okunmalı, müvekkille olan sözleşmesel veri işleme şartları gözden geçirilmelidir.
- Plugin **müvekkil ismi/PII içermeyen, anonimleştirilmiş** girdiyle çalışacak şekilde tasarlanmıştır. Skill'ler bu yönde uyarı verir; ancak nihai sorumluluk avukattadır.

## Avukat İçin Pratik Kılavuz

Bu plugin'i müvekkil dosyası içeren bir senaryoda kullanmadan önce:

1. **Anthropic'in Veri İşleme Sözleşmesi**'ni gözden geçir: https://www.anthropic.com/legal
2. **Müvekkille olan sözleşmenin** veri işleme/üçüncü taraf hükümlerini kontrol et.
3. **Mümkünse anonim/temsili veriyle çalış** — gerçek müvekkil ismi/PII yerine "Müvekkil [A]", "Şirket [X]" gibi temsiller.
4. Şüphe durumunda **bağlı baronuza danış** (`references/baro-danisma-notu.md`).

## Skill Yazarları İçin Kural

Skill'lerde **aşağıdaki yanlış güvenceler ASLA verilmez:**

- ❌ "Yalnızca lokal işlenir"
- ❌ "Dış servise gönderilmez"
- ❌ "Yalnızca yerel makinede saklanır"
- ❌ "Veri dışarı çıkmaz"

Bunların yerine **doğru çerçeveleme:**

- ✅ "Veriler bu plugin tarafından üçüncü taraf servislere (Anthropic dışı) aktarılmaz; ancak Claude'un işleyebilmesi için Anthropic'in API'sine iletilir. TBB m.37 / Av.K. m.36 çerçevesinde avukat bu aktarımı kendi değerlendirir."

veya kısa hali:

- ✅ "Anthropic API'sine iletilir; üçüncü taraf servise aktarılmaz. Detay: `references/veri-isleme-bildirimi.md`"

## Güncelleme

Anthropic'in veri işleme politikaları zaman içinde değişebilir. Bu dosya en az 6 ayda bir gözden geçirilmelidir. Son güncelleme: 2026-05-31.
