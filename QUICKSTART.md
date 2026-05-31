# Hızlı Başlangıç

**Hedef:** 5 dakikada bir KVKK skill'i çalıştırıyor olmak.

## Ön Koşullar

- **Claude Code veya Claude Cowork** — Pro/Max abonelik gerekir.

Başka hiçbir üçüncü taraf araç zorunlu değildir. Plugin tüm temel işlevleri model + kullanıcının sağladığı içerikle yürütür.

## Kurulum

### 1. Marketplace'i ekle

Claude Code terminalinde:
```
/plugin marketplace add kapital-legal/claude-for-tr-legal
```

> 💡 Yerel geliştirme yapanlar README'nin "Geliştirme Notları" bölümüne baksın.

### 2. Plugin'i yükle

```
/plugin install kvkk-uyum-tr@claude-for-tr-legal
```

Kurulum sırasında **kapsam (scope)** sorulursa **"user"** seç. Detay aşağıda.

### 3. Claude Code'u yeniden başlat

Bu adım zorunlu — plugin yeniden başlatma olmadan görünmez.

### 4. Cold-start interview çalıştır

```
/kvkk-uyum-tr:cold-start-interview
```

5-15 dakika sürer. Sana:
- Hangi rolde olduğun (büro avukatı, in-house, KVK uzmanı)
- Veri sorumlusu mu veri işleyen mi (genelde her ikisi de)
- Sektör, müvekkil tipi, ölçek
- VERBİS durumu, mevcut aydınlatma metni, varsa örnek risk değerlendirmesi
soruları sorulur. Cevaplar `~/.claude/plugins/config/claude-for-tr-legal/kvkk-uyum-tr/CLAUDE.md` dosyasına yazılır — bu **senin pratik profilin** olur ve sonraki tüm skill'ler buradan okur.

### 5. İlk skill'ini çalıştır

```
/kvkk-uyum-tr:aydinlatma-metni-uretici
```

veya:

```
/kvkk-uyum-tr:karar-analizi
```

## Mevcut Skill'ler (kvkk-uyum-tr)

| Skill | Ne Yapar |
|---|---|
| `cold-start-interview` | İlk kurulum + profil oluşturma |
| `aydinlatma-metni-uretici` | KVKK m.10 aydınlatma metni üretir |
| `ilgili-kisi-basvurusu` | İlgili kişi başvurularına (KVKK m.13/2) 30 günlük süre içinde yanıt taslar |
| `verbis-uyum-kontrolu` | VERBİS kayıt + güncelleme kontrolü, 6 aylık veri envanteri |
| `ihlal-triaji` | 72 saatlik KVKK bildirim zarfı (m.12/5 + Kurul kararı) |
| `risk-degerlendirme` | KVKK iyi uygulama düzeyinde veri koruma risk değerlendirmesi |
| `politika-fark-takibi` | Şirket politikası vs kullanıcının sağladığı son kararlar arasındaki sapmaları analiz eder |
| `karar-analizi` | Kullanıcının kvkk.gov.tr veya başka kaynaklardan sağladığı karar metnini Türk hukuku perspektifinden analiz eder; eğitim verisinden ilgili konularda bilgi sağlar (her atıf `[doğrulanmalı]` etiketli) |

## Veri Kaynakları & Doğrulama

Bu plugin, **resmi karar veritabanlarına doğrudan bağlanmaz**. Üç katmanlı çalışır:

1. **Model eğitim verisi:** KVKK çerçevesi, m.10/11/12/13 yorumları, geçmiş Kurul kararı örnekleri. Her atıf `[doğrulanmalı]` etiketli.
2. **Kullanıcı girdisi:** kvkk.gov.tr'den kopyaladığın karar metni veya URL'i analiz etmek için skill'lere doğrudan veriliyor.
3. **Opsiyonel — kendi araçların:** Kendi makinende kurduğun herhangi bir Türk hukuk veritabanı aracı varsa (Claude Cowork üzerinden bağlanmış olabilir), Claude bunları kullanabilir — bu plugin sadece kontrol etmez, varsa kullanır.

> ⚠️ **Resmi doğrulama her zaman gereklidir.** Plugin çıktısındaki karar atıfları yalnızca yön gösterici niteliktedir. **Karar numaraları, tarihleri ve içerikleri** kvkk.gov.tr'den teyit edilmelidir.

## Kapsam (user vs project)

Plugin yükledikten sonra Claude Code "user scope mu project scope mu?" diye sorabilir. **"user"** seç. Sebep:

- **user scope:** Hangi klasörde olursan ol plugin çalışır. Müvekkil dosyaların farklı klasörlerdeyse (Downloads, iCloud, Dropbox) bu lazım.
- **project scope:** Sadece bu repo'nun içinden çalışır. Çoğu skill için dosya okuma yetkisi gereklidir; project scope bunu kısıtlar.

Yanlış yüklediysen:
```
/plugin uninstall kvkk-uyum-tr
# Sonra home directory'de iken yeniden:
/plugin install kvkk-uyum-tr@claude-for-tr-legal
```

## Takıldığında

| Sorun | Çözüm |
|---|---|
| "Komut bulunamadı" | Claude Code'u yeniden başlat |
| "Önce cold-start çalıştır" | `/kvkk-uyum-tr:cold-start-interview` çalıştır |
| Karar atıflarında `[doğrulanmalı]` etiketi | Doğru — atıfı kvkk.gov.tr'den teyit et |
| Dosya okunamıyor | Plugin project scope'a yüklenmiş olabilir — user scope'a yeniden yükle |

## Çıktıların Hukuki Statüsü

> ⚠️ **Her çıktı avukat onayına hazır TASLAK'tır.** Direkt müvekkile veya KVKK'ya gönderilmez. Çıktıyı imzalayan/gönderen avukat sorumluluk taşır. Plugin hata yapabilir, kaynağı yanlış gösterebilir; her atıfı resmi kaynaktan doğrulanmalıdır.

Detaylı uyarılar [README.md](README.md) içinde.
