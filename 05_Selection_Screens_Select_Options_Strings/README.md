# 📘 04. Selection Screen & Select Options & Strings

## Bu bölümde hangi konular olacak ? 

Bu bölümde, ABAP raporlarının kullanıcıdan giriş almasını sağlayan **Selection Screen* yapılarını, **Select Options** işlemlerini ve **STRING** manipülasyonlarını ele alacağız. 

---

# 🧩 SELECTION SCREEN (Seçim Ekranı) İşlemleri 

**Selection Screen**, bir ABAP raporu çalıştırıldığında kullanıcıdan parametre veya kriter girmesini sağlayan bir SAP arayüzüdür. 

## :fleur_de_lis: Tekil Değer Alımı: PARAMETERS

**PARAMETERS** komutu, Selection Screen'de kullanıcıdan tek bir değer (tek bir alan) almak için kullanılır. Alınan değer metin, tarih veya sayı biçiminde olabilir.

| Komut | Açıklama | Kullanımı | 
| --- | --- | --- |
| PARAMETERS | Tekil giriş alanı oluşturur. | PARAMETERS p_kunnr TYPE kunnr. |
| DEFAULT | Alan için varsayılan bir değer atar. | PARAMETERS p_tarih TYPE sy-datum DEFAULT sy-datum. |
| AS CHECKBOX | Boolean değer almak için onay kutusu oluşturur. | PARAMETERS p_test AS CHECKBOX. |
|RADIOBUTTON GROUP | Belirli bir grup içindeki seçeneklerden sadece birinin seçilebilmesini sağlar. | PARAMETERS r_opt1 RADIOBUTTON GROUP grp1 DEFAULT 'X'.|


## ⚜️ Aralık ve Çoklu Değer Alımı: SELECT-OPTIONS

**SELECT-OPTIONS** komutu, kullanıcıdan birden fazla değer, aralık veya karmaşık seçim kriterleri (örn. "şu değerden büyük, şu değerden küçük olmasın") almak için kullanılır. 

+ **Yapı:** **SELECT-OPTIONS** ile tanımlanan değişkenler aslında **Internal Table** yapısındadır. Bu tablo, dört ana alan içerir:
  + **SIGN** (**I** - Include / **E** - Exclude)
  + **OPTION** (EQ, BT, GT, LE, vb.)
  + **LOW** (Aralığın başlangıç değeri)
  + **HIGH** (Aralığın bitiş değeri)

```abap
* T001-BUKRS alanına göre çoklu giriş ekranı oluştur
SELECT-OPTIONS s_bukrs FOR t001-bukrs.

START-OF-SELECTION.
* SELECT-OPTIONS tablosunun içeriğini kontrol etme
  LOOP AT s_bukrs INTO DATA(ls_bukrs_line).
    WRITE: / 'Kriter (SIGN/OPTION/LOW/HIGH):',
             ls_bukrs_line-sign,
             ls_bukrs_line-option,
             ls_bukrs_line-low,
             ls_bukrs_line-high.
  ENDLOOP.
```


## ⚜️ Selection Screen Events 

### 🔖 Initialization 

Bu event program ilk yüklendiğinde, henüz ekran gösterilmeden önce ve sadece 1 defa çalışır. 

```abap
PARAMETERS: p_date TYPE sy-datum,
            p_name TYPE char20.

INITIALIZATION.
  p_date = sy-datum.
  p_name = 'ABC'.
```

### 🔖 At Selection Screen 

Kullanıcı girdiyi girdikten hemen sonra çalışır. 

```abap
PARAMETERS: p_name TYPE char20,
            p_age TYPE i.


AT SELECTION-SCREEN.
  " Yaş değerinin pozitif olduğunu doğrulama
  IF p_age < 0.
    MESSAGE 'Age must be a positive number' TYPE 'E'.
  ENDIF.

  " Yaş değerinin boş olmadığını doğrulama
  IF p_name IS INITIAL.
    MESSAGE 'Name can not be empty' TYPE 'E'.
  ENDIF.
```

### 🔖 At Selection Screen Output 

Bu event ekran gösterilmeden hemen önce ve ekran yenilendiğinde çalışır. Kullanıcı ekranla her işlem yaptığında yenilenme -refreshment- gerçekleşecektir. 

