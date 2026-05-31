# KVKK Pratik Profili

> Bu dosya bir TEMPLATE'dir. Cold-start interview tamamlandığında `~/.claude/plugins/config/claude-for-tr-legal/kvkk-uyum-tr/CLAUDE.md` konumuna doldurulmuş halde yazılır.
> [PLACEHOLDER] etiketleri interview sırasında değiştirilir.

## 1. Büro / Şirket Profili

Şirket bilgileri **paylaşılan profilde** saklanır: `~/.claude/plugins/config/claude-for-tr-legal/company-profile.md`.

Bu dosya, sadece KVKK pratiğine özel detayları içerir.

## 2. KVKK Rolü

- **Veri sorumlusu mu, veri işleyen mi, ikisi de mi?** [PLACEHOLDER]
- **VERBİS kayıt durumu:** [Kayıtlı / Zorunlu değil / Kayıtlı olmalıydı]
- **VERBİS son güncelleme:** [PLACEHOLDER tarih]
- **DPIA / VEKD geçmişi:** [Yok / X tane var / düzenli yapılıyor]

## 3. Veri Kategorileri & İşleme Faaliyetleri

Bu büronun/şirketin işlediği başlıca veri kategorileri:

- [ ] Kimlik verisi
- [ ] İletişim verisi
- [ ] Lokasyon
- [ ] Müşteri işlem
- [ ] Finansal
- [ ] Görsel/işitsel kayıt
- [ ] Çerez (web sitesi)
- [ ] **Özel nitelikli — Sağlık**
- [ ] **Özel nitelikli — Biyometrik**
- [ ] **Özel nitelikli — Ceza mahkumiyeti**
- [ ] **Özel nitelikli — Diğer (din, etnik, siyasi)**
- [ ] Diğer: [PLACEHOLDER]

**Ana işleme amaçları:** [PLACEHOLDER]
**Saklama süreleri (genel):** [PLACEHOLDER]

## 4. Aydınlatma Metni Durumu

- **Mevcut aydınlatma metni:** [Var / Yok / Güncelleme gerekli]
- **Son güncelleme tarihi:** [PLACEHOLDER]
- **Hangi noktalarda kullanılıyor:** [Web sitesi / sözleşme / üyelik formu / İK / müvekkil sözleşmesi / diğer]
- **Açık rıza alımı yöntemi:** [Onay kutusu / yazılı / sözlü — kayıt altında / diğer]

## 5. İlgili Kişi Başvuru İşleyişi

- **Başvuru kanalları:** [E-posta / yazılı dilekçe / KEP / web formu / diğer]
- **Yıllık başvuru sayısı (yaklaşık):** [PLACEHOLDER]
- **Reddedilen başvuru oranı:** [PLACEHOLDER]
- **30 günlük süreyi takip eden sistem:** [Excel / CRM / E-posta hatırlatıcı / Yok]

## 6. Veri Sorumlusu Sözleşmeleri

- **Veri işleyenlerle sözleşme şablonu:** [Var / Yok / Standart KVKK eki ile]
- **Sınır ötesi veri aktarımı:** [Var / Yok] — Varsa hangi ülkelere, hangi gerekçeyle (md. 9)
- **Açık rıza dayanağı:** [Yeterli / Yetersiz / Belirsiz]

## 7. İhlal Yönetimi

- **Geçmiş ihlal sayısı:** [Yok / 1-3 / 4+] (son 3 yıl)
- **İhlal bildirim süreci:** [Tanımlı / Yarım / Yok]
- **72 saat içinde bildirilebilir mi?** [Evet / Belirsiz / Hayır]
- **Olay müdahale ekibi:** [Var / Yok]

## 8. Plugin Davranışı Tercihleri

- **Çıktı dili:** Türkçe (varsayılan)
- **Atıf detayı:** [Minimum (sadece madde no) / Tam (madde + karar metni özeti) / Akademik (kaynak + URL + tam alıntı)]
- **Risk eşiği:** [Konservatif / Dengeli / Pratik] — Plugin kararsız kalırsa hangi tarafa yatsın
- **TBB Meslek Kuralları kontrolü:** [Her çıktıda / Hassas konularda / Kapalı]

## 9. Kullanılabilir Araçlar (Opsiyonel)

Bu plugin kendi başına üçüncü taraf MCP zorunlu kılmaz. Aşağıdakiler **kullanıcı tarafından isteğe bağlı** olarak kurulmuşsa skill'ler bunları kullanabilir:

- WebFetch (Claude built-in): [✓ varsayılan açık]
- Kullanıcının kurduğu KVKK/yargı arama araçları: [✓ / ⚪ / ✗]
  - Son test: [PLACEHOLDER tarih]
- Google Drive (dosya okuma): [✓ / ⚪ / ✗]

Plugin yokluğunda hata vermez; model + kullanıcı girdisi modunda çalışır.

## 10. Notlar / Plugin'e Özel Talimatlar

[PLACEHOLDER — kullanıcı serbest format not bırakır]

---

**Setup başlangıç:** [PLACEHOLDER tarih]
**Son güncelleme:** [PLACEHOLDER tarih]
**Cold-start interview süresi:** [PLACEHOLDER dk]

Bilgileri güncellemek için: `/kvkk-uyum-tr:cold-start-interview --redo`
Entegrasyonları yeniden kontrol etmek için: `/kvkk-uyum-tr:cold-start-interview --check-integrations`
