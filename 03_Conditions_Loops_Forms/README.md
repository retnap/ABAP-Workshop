# 📘 03. Conditions & Loops & Forms 

## Bu bölümde hangi konular olacak ? 

Bu bölümde ABAP'ta sıkça kullanılan **koşul ifadeleri**, **döngüler** ve programların daha düzenli hale gelmesini sağlayan **PERFORM - FORM** yapılarını ele alacağız. 

Amaç: Koşul ifadeleri ile karar verme mekanizmaları oluşturmak, döngüler ile tekrarlayan işlemleri yönetmek ve forms ile kod modülerizasyonunu sağlamaktır. 

---

## 🧩 Conditions (Koşul İfadeleri)

Koşul ifadeleri, program akışına göre belirli bir şartın doğru olup olmadığını kontrol edilmesi sonucuna göre farklı kod bloklarının çalıştırılmasını sağlar. ABAP'ta en yaygın koşul ifadeleri şunlardır:

### -IF... ENDIF Yapısı-
   
Bu yapı, çoğu programlama dilinde olduğu gibi en temel karar verme mekanizmasıdır. Bir veya birden fazla koşulu kontrol etmektedir.

+ **Temel Kullanım:** Eğer koşul (condition) doğruysa (TRUE), **IF** ve **ENDIF** arasındaki kod çalışır.

+ **Alternatif Koşul (ELSEIF):** İlk koşul yanlışsa, ikinci bir koşulu kontrol etmek için kullanılır. İhtiyaca göre istenildiği kadar ELSEIF eklenebilir.

+ **Varsayılan Durum (ELSE):** Yukarıda bahsedilen koşulların hiçbiri doğru değilse çalışacak olan kod ELSE altında yazılan kodlardır.  


```abap
DATA: lv_sayi TYPE i.

lv_sayi = 15.

IF lv_sayi = 10.
  WRITE:/ 'Sayi degeri: ', lv_sayi.

ELSEIF lv_sayi > 10.
  WRITE:/ 'Sayi degeri 10''dan buyuk.'.

ELSEIF lv_Sayi < 10.
  WRITE:/ 'Sayi degeri 10''dan kucuk.'.

ELSE.
  WRITE:/ 'Gecerli sayi degeri girin.'.

ENDIF.
```


```abap
DATA: lv_age TYPE i VALUE 20,
      lv_result TYPE string.

IF lv_age < 18.
  lv_result = 'Resit degil.'.

ELSEIF lv_age >= 18 AND lv_age < 65.
  lv_result = 'Calisabilir yas araliginda.'.

ELSE.
  lv_result = 'Emeklilik yasinda.'.

ENDIF.

WRITE:/ lv_result.
```


### -CASE... ENDCASE Yapısı- 

Bazı durumlarda birden fazla farklı değeri tek bir değişken için kontrol etmek gerektiğinde CASE yapısı, iç içe IF-ELSEIF kullanmaktan daha verimli bir alternatif olabilir. 

+ **Kontrol Edilen ALan:** CASE ifadesinden sonra belirtilen alanın değeri, WHEN ifadeleriyle karşılaştırılır.

+ **Değer Eşleşmesi(WHEN):** Eğer alanın değeri WHEN ile belirtilen değere eşitse, o blok çalışır.

+ **Varsayılan Durum (WHEN OTHERS): Hiçbir WHEN koşulunun sağlanmaması durumunda çalışacak kod bloğunu belirtir.


```abap
DATA lv_renk TYPE c LENGTH 1 VALUE 'M'.

CASE lv_renk.
  WHEN 'K'.
    WRITE:/ 'Kırmızı'.
  WHEN 'M'.
    WRITE:/ 'Mor'.
  WHEN OTHERS.
    WRITE:/ 'Bilinmeyen Renk'.
ENDCASE. 
```

```abap
DATA lv_day TYPE i VALUE 2.
DATA lv_name TYPE string.

CASE lv_day.
  WHEN 1.
    lv_name = 'Pazartesi'.
  WHEN 2.
    lv_anme = 'Salı'.
  WHEN 3.
    lv_name = 'Çarşamba'.
  WHEN 4.
    lv_name = 'Perşembe'.
  WHEN OTHERS.
    lv_name = 'Geçerli bir gün girilmedi'.
ENDCASE.

WRITE:/ lv_name. 
```

--- 

## 🧩 Loops (Döngüler)

Döngüler, aynı kod bloğunu belirlenen koşul sağlanıncaya kadar veya belirli bir veri kümesi boyuca tekrar tekrar çalıştırmak için kullanılır. 

### 1. DO...ENDDO Yapısı 

Belirli bir sayıda veya koşul doğru olduğu sürece tekrarlamak için kullanılan genel döngü yapısıdır. 

+ **Belirli Sayıda Tekrar:** **DO N TIMES.** biçiminde kullanılır ve döngünün **N** kez çalışmasını sağlar.

+ **Koşula Bağlı Tekrar:** Koşul içinde bir **EXIT** veya **CONTINUE** ifadesi ile sonsuz bir döngü oluşturulup, içerideki bir IF kontrolüyle kırılabilir.

