# Firma / Büro Profili

> Bu dosya `~/.claude/claude-for-tr-legal/firma-profili.md` konumunda saklanır.
> İlk kurulan plugin'in cold-start interview'unda doldurulur. Sonraki plugin'ler bu dosyayı okur ve büro bilgilerini tekrar sormaz.
> İçeriği güncel tutmak için herhangi bir plugin'in `cold-start-interview --firma-guncelle` komutu ile yeniden çalıştırılabilir.

## Temel Bilgiler

- **Pratik ortamı:** [Bağımsız avukat / Hukuk bürosu / Şirket içi (in-house) / Kamu / Üniversite kliniği]
- **Büro/Şirket adı:** [PLACEHOLDER]
- **Sektör:** [PLACEHOLDER] (örn. teknoloji hukuku, IP, ticaret, aile hukuku, ceza, idare)
- **Ana hizmet:** [PLACEHOLDER] (1-2 cümlede ne yaptığınız)
- **Ölçek:** [Solo / 2-5 avukat / 6-20 / 21-50 / 50+]
- **Konum:** [İl/ilçe — birden fazla ofis varsa hepsi]

## Yargı Çevresi

- **Birincil yargı çevresi:** [PLACEHOLDER] (örn. İzmir, İstanbul, Ankara)
- **Faaliyet gösterilen iller:** [PLACEHOLDER]
- **Uluslararası iş:** [Var / Yok] — varsa hangi ülkeler/diller

## Düzenleyici Otoriteler

Hangi otoritelerin kararları/düzenlemeleri sizi etkiler? (Plugin'ler buna göre veri kaynağı seçer)

- [ ] KVKK (veri koruma)
- [ ] TÜRKPATENT (marka, patent, tasarım)
- [ ] BDDK (bankacılık)
- [ ] SPK (sermaye piyasası)
- [ ] Rekabet Kurumu
- [ ] KİK (kamu ihale)
- [ ] EPDK (enerji)
- [ ] BTK (bilişim)
- [ ] RTÜK (yayıncılık)
- [ ] GİB (vergi)
- [ ] SGK
- [ ] Diğer: [PLACEHOLDER]

## Risk İştahı

- **Risk profili:** [Konservatif / Dengeli / Atak]
- **"Hayır" diyebileceğin durumlar:** [PLACEHOLDER]
- **Eskalasyon eşiği:** Hangi durumda partner/üst avukat onayı gerekir? [PLACEHOLDER]

## Eskalasyon Zinciri

İsim ve rol — plugin "bunu kim onaylasın?" sorusunda kullanır:

| İsim | Rol | Hangi konularda |
|---|---|---|
| [PLACEHOLDER] | [PLACEHOLDER] | [PLACEHOLDER] |
| [PLACEHOLDER] | [PLACEHOLDER] | [PLACEHOLDER] |

## Çalışılan Müvekkil Tipleri

- [ ] Bireysel müvekkil
- [ ] KOBİ (1-50 çalışan)
- [ ] Orta ölçek (51-250)
- [ ] Büyük şirket (250+)
- [ ] Çokuluslu / yabancı sermaye
- [ ] Aile şirketi
- [ ] Startup
- [ ] Kamu kurumu
- [ ] Sivil toplum
- [ ] Yabancı uyruklu

## Dil Tercihleri

- **Müvekkil iletişim dili:** [Türkçe / İngilizce / İkisi de]
- **İç çalışma dili:** [PLACEHOLDER]
- **Resmi belge dili:** Türkçe (varsayılan)

## TBB / Baro Bilgileri

- **Üye olunan baro:** [PLACEHOLDER]
- **Levha numarası (opsiyonel):** [PLACEHOLDER]
- **TBB Meslek Kuralları uyum kontrolü:** [Aktif / Pasif] — Aktif ise her çıktı TBB Meslek Kuralları + Avukatlık Kanunu gözetilerek üretilir

---

## Notlar

Bu profil sadece **yerel makinende** saklanır. Hiçbir plugin bu bilgiyi dışarı göndermez. Anthropic API'sine sadece o anki sorunun cevabı için gerekli olan kısmı iletilir.

Bilgilerini değiştirmek için:
```
/<aktif-plugin>:cold-start-interview --firma-guncelle
```
