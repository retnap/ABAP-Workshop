# 📘 04. Structure & Tables 

## Bu bölümde hangi konular olacak ? 

Bu bölümde ABAP'ın database işlemlerinin yapı taşlarını oluşturan **structure** ve **table** yapılarını işleyeceğiz. 

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

# 🗃️ INTERNAL TABLES 

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

--- 

# 🗃️ Database Tables 

ABAP’ın temel taşlarından biri veri sözlüğü **(ABAP Dictionary)** ve burada tanımlanan Database Table yapılarıdır. Bu tablolar SAP sistemindeki fiziksel verilerin saklandığı yapılardır. 

**SE11** transaction kodu üzerinden tablo oluşturup hem teknik ayarlarını hem de program içinden veri işlemlerini yönetebiliriz.

## 📜 Database Table Nedir ? 

Database Table, SAP sisteminde fiziksel olarak HANA/OpenSQL uyumlu şekilde saklanan tablolardır.

ABAP Dictionary’de oluşturulan tablo:

+ Veri tipleri ve alanları içerir.

+ Primary Key’e sahiptir.

+ Uygulama kodu tarafından SELECT / INSERT / UPDATE / DELETE işlemleriyle kullanılır.


### -Alanlar (Fields)-

* Tablodaki sütunları temsil eder. 

* Her alanın bir adı ve bir veri tipi vardır.

* ABAP geliştiricileri, bir alanın tipini tanımlarken genellikle yerleşik ABAP tipleri yerine **Data Elements** denilen veri öğelerini kullanır.


### -Data Elements (Veri Öğeleri)-

Data Element, bir alanın **anlamsal (semantic)** özelliklerini tanımlar.

+ **Veri Tipi ve Uzunluk:** Alanın temel ABAP veri tipini ve uzunluğunu belirtir.

+ **Etiketler:** Ekranlarda ve raporlarda alanın başlıkları (short, medium, long) bu data element tarafından karşılanır.

+ Data Elementler, tekrar kullanılabilir yapıdadır. Farklı tablolarda çeşitli alanlara atamaları yapılabilir.

### -Domain'ler (Etki Alanları)-

Domain, bir alanın **teknik** özelliklerini ve kabul edebileceği değer aralığını tanımlar. 

Bu teknik özellikler arasında data type, length ve input checks gösterilebilir.

Domainler, data element oluşturmada kullanılabilir ve field alanların özelliklerini detaylandırmada kullanılabilir. 

### -Anahtar Alanlar ve İlişkiler- 

Veritabanı tablolarının en önemli yönü, **Anahtar Alanlar (Key Fields)** ve bunlar aracılığıyla kurulan ilişkilerdir. 

#### :key: Primary Key (Birincil Anahtar)

+ Bir tablodaki bir satırı benzersiz şekilde tanımlayan bir veya daha fazla alan grubudur.

+ SAP tablolarında birincil anahtar genellikle ilk alan olan **Client (İstemci - MANDT)** ile başlar.

+ Birincil anahtar alanları, indexleme (veri erişimi) için optimize edilmiştir.

#### 🗝️ Foreign Key (Yabancı Anahtar) İlişkisi 

+ İki tablo arasındaki bağlantıyı kurar.

+ Bir tablodaki alanın (yabancı anahtar), başka bir tablodaki (kontrol tablosu) birincil anahtar alanına referans vermesidir.

+ **Amaç:** Veri tutarlılığını sağlamaktır. Örneğin, bir satış belgesinde kullanılan Malzeme Numarasının (VBAP), MARA tablosunda gerçekten var olup olmadığını kontrol eder.


#### Indexes (İndeksler) 

İndeksler, tabloda hızlı veri erişimi sağlamak için kullanılan ek veri yapılarıdır. 

+ **Birincil İndeks:** Tablonun birincil anahtar alanlarından otomatik olarak oluşturulur.

+ **İkincil İndeks:** Performansı artırmak için birincil anahtara dahil olmayan alanlar üzerinde manuel olarak oluşturulur.


**⚠️ NOT:** SAP kendi database tabloları üzerinde sadece **SELECT** işlemine izin verir. 


### SELECT Komutunun Kullanımı 

```abap
* TOO1: Şirket Kodları Tablosu
DATA: lt_sirketler TYPE STANDARD TABLE OF t001,   " Internal Table
      ls_sirket TYPE t001.                        " Structure

* SELECT komutu ile T001'den 1000 ve 2000 şirket kodlarını çekme
SELECT bukrs, butxt, ort01              " Sadece ihtiyaç olan alanlar
      FROM t001        
      INTO TABLE @lt_sirketler          " Veriyi doğrudan Internal Table'a aktar
      WHERE bukrs IN ('1000', '2000').

* Çekilen veriyi işleme
IF sy-subrc = 0.
   LOOP AT lt_sirketler INTO ls_sirket.
        WRITE: / ls_sirket-bukrs, ls_sirket-butxt, ls_sirket-ort01.
   ENDLOOP.
ENDIF.  
```

### INSERT Komutu (Kayıt Ekleme)

```abap
DATA: ls_student TYPE zstudent.

ls_student-student_id = '0000000010'.
ls_student-firstname = 'Mehmet'.
ls_student-lastname = 'Kaya'.
ls_student-age = 22.
ls_student-city = 'ISTANBUL'.

INSERT zstudent FROM @ls_student.
IF sy-subrc = 0.
  WRITE: 'Record inserted successfully'.
ENDIF.
```

### UPDATE İşlemi (Kayıt Güncelleme)

```abap
UPDATE zstudent
  SET age = 25
  WHERE student_id = '0000000010'.

IF sy-subrc = 0.
  WRITE: 'Record updated'.
ENDIF.
```

### DELETE İşlemi (Kayıt Silme)

```abap
DELETE FROM zstudent
  WHERE student_id = '0000000010'.

IF sy-subrc = 0.
  WRITE: 'Record deleted'.
ENDIF.
```

### MODIFY (Hem Insert Hem Update)

```abap
MODIFY zstudent FROM @ls_student.
```

