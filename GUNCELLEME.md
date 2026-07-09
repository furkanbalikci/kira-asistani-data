# TÜFE Verisi Aylık Güncelleme Rehberi (5 dakikalık iş)

Her ayın 3'ünde (genellikle sabah 10:00'da) TÜİK yeni TÜFE verisini açıklar.
Yapmanız gereken tek şey: 3 sayıyı bulup `tufe.json` dosyasına yeni bir satır eklemek ve GitHub'a göndermek.

## Adım 1: TÜİK'ten 3 sayıyı bulun

1. Tarayıcıda şu adresi açın: https://data.tuik.gov.tr/Bulten/Index?p=Tuketici-Fiyat-Endeksi
   (En üstteki, en yeni tarihli "Tüketici Fiyat Endeksi, [Ay] [Yıl]" bültenine tıklayın.)
2. Bültenin özet metninde şu üç değeri bulun:
   - **"bir önceki aya göre %X,XX arttı"** → bu `aylik` değeridir
   - **"bir önceki yılın aynı ayına göre %X,XX arttı"** → bu `yillik` değeridir
   - **"on iki aylık ortalamalara göre %X,XX arttı"** → bu `oniki_ay_ortalama` değeridir ⚠️ KİRA ZAMMI İÇİN KULLANILAN ORAN BUDUR
3. Virgüllü sayıları noktaya çevirin: `35,42` → `35.42`

## Adım 2: tufe.json dosyasını güncelleyin

`tufe.json` dosyasını herhangi bir metin düzenleyicide açın (GitHub web arayüzünden de düzenleyebilirsiniz: dosyaya tıklayın → kalem ikonu).

1. `"guncelleme_tarihi"` alanını bugünün tarihi yapın (örn. `"2026-08-03"`).
2. `"veriler"` listesinin **en sonuna** yeni ayı ekleyin. Örnek — 2026 Temmuz verisi açıklandıysa:

```json
    {"yil": 2026, "ay": 7, "aylik": 2.35, "yillik": 34.80, "oniki_ay_ortalama": 40.55}
```

⚠️ Dikkat: Bir önceki satırın sonuna **virgül** eklemeyi unutmayın, yeni (son) satırın sonunda virgül OLMAMALI.

3. İsterseniz 24 aydan eski satırları silebilirsiniz (zorunlu değil; uygulama son 24 ayı gösterir).

## Adım 3: GitHub'a gönderin

**GitHub web arayüzü kullandıysanız:** "Commit changes" butonuna basın. Bitti.

**Bilgisayarınızdan düzenlediyseniz**, terminalde:

```bash
cd kira-asistani-data
git add tufe.json
git commit -m "TÜFE 2026-07 verisi eklendi"
git push
```

## Kontrol

Tarayıcıda şu adresi açıp yeni ayın göründüğünü doğrulayın:
`https://raw.githubusercontent.com/KULLANICI_ADI/kira-asistani-data/main/tufe.json`

Uygulama bir sonraki açılışta yeni veriyi otomatik çeker. Uygulama güncellemesi GEREKMEZ.

## Sık yapılan hatalar

- JSON'da virgül/tırnak hatası → uygulama eski önbelleği kullanmaya devam eder (çökmez ama yeni veri görünmez). Dosyayı https://jsonlint.com adresine yapıştırıp kontrol edebilirsiniz.
- `aylik` ile `oniki_ay_ortalama` değerlerini karıştırmak → kira hesabı yanlış çıkar. Kira zammında kullanılan **"on iki aylık ortalamalara göre değişim"** oranıdır.
- Ondalık ayracı olarak virgül kullanmak → `2,35` değil `2.35` yazın.
