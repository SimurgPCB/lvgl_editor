# LVGL XML Quick Start Guide

## 🚨 Hata Çözümü: "Wrong Name" Hatası

### Problem
LVGL Editor'da ekran veya component oluştururken `test_deneme.xml` gibi dosya adlarında "Wrong Name" hatası alıyorsunuz.

### Sebep
LVGL XML dosya adları **C değişken adı kurallarına** uymalıdır. Türkçe karakterler ve özel karakterler kullanılamaz.

### ✅ Çözüm

#### Geçerli Dosya Adları:
```
✅ test_deneme.xml      (Türkçe karakter yoksa)
✅ ekran_gorunumu.xml   (ğ → g, ü → u)
✅ ana_sayfa.xml
✅ buton_bileseni.xml
✅ main_screen.xml
✅ my_component.xml
```

#### ❌ Geçersiz Dosya Adları:
```
❌ test-deneme.xml      (tire karakteri)
❌ test deneme.xml      (boşluk)
❌ 123ekran.xml         (sayı ile başlama)
❌ ekran@home.xml       (özel karakter)
❌ görünüm.xml          (Türkçe karakter)
```

#### Türkçe Karakter Dönüşümü:
```
ğ → g    (örnek: değer → deger)
ş → s    (örnek: başlık → baslik)
ç → c    (örnek: çerçeve → cerceve)
ı → i    (örnek: ışık → isik)
ö → o    (örnek: görünüm → gorunum)
ü → u    (örnek: düğme → dugme)
İ → I    (örnek: İçerik → Icerik)
Ğ → G, Ş → S, Ç → C, Ö → O, Ü → U
```

## 🚀 Hızlı Başlangıç

### 1. Yeni Ekran Oluşturma

**Dosya:** `screens/ana_ekran.xml`

```xml
<screen>
    <styles>
        <style name="arkaplan_stili" bg_color="0xf0f0f0" pad_all="20" />
    </styles>
    
    <view>
        <style name="arkaplan_stili" />
        
        <lv_label text="Merhaba Dünya!" align="center" />
        
        <lv_button align="center" style_margin_top="20">
            <lv_label text="Tıkla" />
        </lv_button>
    </view>
</screen>
```

### 2. Yeni Component Oluşturma

**Dosya:** `components/ozel_buton.xml`

```xml
<component>
    <api>
        <prop name="buton_metni" type="string" default="Buton" />
        <prop name="renk" type="color" default="0x0066cc" />
    </api>
    
    <styles>
        <style name="buton_stili"
               bg_color="$renk"
               text_color="0xffffff"
               radius="8"
               pad_hor="16"
               pad_ver="8" />
    </styles>
    
    <view extends="lv_button">
        <style name="buton_stili" />
        <lv_label text="$buton_metni" align="center" />
    </view>
</component>
```

### 3. Component Kullanımı

**Dosya:** `screens/buton_demo.xml`

```xml
<screen>
    <view style_pad_all="20">
        <column width="100%">
            <ozel_buton buton_metni="Ana Buton" renk="0x0066cc" />
            <ozel_buton buton_metni="İkinci Buton" renk="0xcc6600" style_margin_top="10" />
        </column>
    </view>
</screen>
```

## 📁 Proje Yapısı

```
proje/
├── project.xml          # Ana proje ayarları
├── globals.xml          # Global kaynaklar
├── screens/            # Ekranlar
│   ├── ana_ekran.xml
│   ├── ayarlar_ekrani.xml
│   └── ...
├── components/         # Tekrar kullanılabilir bileşenler
│   ├── ozel_buton.xml
│   ├── kart_bileseni.xml
│   └── ...
├── images/            # Resim dosyaları
├── fonts/             # Font dosyaları
└── preview-bin/       # Otomatik oluşturulan dosyalar
```

## 🎨 Temel Stil Özellikleri

### Boyutlar
```xml
<style name="boyut_stili"
       width="200"
       height="100"
       min_width="50"
       max_width="300" />
```

### Renkler
```xml
<style name="renk_stili"
       bg_color="0xff0000"      <!-- Kırmızı arka plan -->
       text_color="0xffffff"    <!-- Beyaz metin -->
       border_color="0x000000"  <!-- Siyah kenarlık -->
       bg_opa="80%" />          <!-- %80 şeffaflık -->
```

### Kenarlık ve Radius
```xml
<style name="cerceve_stili"
       border_width="2"
       border_color="0x333333"
       radius="10" />
```

### Padding ve Margin
```xml
<style name="bosluk_stili"
       pad_all="10"           <!-- Tüm yönlerde 10px iç boşluk -->
       pad_top="5"            <!-- Üst iç boşluk -->
       margin_all="15"        <!-- Tüm yönlerde 15px dış boşluk -->
       margin_left="20" />    <!-- Sol dış boşluk -->
```

## 🔗 Veri Bağlama (Data Binding)

### Global Değişkenler Tanımlama

**Dosya:** `globals.xml`

