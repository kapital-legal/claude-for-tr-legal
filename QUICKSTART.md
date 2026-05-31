# Hızlı Başlangıç

**Hedef:** 5 dakikada bir KVKK skill'i çalıştırıyor olmak.

## Ön Koşullar

1. **Claude Code veya Claude Cowork** — Pro/Max abonelik gerekir.
2. **yargi-mcp** — yüklü olmalı. Kontrol:
   ```
   Claude Code'da: /plugin diye başlayan komutlarda yargi-mcp araçları görünüyorsa hazır.
   ```
   Yüklü değilse: https://github.com/saidsurucu/yargi-mcp

## Kurulum

### 1. Marketplace'i ekle

Claude Code terminalinde:
```
/plugin marketplace add /Users/av.haruneren/Desktop/Agent-dosyaları/claude-for-tr-legal
```

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
- VERBİS durumu, mevcut aydınlatma metni, varsa örnek DPIA
soruları sorulur. Cevaplar `~/.claude/plugins/config/claude-for-tr-legal/kvkk-uyum-tr/CLAUDE.md` dosyasına yazılır — bu **senin pratik profilin** olur ve sonraki tüm skill'ler buradan okur.

### 5. İlk skill'ini çalıştır

```
/kvkk-uyum-tr:aydinlatma-metni-uretici
```

veya:

```
/kvkk-uyum-tr:emsal-karar-arayici
```

## Mevcut Skill'ler (kvkk-uyum-tr)

| Skill | Ne Yapar |
|---|---|
| `cold-start-interview` | İlk kurulum + profil oluşturma |
| `aydinlatma-metni-uretici` | KVKK m.10 aydınlatma metni üretir |
| `ilgili-kisi-basvurusu` | İlgili kişi başvurularına (DSAR) 30 günlük süre içinde yanıt taslar |
| `verbis-uyum-kontrolu` | VERBİS kayıt + güncelleme kontrolü, 6 aylık veri envanteri |
| `etki-degerlendirme` | DPIA / Veri Koruma Etki Değerlendirmesi üretir |
| `ihlal-triaji` | Veri ihlali raporu — 72 saatlik KVKK bildirim zarfı |
| `emsal-karar-arayici` | yargi-mcp ile canlı KVKK Kurulu kararı arar, durumuna emsal getirir |
| `politika-fark-takibi` | Şirket politikası vs son KVKK kararları arasındaki sapmaları tarar |

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
| "yargi-mcp aracı yok" | yargi-mcp kurulumunu kontrol et — README'deki bağlantı |
| Çıktıda `[verify]` etiketi | Bilgi training data'dan geliyor, yargi-mcp'den doğrulanması gerek |
| Dosya okunamıyor | Plugin project scope'a yüklenmiş olabilir — user scope'a yeniden yükle |

## Çıktıların Hukuki Statüsü

> ⚠️ **Her çıktı avukat onayına hazır TASLAK'tır.** Direkt müvekkile veya KVKK'ya gönderilmez. Çıktıyı imzalayan/gönderen avukat sorumluluk taşır. Plugin hata yapabilir, kaynağı yanlış gösterebilir; her atıfı yargi-mcp veya resmi kaynakla doğrulanmalıdır.

Detaylı uyarılar [README.md](README.md) içinde.