```abap
DATA gv_i TYPE i.

DO 5 TIMES.
  gv_i = sy-index.     " sy-index, mevcut iterasyon numarasını tutar (1'den başlar)
  WRITE:/ 'Iterasyon:', gv_i.
ENDDO. 
```

### 2. WHILE... ENDWHILE Yapısı 

Belirtilen bir koşul doğru olduğu sürece çalışır. 

+ **Koşul Kontrolü:** Döngüye her girildiğinde belirlenen koşul kontrol edilir. Koşul yanlış hale geldiği anda döngü durur.

```abap
DATA gv_sayac TYPE i VALUE 1.

WHILE gv_sayac <= 3.
  WRITE:/ 'Sayac: ', gv_sayac.
  gv_sayac = gv_sayac + 1.      " Sayaç artırılmazsa sonsuz döngü oluşur.
ENDWHILE. 
```


### 3. LOOP AT... ENDLOOP Yapısı 

ABAP'ta en sık kullanılan döngü ve en önemli döngü yapısıdır. Bunun sebebi Internal Table adı verilen veri koleksiyonları üzerindeki her satırı tek tek işlemek için kullanılmasıdır. 

+ **Satır İşleme:** Her iterasyonda, iç tablonun bir satırı belirlenen bir **Structure** veya **Filed Symbol** içine aktarılır ve kod o satır üzerinde işlem yapar.

```abap
TYPES: BEGIN OF ty_person,
        name TYPE string,
        age TYPE i,
      END OF ty_person.

DATA: lt_people TYPE INTERNAL TABLE OF ty_person,
      ls_person TYPE ty_person.

ls_person-name = 'Ahmet'.
ls_person-age = 30.

APPEND ls_person TO lt_people.

ls_person-name = 'Ayşe'.
ls_person-age = 25.

APPEND ls_person TO lt_people.

LOOP AT lt_people INTO ls_person.
  WRITE:/ 'Name: ', ls_person-name,
          'Age: ', ls_person-age.
ENDLOOP. 
```

---

## 🧩 FORMS 

**FORM** ve **PERFORM** yapıları, ABAP'ta kodun daha okunaklı, tekrar kullanılabilir ve bakımı kolay hale getirilmesi için kullanılan temel bir kullanımdır. 

### FORM... ENDFORM - PERFORM

**FORM** ile başlayan ve **ENDFORM** ile biten blok, bir alt programın (subroutine) tanımını yapar. Bu kod bloğu, program ana akışında yer almaz; sadece çağrıldığında çalışır. 

**PERFORM** ifadesi, daha önce tanımlanmış olan FORM alt programını çalıştırır. Kod akışı, PERFORM satırına geldiğinde alt programa atlar, alt program bittiğinde ise PERFORM satırından sonraki kodla devam eder. 

+ **Tanım Yeri:** Genellikle programın en sonunda veya **INCLUDE** dosyaları içinde tanımlanır.

+ **Parametreler:** **USING** anahtar kelime ile değer alabilir (value ile alandaki değeri) veya **CHANGING** anahtar kelimesi ile dışarıdaki bir değişkenin değerini değiştirebilir. 

```abap
REPORT z_form_example.

DATA lv_total TYPE i.

PERFORM calculate_total USING 5 10 CHANGING lv_total.

WRITE:/ 'Toplam: ', lv_total. 

FORM calculate_total USING pv_a TYPE i
                           pv_b TYPE i
                     CHANGING pv_result TYPE i.

   pv_result = pv_a + pv_b.
ENDFORM. 
```

---

Aşağıdaki örnekte kullanıcı yaşına göre bilet fiyatı hesaplanıyor, sonuç bir FORM içerisinde üretiliyor.

```abap
DATA: lt_ages TYPE TABLE OF i,
      lv_age TYPE i,
      lv_price TYPE i.

APPEND 5 TO lt_ages.
APPEND 17 TO lt_ages.
APPEND 30 TO lt_ages.
APPEND 70 TO lt_ages.

LOOP AT lt_ages INTO lv_age.
   PERFORM get_ticket_price USING lv_age CHANGING lv_price.
   WRITE:/ 'Yaş: ', lv_age, 'Fiyat: ', lv_price.
ENDLOOP.

FORM get_ticket_price USING pv_age TYPE i
                      CHANGING pv_price TYPE i.

   IF pv_age < 12.
      pv_price = 10.
   ELSEIF pv_age < 65.
      pv_price = 20.
   ELSE.
      pv_price = 15.
   ENDIF.
ENDFORM.  
```




<img width="473" height="338" alt="01" src="https://github.com/user-attachments/assets/39a4606e-eb83-4e18-a8e9-fc372cb2b4ea" />

<img width="627" height="570" alt="02" src="https://github.com/user-attachments/assets/cfe2f1a6-9399-4f9c-b1cb-9d86d9d81603" />

<img width="547" height="510" alt="03" src="https://github.com/user-attachments/assets/467c46e1-2423-4d70-a695-ee421003d64b" />



