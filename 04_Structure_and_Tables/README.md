# 📘 03. Structure & Tables 

## Bu bölümde hangi konular olacak ? 

Bu bölümde ABAP'ın database işlemlerinin yapı taşlarını oluşturan **structure** ve **table** yapılarını işleyeceğiz. 

Amaç: 

---

# 🧩 STRUCTURE

**Structure (Yapı)**, ABAP'ta birden fazla alanı (field) tek bir veri tipi altında toplamak için kullanılan Data Dictionary (SE11) nesnesidir. 

Bir structure içerisinde **alan isimleri (field names)**, **veri tipleri (data types)** ve **uzunluklar (lengths)** yer alır; fakat **veri saklanmaz**.

Önemli ve hatırlanması gereken noktaları maddeler halinde verecek olursak:

+ Bir structure **TABLO değildir**, veri tutmaz.

+ Ama "tek satırlık tablo şeması" gibi düşünülebilir.

+ Programlarda ve başka kaynaklarda **work area** adıyla da geçmektedir.

## 📜 Structure Türleri 

### A. Flat Structure

Düz, alt alan içermeyen, doğrudan alan listesi bulunan yapıdır. 

### B. Nested Structure 

Bir structure'ın içinde başka bir structure kullanılması. 

### C. Table Type ile Kullanılan Structure 

Bir structure'ı temel alarak internal table oluşturma işlemidir.

```abap
TYPES: BEGIN OF ty_employee,
        id TYPE i,
        name TYPE string,
        dept TYPE char10,
      END OF ty_employee.

DATA: lt_emp TYPE TABLE OF ty_employee,
      ls_emp TYPE ty_employee.
```


## 📜 Structure Nasıl Oluşturulur ? 

### 1. SE11 transaction kodu üzerinden 

1. SE11 -> Data Type -> Create
2. Type = Structure
3. Alanları doldur:
   * Field Name
   * Data Type
   * Data Element
   * Length
   * Short Description
4. Save -> Activate 

### 2. TYPES ile Yeni Bir Structure Tipi Tanımlamak

Bu yöntemi, genellikle bir Internal Table veya Metot için bir veri tipi tanımlaması yapmak istediğimizde kullanırız. Önce tipi tanımlar, sonra bu tipi kullanarak **DATA** ile değişken tanımlarız. 

```abap
" 1. TYPES ile yeni bir local structure tipi tanımlama
TYPES: BEGIN OF ts_calisan_veri,
        sicil_no   TYPE c LENGTH 8,
        ad         TYPE c LENGTH 25,
        soyad      TYPE c LENGTH 30,
        maas       TYPE p DECIMALS 2,
    END OF ts_calisan_veri.

" 2. Tanımlanan tipi kullanarak DATA ile bir structure tanımlama
DATA: gs_calisan_kayit TYPE ts_calisan_veri. 
```

### 3. DATA ile Doğrudan Structure Tanımlamak 

Daha az karmaşık veya program içinde bir defa kullanılacak yapılar için doğrudan **DATA** bloğu içinde tanımlama yapılabilir. 

```abap
" DATA ile bir Structure değişkeni tanımlama
DATA: BEGIN OF gs_musteri_bilgi,
        musteri_id    TYPE c LENGTH 10,
        sirket_ad     TYPE c LENGTH 50,
        ulke          TYPE c LENGTH 3,
      END OF gs_musteri_bilgi. 
```


## 📜 Structure Alanlarına Erişim 

Structure alanlarına erişim, alan adından önce yapının adını ve araya bir tire işareti (-) koyarak yapılır. 

```abap
"gs_calisan_kayit structure'ına değer atama 
gs_calisan_kayit-sicil_no = '0001234'.
gs_calisan_kayit-ad       = 'Ahmet'.
gs_calisan_kayit-soyad    = 'Yılmaz'.
gs_calisan_kayit-maas     = '15000.50'.

" Değerleri Ekrana Yazdırma
RITE: / 'Sicil No   :', gs_calisan_kayit-sicil_no,
       / 'Tam Adı    :', gs_calisan_kayit-ad, gs_calisan_kayit-soyad,
       / 'Maaş       :', gs_calisan_kayit-maas CURRENCY 'EUR'.

" Bütün bir yapıyı sıfırlama (Initial Value)
CLEAR gs_calisan_kayit. 
```


---

# INTERNAL TABLES 

## :scroll: Internal Table Nedir ? 

ABAP Programının RAM belleğinde çalışan geçici tablolardır. Database'de saklanmazlar. 

+ Structure (work area) ile işlem yapılır.

+ Loop döngüsü ile okunur.

+ Append, Insert, Modify, Delete işlemleri yapılabilir.

+ ALV raporlamada, SELECT sonuçlarında, hesaplamalarda kullanılır.

## :scroll: Internal Table Türleri 

### A. Standard Table 

+ En çok kullanılan tablo türüdür.

+ Satırlar index ile erişilebilir.

+ Append ile sona eklenir.

+ Sıralı arama algoritması ile çalışır. 

+ Syntax:

```abap
DATA: lt_tab TYPE STANDARD TABLE OF ty_row. 
```

### B. Sorted Table 

+ Tablonun satırları anahtar alanlara göre otomarik sıralıdır.

+ **Binary Search** arama algoritması ile çalışır.

+ Syntax:

```abap
DATA: lt_tab TYPE SORTED TABLE OF ty_row WITH UNIQUE KEY field. 
```

### C. Hashed Table 

+ **Hash Algoritaması**nı kullanır.

+ Satır arama işlemi O(1) hızında yapılır (en hızlısı).

+ Yalnızca **key** ile erişilebilir.

+ LOOP ile satır satır okumak yavaştır.

+ Syntax:

```abap
DATA: lt_tab TYPE HASHED TABLE OF ty_row WITH UNIQUE KEY field. 
```

## 📜 Internal Table İşlemleri 

Aşağıda yapacağımız örneklerde aynı structure'ı kullanacağız:

```abap
TYPES: BEGIN OF ty_student,
         id    TYPE i,
         name  TYPE string,
         score TYPE i,
       END OF ty_student.
```

### A. Kayıt Eklemek (APPEND)

```abap
DATA: lt_std TYPE TABLE OF ty_student,
      ls_std TYPE ty_student.

ls_std-id = 1.
ls_std-name = 'Ali'.
ls_std-score = 90.
APPEND ls_std TO lt_std.

CLEAR ls_std.

ls_std-id = 2.
ls_std-name = 'Veli'.
ls_std-score = 85.
APPEND ls_std TO lt_std.
```


### B. Kayıt Okuma (LOOP)

```abap
LOOP AT lt_std INTO ls_std.
    WRITE: / ls_std-id, ls_std-name, ls_std-score.
ENDLOOP.    
```

### C. READ TABLE (Tek Kayıt Bulma)

Index ile:

```abap
READ TABLE lt_std INTO ls_std INDEX 1.
```

Key ile:

```abap
READ TABLE lt_std WITH KEY id = 2.
```

### D. MODIFY (Mevcut Kaydı Güncelleme)

```abap
READ TABLE lt_std INTO ls_std WITH KEY id = 2.

IF sy-subrc = 0.
  ls_std-score = 95.
  MODIFY lt_std FROM ls_std.
ENDIF.
```

### E. DELETE (Kayıt Silme)

```abap
DELETE lt_std WHERE id = 1. 
```

### F. INSERT (Belirli Index'e Kayıt Ekleme)

```abap
INSERT ls_std INTO lt_std INDEX 1. 
```



