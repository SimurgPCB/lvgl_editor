# LVGL XML "Wrong Name" Hatası Çözümü

## ⚠️ Problem: "File/Directory must be a valid variable name" Hatası

LVGL Editor'de ekran veya component oluştururken bu hatayı alıyorsanız, dosya veya klasör adınız **C değişken isimlendirme kurallarına** uymuyordur.

## 🔧 Çözüm

### Dosya ve Klasör İsimlendirme Kuralları

LVGL XML dosyaları ve klasörleri **C programlama dilinin değişken isimlendirme kurallarına** uymak zorundadır çünkü bu isimler otomatik olarak C koduna dönüştürülür.

#### ✅ GEÇERLİ İsimler:

1. **Harf ile başlamalı** (a-z, A-Z) veya alt çizgi (_)
2. **Sadece şunları içerebilir:**
   - Harfler (a-z, A-Z)
   - Rakamlar (0-9)
   - Alt çizgi (_)
3. **TIRE (-), NOKTA (.), BOŞLUK kullanılamaz!**

```
✅ DOĞRU Örnekler:
- alarm.xml
- ekran_ana.xml
- kullanici_profil.xml
- theme_switcher.xml
- buton_1.xml
- _private_component.xml
- test_deneme.xml        ← Bu doğru!
```

#### ❌ GEÇERSİZ İsimler (Hata Verir):

```
❌ YANLIŞ Örnekler:
- test-deneme.xml        ← TİRE kullanılamaz!
- ekran.ana.xml          ← NOKTA kullanılamaz (.xml hariç)!
- my component.xml       ← BOŞLUK kullanılamaz!
- 1_buton.xml           ← RAKAM ile başlayamaz!
- kullanıcı-profil.xml  ← TİRE ve Türkçe karakter kullanılamaz!
```

## 📋 Örnekler

### Yanlış → Doğru Dönüşümler

| ❌ Yanlış | ✅ Doğru | Açıklama |
|----------|---------|----------|
| `test-deneme.xml` | `test_deneme.xml` | Tire yerine alt çizgi kullan |
| `ekran.ayarlar.xml` | `ekran_ayarlar.xml` | Nokta yerine alt çizgi kullan |
| `ana ekran.xml` | `ana_ekran.xml` | Boşluk yerine alt çizgi kullan |
| `1.buton.xml` | `buton_1.xml` | Rakam ile başlama |
| `kullanıcı.xml` | `kullanici.xml` | Türkçe karakter kullanma |

### Önerilen İsimlendirme Örnekleri

#### Ekranlar (Screens):
```
screen_ana.xml
screen_ayarlar.xml
screen_profil.xml
screen_giris.xml
```

#### Componentler:
```
buton_ana.xml
kart_kullanici.xml
slider_ses.xml
switch_tema.xml
profil_resmi.xml
```

#### Klasörler:
```
components/
  temel/
    buton/
    slider/
  kartlar/
    kullanici_karti/
    istatistik_karti/
screens/
  ana_ekran/
  ayarlar/
```

## 🎯 Senin Durumun İçin Çözüm

Eğer `test_deneme.xml` veya benzeri bir isim kullanıyorsan:

### ✅ Bu İsimler DOĞRU (Hata vermez):
- `test_deneme.xml`
- `test_component.xml`
- `deneme_1.xml`
- `yeni_ekran.xml`

### ❌ Bu İsimler YANLIŞ (Hata verir):
- `test-deneme.xml` (tire var)
- `test.deneme.xml` (nokta var)
- `test deneme.xml` (boşluk var)

## 📂 Proje Yapısı Örneği

```
my_project/
├── project.xml
├── globals.xml
├── components/
│   ├── temel/
│   │   ├── buton/
│   │   │   └── buton.xml          ✅ Doğru
│   │   └── kart/
│   │       └── kart_ana.xml       ✅ Doğru
│   └── ozel/
│       └── kullanici_profil/
│           └── kullanici_profil.xml  ✅ Doğru
├── screens/
│   ├── screen_ana.xml             ✅ Doğru
│   ├── screen_ayarlar.xml         ✅ Doğru
│   └── screen_profil.xml          ✅ Doğru
└── images/
    ├── icon_home.png              ✅ Doğru
    └── kullanici_avatar.png       ✅ Doğru
```

## ❓ Neden Bu Kural Var?

LVGL XML dosyalarından **otomatik olarak C kodu üretilir**. Dosya ve klasör isimleri:

1. **Fonksiyon isimleri** olur
2. **Değişken isimleri** olur
3. **Tip (type) isimleri** olur
4. **Include guard isimleri** olur

Örnek:
```xml
<!-- test_deneme.xml dosyası -->
<component>
    ...
</component>
```

Şu C koduna dönüşür:
```c
// test_deneme_gen.h
#ifndef TEST_DENEME_GEN_H
#define TEST_DENEME_GEN_H

lv_obj_t* test_deneme_create(lv_obj_t* parent);

#endif
```

Eğer `test-deneme.xml` kullansaydık:
```c
// HATA! C'de fonksiyon ismi tire içeremez
lv_obj_t* test-deneme_create(lv_obj_t* parent);  ❌ DERLEME HATASI!
```

## 🛠️ Hata Düzeltme Adımları

1. **Dosya ismini kontrol et** - Tire, nokta, boşluk var mı?
2. **Rakamla başlıyor mu?** - Rakamla başlayan isimleri değiştir
3. **Türkçe karakter var mı?** - ç, ğ, ı, ş, ü, ö → c, g, i, s, u, o
4. **Alt çizgi kullan** - Kelime ayırmak için `_` kullan
5. **Dosyayı yeniden adlandır** - Doğru isimlendirmeye göre düzelt

### Adım Adım Örnek:

```
Orijinal: test-deneme.xml ❌

1. Tireyi tespit et: test-deneme
2. Alt çizgiyle değiştir: test_deneme
3. .xml ekle: test_deneme.xml ✅

Sonuç: test_deneme.xml (DOĞRU!)
```

## 📚 Ek Kaynaklar

- **Resmi Dokümantasyon:** https://docs.lvgl.io/master/details/xml/index.html
- **Detaylı Türkçe/İngilizce Dokümantasyon:** `LVGL_XML_DOCUMENTATION.md`
- **LVGL Pro Editor:** https://pro.lvgl.io
- **Online Viewer:** https://viewer.lvgl.io

## ✨ Özet

### Hatanın Sebebi:
LVGL XML dosya/klasör isimleri **C değişken isimlendirme kurallarına** uymak zorunda.

### Çözüm:
- ✅ **Sadece harf, rakam ve alt çizgi (_) kullan**
- ❌ **Tire (-), nokta (.), boşluk kullanma**
- ❌ **Rakamla başlama**
- ❌ **Türkçe karakter kullanma**

### Senin Durumun:
```
❌ test-deneme.xml    → Tire var, hata verir
✅ test_deneme.xml    → Doğru, hata vermez
```

## 🎉 İyi Çalışmalar!

Bu kuralları takip ederek LVGL Editor'de sorunsuz çalışabilirsin. Herhangi bir dosya veya klasör oluştururken bu isimlendirme kurallarını hatırla!