```xml
<globals name="uygulama">
    <subjects>
        <int name="sayac" value="0" />
        <string name="kullanici_adi" value="Misafir" />
        <bool name="karanlik_tema" value="false" />
    </subjects>
    
    <styles>
        <style name="aydinlik_tema" bg_color="0xffffff" text_color="0x000000" />
        <style name="karanlik_tema" bg_color="0x000000" text_color="0xffffff" />
    </styles>
</globals>
```

### Veri Bağlama Kullanımı

```xml
<screen>
    <view>
        <!-- Koşullu stil uygulama -->
        <bind_style subject="karanlik_tema" name="aydinlik_tema" ref_value="false" />
        <bind_style subject="karanlik_tema" name="karanlik_tema" ref_value="true" />
        
        <!-- Dinamik metin -->
        <lv_label text="Merhaba $kullanici_adi!" />
        <lv_label text="Sayaç: $sayac" />
        
        <!-- Buton ile sayaç artırma -->
        <lv_button>
            <lv_label text="Artır" />
            <change_subject_event subject="sayac" operation="add" value="1" />
        </lv_button>
    </view>
</screen>
```

## 📱 Layout Örnekleri

### Dikey Sıralama (Column)
```xml
<column width="100%" height="100%" style_flex_cross_place="center">
    <lv_label text="Başlık" />
    <lv_button><lv_label text="Buton 1" /></lv_button>
    <lv_button><lv_label text="Buton 2" /></lv_button>
</column>
```

### Yatay Sıralama (Row)
```xml
<row width="100%" style_flex_cross_place="center">
    <lv_button flex_grow="1"><lv_label text="Sol" /></lv_button>
    <lv_button flex_grow="1"><lv_label text="Orta" /></lv_button>
    <lv_button flex_grow="1"><lv_label text="Sağ" /></lv_button>
</row>
```

### Flex Layout
```xml
<div flex_flow="row_wrap" width="100%">
    <lv_button style_margin_all="5"><lv_label text="1" /></lv_button>
    <lv_button style_margin_all="5"><lv_label text="2" /></lv_button>
    <lv_button style_margin_all="5"><lv_label text="3" /></lv_button>
</div>
```

## 🖼️ Resim ve Font Kullanımı

### Resim Tanımlama (globals.xml)
```xml
<images>
    <file name="ana_logo" src_path="images/logo.png" />
    <file name="arka_plan" src_path="images/background.jpg" />
</images>
```

### Resim Kullanımı
```xml
<lv_image src="ana_logo" align="center" />
```

### Font Tanımlama (globals.xml)
```xml
<fonts>
    <tiny_ttf name="buyuk_font" size="24" src_path="fonts/arial.ttf" />
    <tiny_ttf name="kucuk_font" size="12" src_path="fonts/arial.ttf" />
</fonts>
```

### Font Kullanımı
```xml
<lv_label text="Büyük Başlık" style_text_font="buyuk_font" />
<lv_label text="Küçük metin" style_text_font="kucuk_font" />
```

## ⚡ Animasyonlar

### Timeline Animasyonu
```xml
<lv_button>
    <lv_label text="Animasyon Başlat" />
    <play_timeline_event target="hedef_nesne" timeline="fade_in" />
</lv_button>
```

## 🐛 Sık Karşılaşılan Hatalar ve Çözümleri

### 1. "Resource not found" Hatası
**Çözüm:** `globals.xml` dosyasında kaynak tanımlandığından emin olun.

### 2. "Invalid property" Hatası
**Çözüm:** Özellik adlarını ve değerlerini kontrol edin.

### 3. "Syntax error" Hatası
**Çözüm:** XML syntax'ını kontrol edin, tüm tag'lerin kapatıldığından emin olun.

### 4. Derleme Hatası
**Çözüm:** Dosya adlarının C identifier kurallarına uyduğundan emin olun.

## 📚 Yararlı İpuçları

1. **Dosya Adları:** Her zaman İngilizce karakterler kullanın
2. **Stil Organizasyonu:** Ortak stilleri `globals.xml`'de tanımlayın
3. **Component Tasarımı:** Tekrar kullanılabilir bileşenler oluşturun
4. **Test Etme:** Küçük örneklerle başlayın
5. **Dokümantasyon:** Karmaşık bileşenleri yorumlayın

## 🔧 Hızlı Referans

### Temel Widget'lar
- `<lv_button>` - Buton
- `<lv_label>` - Metin etiketi
- `<lv_image>` - Resim
- `<lv_slider>` - Kaydırıcı
- `<lv_switch>` - Anahtar
- `<lv_checkbox>` - Onay kutusu
- `<lv_dropdown>` - Açılır liste

### Layout Container'ları
- `<view>` - Temel container
- `<row>` - Yatay sıralama
- `<column>` - Dikey sıralama
- `<div>` - Flex layout

### Hizalama Değerleri
- `center` - Orta
- `top_left` - Sol üst
- `top_right` - Sağ üst
- `bottom_left` - Sol alt
- `bottom_right` - Sağ alt

Bu rehber ile LVGL XML kullanarak hızlıca başlayabilir ve "Wrong Name" hatasını çözebilirsiniz!