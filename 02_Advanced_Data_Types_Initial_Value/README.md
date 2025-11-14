# 📘 02. Data Types (Advanced) & Types & Initial Values 

## 🎯 Bu bölümde hangi konular olacak? 

Bu bölümde, ABAP'ta veri tiplerini daha detaylı olarak ele alacağız. Temel tiplerin ötesine geçerek:
- Domain kullanım mantığı
- TYPES komutu ile tip tanımlama 
- Initial Value kavramı 
- Pointer tip değişkenler üzerinde durulmuştur. 

Amaç: Veri yapılarının bellekte nasıl davrandığını anlamak, kendi veri tiplerimizi oluşturmak ve veri tiplerinin başlangıç değerlerinin nasıl verildiğini kavramaktır. 

---

## 🧩 Domain Veri Tipi Nedir ? 

Domain, ABAP DATA Dictionary'de bir alanın teknik özelliklerini tanımlamak için kullanılır. 
Bir domain aşağıdaki bilgileri içerir: 
+ Veri tipi (CHAR, NUMC, INT4...)
+ Alan uzunluğu
+ Alt/üst değer aralıkları (value range)

### Domain Kullanım Amacı 
+ Tekrarlı tip tanımlarını ortadan kaldırmak
+ Sistem içinde standartlaşmayı sağlamak
+ Bir alanın tüm teknik özelliklerini tek noktadan yönetmek

### Örnek Domain Oluşturma 
SE11 -> Domain -> CHAR* -> F4
     -> Domain -> CHAR* -> F4 şeklinde tüm liste görülebilir. 

Oluşturulan domain daha sonra bir **Data Element** içinde kullanılarak tüm tablolar için aynı yapı sağlanabilir. 

**Not:** Temel veri tiplerinden domain, domain'lerden data type'lar, data type'lardan veri tabanı tablo değişkenleri 
oluşmuştur. 

--- 

## 🧩 TYPES Komutu ile Tip Tanımlama 

ABAP'ta **TYPES** komutu ile kendi veri tiplerimizi oluşturabiliriz:
+ Type
+ Structure
+ Table Type şeklinde olabilir.

**Not:** Type'ların kullanım amacı değişkenlerde kullanılacak veri tiplerini oluşturmaktır. 


### TYPE Tanımlama Örneği 
```abap
TYPES: vg0235 TYPE c LENGTH 35.   " c tipinde 35 karakter uzunluğunda vg0235 adında değişkenlerde kullanılacak veri tipi oluşturuldu.
DATA: gv35 TYPE vg0235.           " oluşturulan veri tipi (data type) değişken için kullanıldı. 
```

```abap
TYPE: sayi TYPE i.
DATA: toplam LIKE sayi.           " toplam, sayi gibi bir değişkendir 
```

### STRUCTURE Tanımlama Örneği 
```abap
TYPES: BEGIN OF ty_employee,
          id     TYPE i,
          name   TYPE string,
          salary TYPE p DECIMALS 2,
        END OF ty_employee.

DATA: ls_employee TYPE ty_employee.

" ty_employee tipinde bir structure (ls_employee) oluşturuldu. 
```

### INTERNAL TABLE TYPE Tanımlama Örneği 
```abap
TYPES: ty_t_employee TYPE STANDARD TABLE OF ty_employee.

DATA: lt_emp TYPE ty_t_employee.
```

--- 


## 🧩 Initial Value (Başlangıç Değeri) 
ABAP'ta her data type otomatik olarak bir initial (varsayılan) değer alır.
Initial değerler veri tipine göre değişir.

### Sayısal Tipler 
| Data Type | Açıklama | Initial Value |
| ---- | ---- | ---- | 
| INT1 | 1 byte int | 0 |
| INT2 | 2 byte int | 0 |
| INT4(i) | 4 byte int | 0 |
| INT8 | 8 byte int (HANA) | 0 |
| p | packed number | 0 |
| f | float | 0.000000000 |


```abap
DATA: lv_i1 TYPE int1,
      lv_i2 TYPE int2,
      lv_i4 TYPE int4,
      lv_i8 TYPE int8.

WRITE: / 'INT1 Initial:', lv_i1,
       / 'INT2 Initial:', lv_i2,
       / 'INT4 Initial:', lv_i4,
       / 'INT8 Initial:', lv_i8.
```

### Karakter Veri Tipleri 
| Tip | Initial Değer | 
| ---- | ---- | 
| CHAR | ' ' (boşluk) |
| STRING | "" (null string) | 
| D (date) | '00000000' |
| T (time) | '000000' | 
| n | '0' (uzunluğu kadar 0, 11 tane 0) |  


```abap
DATA: lv_name TYPE c(20).
      lv_text TYPE string.

WRITE: / 'CHAR Initial:', lv_name.
      / 'STRING Initial:', lv_text.
```

**Not:** Değişken tanımlanırken, ***value*** ile intial değeri verilebilir. 

**Not:** **clear** ile değişken initial değerine geri döner. 

```abap
DATA: id(11) TYPE n VALUE '321654987'.
id = '9876543210'.
WRITE:/ 'ID......:', id.
CLEAR id. 
```

## 🧩 Pointer Tip Değişken (Data Reference)
ABAP'ta pointer mantığı **data reference** ile sağlanır. 

Pointer, bir değişkenin bellekteki adresini ifade eder. 

### Pointer (Data Reference) Oluşturma 
```abap
DATA: lv_text TYPE c LENGTH 10 VALUE 'Hello!'.

FIELD-SYMBOLS <hex> TYPE x.

ASSIGN text TO <hex> CASTING.

WRITE <hex>.  
```


## 🖼 Görseller

### TYPES 
<img width="648" height="198" alt="01-Basic-Type" src="https://github.com/user-attachments/assets/10245df5-964a-4f30-952a-e26d2ae3101a" />

### STRUCTURE 
<img width="566" height="437" alt="02-Structure" src="https://github.com/user-attachments/assets/d563c8d8-d667-4965-af3f-63f02ae36457" />

<img width="272" height="75" alt="03_Structure_Print" src="https://github.com/user-attachments/assets/a17b63cf-0a0a-47c7-906f-cc0c25518f41" />

### Internal Table 
<img width="697" height="636" alt="04_Internal_Table" src="https://github.com/user-attachments/assets/73b4bca3-60bf-4545-a26c-19e648d9d692" />

### Pointer
<img width="847" height="270" alt="05_Pointer" src="https://github.com/user-attachments/assets/0526a6a1-60c3-4bec-ada3-278184227989" />

<img width="542" height="60" alt="06_Pointer_Print" src="https://github.com/user-attachments/assets/36cdb4a3-153c-4abc-8133-67b5bbca17dd" />



