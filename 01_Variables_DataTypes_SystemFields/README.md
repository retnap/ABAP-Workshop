# 📘 01. Değişkenler & Karakter Veri Tipleri & Sistem Sabitleri (+ Request Kavramı)

## 🎯 Bu bölümde hangi konular olacak?
- ABAP'ta değişken tanımlama ve temel veri tipleri
- Karakter veri tiplerinin farkları: `C`, `STRING`, `N`, `X`
- Önemli **sistem sabitleri** (`sy-*`) ve ne işe yaradıkları
- Kısa not: **Request** (Transport Request / Paket) kavramı

---

## 🧩 Temel Kavramlar

### -Değişkenler ve Değişken Tanımlama-
Programlamada **değişlenler**, veriyi geçici olarak sakladığımız, program yürütülürken içeriği değiştirilebilen adlandırılmış bellek alanlarıdır. **Sabitler** ise program boyunca değeri değiştirilemeyen adlandırılmış bellek alanlarıdır. 

#### Anahtar Kelime (DATA)
ABAP'ta değişken tanımlamak için **DATA** anahtar kelimesi kullanılır. 

| Söz Dizimi (Syntax) | Açıklama | 
| ---- | ---- |
| `DATA <değişken_adı> TYPE <veri_tipi>. ` | Veri tipine göre standart uzunlukta değişken tanımlar.  | 
| `DATA <değişken_adı> TYPE <veri_tipi> LENGTH <uzunluk>. ` | Belirtilen uzunlukta değişken tanımlar (özellikle C ve N tipleri için).   | 
| `DATA <değişken_adı> TYPE <veri_tipi> VALUE <başlangıç_değeri>. `| Başlangıç değeri atayarak değişken tanımlar. |


* Değişken Tanımlama Şablonu
```abap
DATA:
  " General Variables (genellikle 'g' veya 'gv' ön eki kullanılır)
  gv_tarih  TYPE d,                  " Tarih (D-Date)
  gv_miktar TYPE p DECIMALS 2,       "Ondalıklı sayı (P - Packed Number)
  gv_sayac  TYPE i,                  " Tam sayı (I - Integer)

  " Local Variables (genellikle 'l' veya 'lv' ön eki kullanılır)
  lv_kullanici  TYPE c LENGHT 12,                   " Char
  lv_mesaj      TYPE string,                        " String
  lv_kontrol    TYPE abap_bool VALUE abap_false.    " Boolean
```

### -Sabit Tanımlama (CONSTANTS)-
Sabitler, program çalıştığı sürece değeri değişmeyecek veriler için kullanılır ve **CONSTANTS** anahtar kelimesi ile tanımlanır. **Tanımlama anında mutlaka bir başlangıç değeri atanmalıdır.**

#### Anahtar Kelime: CONSTANTS

| Söz dizimi (Syntax) | Açıklama |
| ---- | ---- |
| `CONSTANTS <sabit_adı> TYPE <veri_tipi> VALUE <değer>. ` | Değeri değiştirilemez sabit tanımlanır. | 

* CONSTANTS Tanımlama Şablonu
```abap
CONSTANTS:
   " Sabitler genellikle 'c' veya 'lc' ön eki kullanır
   lc_pi           TYPE p DECIMALS 5 VALUE '3.14159',    " PI Sayısı
   lc_maks_kayit   TYPE i VALUE 1000,                    " Maksimum Kayıt Limiti
   lc_sirket_kodu  TYPE c LENGTH 4 VALUE '1000'.         " Şirket Kodu
```

### -Karakter veri tipleri-
ABAP'ta metinsel ve karakter tabanlı verileri depolamak için kullanılan temel tipleridir. 

- `C` (Character): Sabit uzunluklu karakter alanı. `TYPE c LENGTH 10`.
- `STRING`: Değişken uzunluklu karakter dizisi.
- `N` (Numeric text): Sadece rakam karakterlerini tutar, digit count sabit.
- `X` (Hex): Binary/hex veri için.
- `D`, `T`: Tarih ve saat tipleri (`D`=YYYYMMDD, `T`=HHMMSS).

**Örnek:**
```abap
DATA:
  lv_kod_c(5)      TYPE c VALUE 'ABCDE',       " C tipi, 5 karakter uzunluğunda
  lv_tel_no(10)    TYPE n VALUE '5551234567',  " N tipi, 10 haneli (sadece rakam)
  lv_uzun_metin    TYPE string.                " STRING tipi (uzunluk belirtilmez)

lv_uzun_metin = 'Bu bir string ABAP metnidir.' .

WRITE: / 'C Karakter:', lv_kod_c,
       / 'N Sayısal Karakter:', lv_tel_no,
       / 'String Metin:', lv_uzun_metin.
```

---

## 🧠 Sistem Sabitleri (sy-)
`sy` yapısı SAP runtime hakkında bilgi verir. En sık kullanılanlar:

- `sy-subrc` : Son ABAP komutunun dönüş kodu. **0** (başarılı) 
- `sy-datum` : Uygulama sunucusunun mevcut tarihi (YYYYMMDD).  
- `sy-uzeit` : Uygulama sunucusunun mevcut saati (HHMMSS).  
- `sy-uname` : Oturumu açan kullanıcı adı.
- `sy-mandt` : Mevcut istemci (Client) numarası.  
- `sy-repid` : Şu anda çalışmakta olan programın adı/rep id.  
- `sy-tcode` : Transaction code (geçerli ise).  
- `sy-dbname` : Veritabanı adı.

**Kullanım örneği:**
```abap
WRITE: / '**************************************',
       / 'Sistem Bilgileri:',
       / '**************************************'.

WRITE: / 'Kullanıcı Adı    :', sy-uname,
       / 'İstemci No       :', sy-mandt,
       / 'Program Adı      :', sy-repid,
       / 'İşlem Kodu       :', sy-tcode.

WRITE: / 'Server Tarih/Saat:', sy-datum, sy-uzeit.

* SY-SUBRC Örneği (SELECT işleminden sonra kontrol etmek kritik öneme sahiptir)
SELECT SINGLE mandt FROM t000 INTO @DATA(lv_client) WHERE mandt = sy-mandt.
IF sy-subrc = 0.
  WRITE: / 'SELECT Başarılı (sy-subrc:', sy-subrc, ')'.
ELSE.
  WRITE: / 'SELECT Başarısız (sy-subrc:', sy-subrc, ')'.
ENDIF.
```

---

## 🔐 Request - Transport Request 
- ABAP geliştirmeleri, taşıma yönetimi (Change and Transport System - CTS) ile transport request (istek) içine alınıp sistemler arası taşınır.
- **Request number** genelde `KXXXXXX` veya `DEVKXXXXX` formatında olabilir; SAP sisteminde obje oluşturulduğunda (örn: Z table) sistem hangi request'e eklemek istendiğini sorar.
- **Package**: ABAP objelerini organize etme birimi. `LOCAL` (development class = $TMP) seçilirse obje transport edilmez.

> Not: Geliştirme yaparken `package` ve `transport request` seçimi önemlidir. Production’a taşımak için transport request gerekir.

---


## 🖼 Görseller

<img width="582" height="377" alt="01_Variable_Definition" src="https://github.com/user-attachments/assets/76293555-ca37-4131-9338-b3ed28730816" />

<img width="786" height="618" alt="02_Data_Types_and_System_Fields" src="https://github.com/user-attachments/assets/70539888-d99e-448d-aeaa-86e9b8e9a1d6" />

---

<img width="497" height="397" alt="03_Terminal" src="https://github.com/user-attachments/assets/e175adb2-d0af-48eb-9cca-4df7838bd31f" />



