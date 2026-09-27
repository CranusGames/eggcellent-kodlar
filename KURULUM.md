# Haftanın Kodu Paneli — kurulum

- **Site:** https://cranusgames.github.io/eggcellent-kodlar/
- **Repo:** https://github.com/CranusGames/eggcellent-kodlar

Bu site iki şeyi yönetir; ikisi de oyun güncellemesi ya da sunucuyu yeniden başlatmak
gerektirmez:

1. **Haftanın Kodu** — oyundaki HAFTANIN KODU kartı. Biri kodu yazıp **Onayla ve yayınla**'ya
   basar; sunucu en geç ~2 dakika içinde alır, kod başlangıç saatinde ana menüde görünür.
2. **Oyuncu Ayarları** — oyuncunun hızı, zıplaması, dash mesafesi, hasarı, canı ve
   düşmanların hasarı (yüzde olarak, %50–%200).

```
 arkadaşın (telefon)          GitHub (bu repo)              oyun sunucusu (VPS)
 ───────────────────          ─────────────────             ───────────────────
 siteye kodu yazar  ──────►   weekly_codes.json   ◄──────   2 dk'da bir kontrol eder,
 "Onayla"ya basar             güncellenir                   değiştiyse oyunculara yollar
```

Site sunucuya **hiç bağlanmaz**. Sunucu dosyayı GitHub'dan kendisi çeker, yani sunucuda
internete açık yeni bir kapı yoktur. Kimin kod yazabileceğine GitHub karar verir.

---

## 1) Repo ve site (HAZIR)

Repo ve GitHub Pages kuruldu (public — ücretsiz hesapta Pages sadece public repoda çalışır).
Dosyalar: `index.html` (site), `weekly_codes.json` (kod takvimi), `tuning.json` (oyuncu
ayarları), `KURULUM.md` (bu rehber). Kaynakları oyun projesinde `Deploy/kod-paneli/`.

> Repo public olduğu için `weekly_codes.json` içindeki **gelecek haftaların kodları da
> görülebilir**. Bu bir açık değildir: sunucu sadece o an aktif olan kodu kabul eder. Tek
> sonucu, merak edenin sürprizi önceden görebilmesidir.

## 2) Sunucuyu repoya bağla (sen, bir kez — yeni sunucu build'i deploy edildikten SONRA)

Proje kökünde, Git Bash'te:

```bash
./Deploy/weekly-codes.sh root@62.238.35.145 --github https://api.github.com/repos/CranusGames/eggcellent-kodlar/contents/weekly_codes.json
```

Repo public olduğu için token gerekmez; `tuning.json` aynı klasörden kendiliğinden okunur.
Komut lobiyi bir kez yeniden başlatır (o an menüde olanlar birkaç saniye kopar) ve son log
satırlarını gösterir. Şunu görmelisin:

```
[Uzak] GitHub kaynagi: https://api.github.com/repos/... (120 sn'de bir, tokensiz)
[Uzak] GitHub'dan YENI haftalik kod alindi, yerel dosya guncellendi.
[HaftalikKod] Takvim yuklendi: 4 kod.
[Uzak] GitHub'dan YENI oyuncu ayarlari alindi, yerel dosya guncellendi.
[Ayar] Yuklendi — AcikDunya: hiz x1 zipla x1 ...
```

Durumu sonra kontrol etmek için:

```bash
./Deploy/weekly-codes.sh root@62.238.35.145 --show
```

> Bundan sonra takvimin **tek kaynağı GitHub'dır**. Sunucudaki dosyayı elle değiştirirsen
> birkaç dakika içinde GitHub'daki sürümle ezilir.

## 3) Arkadaşına yazma yetkisi ver

Sen yokken de çalışabilmesi için arkadaşın **kendi GitHub hesabıyla** girmeli; token'ının
süresi dolarsa kendisi yenileyebilir.

**Sen:** repo → **Settings → Collaborators → Add people** → arkadaşının GitHub adı.
Arkadaşın gelen daveti kabul etmeli (e-posta ya da github.com/notifications).

**Arkadaşın:**

1. GitHub → sağ üstte profil resmi → **Settings → Developer settings → Personal access
   tokens → Tokens (classic) → Generate new token (classic)**.