Bir diğer ifade ile bu event, selection screen görüntülenmeden önce ve kullanıcı ekranla iletişime girdiğinde ekranı yenilediği için, selection screen ekranını dinamik olarak değiştirmek için kullanılır. Böylece, seçim ekranının (selection screen) görünümünü ve davranışını dinamik olarak kontrol edebiliriz. 

**LOOP AT SCREEN** ile alanları gizleyebilir veya görüntüleyebilir, alan etiketlerini değiştirebilir veya belirli koşullara göre alanları zorunlu veya salt okunur hale getirebiliriz.


```abap
SELECTION-SCREEN BEGIN OF BLOCK b1 WITH FRAME TITLE text-001.
PARAMETERS: p_radio1 RADIOBUTTON GROUP rad1 DEFAULT 'X' USER-COMMAND UC,  " First radio button
            p_radio2 RADIOBUTTON GROUP rad1.  " Second radio button

PARAMETERS: p_chk1 AS CHECKBOX MODIF ID AA,  " First checkbox
            p_chk2 AS CHECKBOX MODIF ID BB,  " Second checkbox
            p_chk3 AS CHECKBOX MODIF ID BB.  " Third checkbox
SELECTION-SCREEN END OF BLOCK b1.

START-OF-SELECTION.

AT SELECTION-SCREEN OUTPUT.

  IF p_radio1 = 'X'.
    LOOP AT SCREEN.
      IF screen-group1 = 'BB'.
         screen-active = 0.
         MODIFY SCREEN.
      ENDIF.
    ENDLOOP.


  ELSEIF p_radio2 = 'X'.
    LOOP AT SCREEN.
      IF screen-group1 = 'AA'.
        screen-active = 0.
        MODIFY SCREEN.
      ENDIF.
    ENDLOOP.

  ENDIF.
```

### 🔖 At Selection Screen On 

**AT SELECTION-SCREEN ON <alan>** şeklinde kullanılır ve kullanıcı ilgili alandan çıkıp enter/f8 yaptığında yalnızca belirtilen alan için tetiklenir. 

Bu event sayesinde:

* Sadece bir parametre veya

* Sadece bir select-options alanı

üzerinde kontrol veya doğrulama yapılabilir. 

```abap
AT SELECTION-SCREEN ON p_age.

IF p_age < 0.
  MESSAGE 'Yaş negatif olamaz!' TYPE 'E'.
ENDIF.
```

**E tipi mesaj**, kullanıcıyı selection screen'e geri döndürür. Böylece kullanıcı hatayı düzeltmek zorunda kalır.

---

```abap
AT SELECTION-SCREEN ON s_matnr.

IF s_matnr[] IS INITIAL.
  MESSAGE 'Malzeme alanı boş bırakılamaz!' TYPE 'E'.
ENDIF.
```

Burada kontrol sadece **s_mantr** alanından çıkıldığında yapılır. 

---

```abap
AT SELECTION-SCREEN ON r_a.

IF r_a = '' AND r_b = ''.
  MESSAGE 'Bir seçim yapmak zorunludur!' TYPE 'E'.
ENDIF.
```

Radio button grupları tek alan gibi çalışmaz. Bu nedenle tüm grup için **teki bir event** yazmak gerekir. “ON r_a” yazmak tüm grubun kontrol edilmesi için yeterlidir.

---

```abap
AT SELECTION-SCREEN ON p_start.
  IF p_start < '20240101'.
    MESSAGE 'Başlangıç tarihi sistemden eski olamaz' TYPE 'E'.
  ENDIF.

AT SELECTION-SCREEN ON p_end.
  IF p_end < p_start.
    MESSAGE 'Bitiş tarihi başlangıç tarihinden önce olamaz' TYPE 'E'.
  ENDIF.
```

Farklı alanlar için farklı kontrol blokları yazılabilir.

---

```abap
AT SELECTION-SCREEN ON p_matnr.

SELECT SINGLE matnr
  FROM mara
  WHERE matnr = @p_matnr.

IF sy-subrc <> 0.
  MESSAGE 'Bu malzeme numarası sistemde bulunamadı!' TYPE 'E'.
ENDIF.
```

SAP projelerinde en sık kullanılan pattern üstteki gibidir. Bu sayede yanlış veri girişleri rapor çalıştırılmadan engellenir.


--- 


