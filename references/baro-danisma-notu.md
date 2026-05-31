# Baroya Danışma Notu — Repo Sahipleri ve Sponsorlar İçin

> Bu dosya, hukuk bürosu sponsorluğunda yayımlanan açık kaynak araçlar için baro/TBB perspektifinden dikkat edilmesi gereken hususları derler. **Hukuki tavsiye değildir.** Repo yayını öncesi ve sonrasında bağlı baronuza özel olarak danışmanız önerilir.

## Neden Bu Not Var?

Türkiye Barolar Birliği Meslek Kuralları ve barolarımızın reklam/tabela düzenlemeleri, avukatların ve hukuk bürolarının kamuya yönelik faaliyetlerini sıkı şekilde düzenler. Bir hukuk bürosunun (Kapital Legal gibi) **açık kaynak yazılım yayımlaması, GitHub üzerinde maintainership üstlenmesi ve LinkedIn'de duyurması** klasik reklam tanımına girmese de yorumlamaya açık alanlar barındırır.

Bu repo aşağıdaki tasarım kararlarıyla bu riski **azaltmaya** çalışmıştır, fakat tamamen ortadan kaldırmaz:

## Repo'nun Tasarım Kararları

1. **Sponsor şeffaflığı:** Repo açıkça Kapital Legal sponsorluğunda olduğunu belirtir; gizlemez.
2. **Reklam yasağı:** README ve plugin açıklamaları, Kapital Legal'in hukuki hizmetlerini pazarlamaz. "Falanca konuda Türkiye'nin en iyi hukuk bürosu" tarzı ifadeler yoktur.
3. **Hizmet sunumu yok:** Plugin'ler müvekkil işi yapmaz, hukuki tavsiye üretmez. Yalnızca **avukatlar için** araç koleksiyonudur.
4. **Disclaimer her yerde:** "draft for attorney review" + "hukuki tavsiye değildir" ifadeleri tutarlı şekilde tüm dokümanlarda.
5. **TBB Meslek Kuralları hook'u:** Skill çıktıları meslek kuralları çerçevesinde kontrol edilir (gelecek hook).
6. **Müvekkil dosyası yok:** Repo'da, örneklerde veya issue'larda gerçek müvekkil bilgisi olmaz.
7. **Lisans:** Apache 2.0 — kar amacı gütmeden topluluğa açık.

## Riskli Alanlar (Tartışmaya Açık)

### A) Repo'nun varlığı reklam mı?
- **Argüman var:** Repo açıkça Kapital Legal sponsorluğunda. Müvekkil çekme amacı taşıyabilir.
- **Argüman yok:** Repo bir araç; hukuki hizmet sunmuyor. Apache 2.0 lisanslı; herkes ücretsiz kullanabilir. Sponsorluk Kapital Legal'in fikri katkısının kabulüdür.
- **Tavsiye:** Repo URL'sini birebir Kapital Legal web sitesinin "hizmetler" sayfasında satış amaçlı listeleme. Yalnızca "katkıda bulunduğumuz projeler" gibi nötr bir başlık altında at.

### B) LinkedIn'de duyurmak reklam mı?
- **Argüman var:** Sponsor olarak repo'yu duyurmak Kapital Legal'in görünürlüğünü artırır.
- **Argüman yok:** Avukatın bilimsel/mesleki içerik üretmesi reklam yasağına genellikle girmez. (Birincil kaynak: TBB Reklam Yasağı Yönetmeliği; TBB Meslek Kuralları m.7/8/12 [doğrulanmalı] paralel düzenleme — kullanmadan önce baroya/güncel Yönetmeliğe danışın.)
- **Tavsiye:** Duyuru postunda "hizmet alın" tarzı CTA olmasın. Sadece "katkı ekosistemi" çerçevesinde sun.

### C) Diğer baroların reklam kuralları
- Her baronun reklam düzenlemeleri farklı olabilir. İzmir Barosu, İstanbul Barosu, Ankara Barosu reklam kuralları belli açılardan farklılaşır.
- Repo maintainer ve sponsor avukatları **bağlı oldukları baroya** danışmalıdır.

### D) Müvekkil bilgisi paylaşımı
- Issue'larda, PR'larda veya örneklerde **gerçek müvekkil bilgisi paylaşılmamalıdır.**
- Anonim örnekler ("Şirket [X]", "Müvekkil [A]") kullanılır.
- Repo maintainer'ları PR/Issue'ları gözden geçirirken bu riski takip etmelidir.

## Önerilen Danışma Soruları

Bağlı baronuza şu spesifik soruları sormanız önerilir:

1. **"Sponsorluğunda kamuya açık bir yazılım yayımlamak reklam kapsamına girer mi?"**
2. **"Bu yazılımı LinkedIn'de duyurmak meslek kurallarına aykırı mı?"**
3. **"Müvekkil verisi içermeyen, anonim örneklerle kurulan bir araç, müvekkil sırrı kuralı (TBB Meslek Kuralları m.37 + 1136 sayılı Avukatlık Kanunu m.36) açısından sakıncalı mı?"**
4. **"TBB Meslek Kuralları çerçevesinde 'hukuki tavsiye değildir' disclaimer'ı yeterli mi?"**
5. **"Açık kaynak topluluk katkılarını kabul etmek (PR'lar, anonim katkılar) hangi yükümlülükleri doğurur?"**

## Kapital Legal Özel Notlar (Maintainer için)

İzmir Barosu'nun reklam ve tanıtım yönergesi göz önüne alınarak:
- Kapital Legal web sitesinde repo, "Açık Kaynak Katkılarımız" gibi nötr bir başlık altında listelenebilir
- Müvekkil davalarına atıf yapılmaz
- "Türkiye'nin ilk Türk hukuk plugin'i" gibi pazarlama ifadeleri kullanılmaz (gerçek bile olsa)
- LinkedIn duyurusunda "biz çözdük" yerine "ortak ekosistem altyapısı" dili tercih edilir

## Güncelleme

TBB Meslek Kuralları, baro reklam yönergeleri ve KVKK avukatlık verisi uygulamaları zaman içinde değişir. Bu dosya:

- Her launch öncesi gözden geçirilir
- Baro yazılı görüşü alındığında güncellenir
- Topluluk geri bildirimleriyle iyileştirilir

Son güncelleme: 2026-05-31