2. *Note:* `eggcellent kod paneli`; *Expiration:* **No expiration** (ya da uzun bir süre);
   *Scopes:* sadece **`public_repo`** kutusu.
3. **Generate token** → çıkan `ghp_...` metnini kopyala. Bu metin bir daha gösterilmez.
4. Siteyi aç, token'ı yapıştır, **Bağlan**. Repo adı site adresinden kendiliğinden dolar.

Token sadece o tarayıcıda saklanır. Telefon kaybolursa: GitHub → aynı sayfadan token'ı
**Delete** et, yenisini üret.

## 4) Kod eklemek (arkadaşın, her hafta ya da toplu)

- **Kod:** sadece A–Z ve 0–9, 3–20 karakter. Türkçe harfleri site kendisi düzeltir
  (`şeker` → `SEKER`). **Üret** butonu rastgele kod önerir.
- **Başlangıç:** Türkiye saati. Site, takvimdeki son kodun bittiği anı kendisi önerir.
  Kod bir sonraki kodun başlangıcına kadar (en fazla 7 gün) geçerlidir.
- **Ödül:** küçük tut (50–150). Ara sıra **400** = bir basit skin parası. En fazla 1000
  (sunucu fazlasını kırpar).
- Özet kutusunu oku → **Onayla ve yayınla** → çıkan pencerede **Yayınla**.
- Henüz başlamamış bir kodu çöp kutusu simgesiyle silebilirsin. Başlamış ya da bitmiş kod
  silinemez (oyuncular kullanmış olabilir).

**Birkaç haftayı önceden girmek serbest.** Sunucu sırası gelince kendisi geçer.

## 5) Oyuncu ayarları (sitede ikinci sekme)

Her kaydırıcı oyundaki normal değere göre bir yüzde: **%100 = değişiklik yok**.

| Grup | Ayar | Not |
|---|---|---|
| Açık Dünya | Koşu hızı, Zıplama gücü, Dash mesafesi | zıplama yüksekliği karesiyle artar (%120 ≈ %44 daha yüksek) |
| Açık Dünya | Oyuncunun vurduğu hasar, Oyuncunun canı | |
| Açık Dünya | Düşmanların vurduğu hasar | yükseltmek oyunu zorlaştırır |
| Maçlar | Koşu hızı, Zıplama gücü | Yarış parkurları %100'e göre ölçüldü — dikkatli |

- Değer %50–%200 dışına çıkamaz; oyun da ayrıca kırpar, yanlış bir değer oyunu bozamaz.
- Yayınlamadan önce pencere neyin neye değiştiğini listeler ("%100 → %120").
- Menüdeki oyuncular ~2 dakikada, Açık Dünya'daki oyuncular bir sonraki girişlerinde alır.
- Oyuncu çevrimdışıyken son aldığı ayarla oynar.
- Geri almak: kaydırıcıları eski yerine getir (ya da **Hepsini %100 yap**) ve tekrar yayınla.

Dash **bekleme süresi** bilerek yok: sunucu onu ayrıca doğruluyor, sadece bir tarafta
değişirse dash'ler reddedilir.

## Sorun olursa

| Sitede görünen | Sebep | Çözüm |
|---|---|---|
| Token geçersiz ya da süresi dolmuş | token silinmiş/süresi bitmiş | yeni token üret, **Çıkış** → tekrar bağlan |
| Repoyu görüyor ama YAZMA izni yok | collaborator daveti kabul edilmemiş | daveti kabul et |
| Repo bulunamadı | repo adı yanlış | `kullanıcı/repo` biçiminde yaz |
| Dosya bu arada değişti | aynı anda iki kişi düzenledi | sayfa yenilendi, tekrar yayınla |

Yanlış bir şey yayınlandıysa: repo → `weekly_codes.json` → **History** → eski sürümü aç →
içeriği kopyala → dosyayı düzenle, yapıştır, kaydet. Sunucu ~2 dakikada eski hale döner.

GitHub çökerse ya da sunucu GitHub'a ulaşamazsa: sunucu **son aldığı takvimle çalışmaya
devam eder**, oyunda bir şey bozulmaz.