# 🧩 STRINGS

## ⚜️ STRLEN 

Hedeflenen string'in uzunluğunu bulma.

```abap
DATA(text) = 'Hello World'.
DATA(len) = strlen( text ).

WRITE:/ 'Uzunluk: ', len.
```

## ⚜️ CONDENSE 

Metin içindeki birden fazla boşluğu teke düşürür. 

**NO-GAPS** kullanılırsa tüm boşluklar tamamen silinir.

```abap
DATA: text TYPE string VALUE 'ABAP   Egitim    Serisi'.

CONDENSE text.
" Sonuç: 'ABAP Egitim Serisi'

CONDENSE text NO-GAPS.
" Sonuç: 'ABAPEgitimSerisi'
```

## ⚜️ FIND 

Bir substring'in bulunduğu pozisyonu verir.

```abap
DATA: text TYPE string VALUE 'ABAP Egitim',
      pos TYPE i.

FIND 'Egitim' IN text MATCH OFFSET pos.

" pos = 5 döner çünkü 'E' harif 5. indexte bulunuyor
```

## ⚜️ TRANSLATE

Metni büyük/küçük harfe çevirir veya karakter dönüştürmesi yapar.

```abap
DATA: text TYPE string VALUR 'Abap Egitim'.

TRANSLATE text TO UPPER CASE.
" Sonuç: 'ABAP EGITIM'

TRANSLATE text TO LOWER CASE.
" Sonuç: 'abap egitim'
```

## ⚜️ SHIFT 

Metni sola veya sağa kaydırır. 

**LEFT**, **RIGHT**, **CIRCULAR**, **DELETING LEADING** gibi ekleri vardır.

```abap
DATA: text TYPE string VALUE '    ABAP'.

SHIFT text LEFT DELETING LEADING SPACE.
" Sonuç: 'ABAP'
```

## ⚜️ SPLIT

Bir metni ayırma karakterine göre böler ve parçlara atar.

```abap
DATA: text TYPE string VALUE 'ABAP;Egitim;Serisi'.
DATA: p1 TYPE string,
      p2 TYPE string,
      p3 TYPE string.

SPLIT text AT ';' INTO p1 p2 p3.

" p1 = 'ABAP'
" p2 = 'Egitim'
" p3 = 'Serisi'
```

## ⚜️ STRING COMPRASION 

**< > <= >= = <>** operatörleri stringlerde doğrudan çalışır.

```abap
DATA: t1 TYPE string VALUE 'ABAP',
      t2 TYPE string VALUE 'ABAP'.

IF t1 = t2.
  WRITE:/ 'Ayni Metin'.
ENDIF.
```

## ⚜️ CONTAINS NOT ONLY

Metin yalnızca belirtilen karakterlerden oluşmuyorsa TRUE döner. 

```abap
DATA: text TYPE string VALUE 'ABC123'.

IF text CONTAINS NOT ONLY 'ABC'.
  WRITE:/ 'Sadece ABC degil, baska karakterler de var'.
ENDIF.
```

## ⚜️ CONTAINS ANY (CA)

Metin belirtilen karakterleden en az birini içeriyorsa TRUE döner. 

```abap
DATA: text TYPE string VALUE 'ABAP2025'.

IF text CONTAINS ANY '0123456789'.
  WRITE 'Sayisal karakter iceriyor'.
ENDIF.
```

## ⚜️ CONTAINS NOT ANY 

Metin belirtilen karakterlerden hiçbirini içermiyorsa TRUE döner.

```abap
DATA: text TYPE string VALUE 'ABAP'.

IF text CONTAINS NOT ANY '0123456789'.
  WRITE:/ 'Sayi icermiyor.'.
ENDIF.
```

## ⚜️ CONTAINS STRING

Metin içinde substring geçiyor mu kontrol eder.

```abap
DATA: text TYPE string VALUE 'ABAP Egitim'.

IF text CONTAINS STRING 'Egitim'.
  WRITE 'Bulundu'.
ENDIF.
```

## ⚜️ CONTAINS NO STRING

Metin içinde belirtilen substring yoksa TRUE olur. 

```abap
DATA: text TYPE string VALUE 'ABAP Egitim'.

IF text CONTAINS NO STRING 'Java'.
  WRITE 'Java gecmiyor'.
ENDIF.
```
