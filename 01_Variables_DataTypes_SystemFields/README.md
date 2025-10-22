# 📘 01. Değişkenler & Karakter Veri Tipleri & Sistem Sabitleri (+ Request Kavramı)

## 🎯 Bu bölümde hangi konular olacak?
- ABAP'ta değişken tanımlama ve temel veri tipleri
- Karakter veri tiplerinin farkları: `C`, `STRING`, `N`, `X`
- Önemli **sistem sabitleri** (`sy-*`) ve ne işe yaradıkları
- Kısa not: **Request** (Transport Request / Paket) kavramı

---

## 🧩 Temel Kavramlar

### Değişken tanımlama
ABAP'ta değişkenleri `DATA`, `CONSTANTS` veya `TYPES` ile tanımlarız.
```abap
DATA: lv_ctr TYPE i,          " integer
      lv_name TYPE string,    " dinamik string
      lv_flag TYPE abap_bool. " boolean (VALUE 'X' or space)
```

### Karakter veri tipleri
- `C` (Character): Sabit uzunluklu karakter alanı. `TYPE c LENGTH 10`.
- `STRING`: Değişken uzunluklu karakter dizisi.
- `N` (Numeric text): Sadece rakam karakterlerini tutar, digit count sabit.
- `X` (Hex): Binary/hex veri için.
- `D`, `T`: Tarih ve saat tipleri (`D`=YYYYMMDD, `T`=HHMMSS).

**Örnek:**
```abap
DATA: c_fixed TYPE c LENGTH 10,
      v_string TYPE string,
      n_num TYPE n LENGTH 4,
      x_bin TYPE x LENGTH 16.
```

---

## 🧠 Sistem Sabitleri (sy-)
`sy` yapısı SAP runtime hakkında bilgi verir. En sık kullanılanlar:

- `sy-datum` : Sistem tarihi (YYYYMMDD).  
- `sy-uzeit` : Sistem saati (HHMMSS).  
- `sy-uname` : Oturumu açan kullanıcı adı.  
- `sy-repid` : Şu anda çalışmakta olan programın adı/rep id.  
- `sy-tcode` : Transaction code (geçerli ise).  
- `sy-dbname` : Veritabanı adı.

**Kullanım örneği:**
```abap
WRITE: / 'Today:', sy-datum, 'Time:', sy-uzeit.
WRITE: / 'User:', sy-uname, 'Program:', sy-repid.
```

---

## 🔐 Request - Transport Request & Paket (kısa)
- ABAP geliştirmeleri, taşıma yönetimi (Change and Transport System - CTS) ile transport request (istek) içine alınıp sistemler arası taşınır.
- **Request number** genelde `KXXXXXX` veya `DEVKXXXXX` formatında olabilir; SAP sisteminde obje oluşturduğunda (ör: Z table) sistem senden hangi request'e eklemek istediğini sorar.
- **Package**: ABAP objelerini organize etme birimi. `LOCAL` (development class = $TMP) seçersen obje transport edilmez.

> Not: Geliştirme yaparken `package` ve `transport request` seçimi önemlidir. Production’a taşımak için transport request gerekir.

---

## 🧾 Kod Şablonları (Hemen kullanabileceğin kısa örnekler)

### Değişken örneği
```abap
REPORT z_demo_variables.

DATA: lv_count TYPE i VALUE 0,
      lv_text  TYPE string.

lv_count = 5.
lv_text = |Bugün: { sy-datum } { sy-uzeit }|.

WRITE: / lv_count, lv_text.
```

### Karakter tipleri örneği
```abap
REPORT z_char_types.

DATA: c_name TYPE c LENGTH 20,
      s_desc TYPE string,
      n_code TYPE n LENGTH 4.

c_name = 'Selman'.
s_desc = 'ABAP Workshop — Karakter tipleri'.
n_code = '0123'.

WRITE: / c_name.
WRITE: / s_desc.
WRITE: / n_code.
```

---

## 🖼 Görseller




