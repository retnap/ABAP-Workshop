# 📘 06. Database & Dictionary 

## Bu bölümde hangi konular olacak ?

Bu bölümde, ABAP öğrenirken kritik eşiklerden biri **Database ve Data Dictionary(DDIC)** konusunu işleyeceğiz. SAP'de verinin **nasıl tanımlandığını, nasıl ilişkilendirildiğini** ve **nasıl kontrol edildiğini** adım adım ele alacağız. 

---

# 🧩 SAP Data Dictionary (DDIC) Nedir ? 

SAP'te veritabanı nesneleri doğrudan kod içinde değil, **Data Dictionary** üzerinden tanımlanır. 

Bu yapı sayesinde:

+ Veri tutarlılığı sağlanır.

+ Programlar arasında ortak kullanım mümkün olur.

+ Değişiklikler merkezi olarak yönetililr.

SAP'de tüm dictionary nesnelerine **SE11** transaction kodu üzerinden erişilir. 

<img width="662" height="477" alt="01_SE11_View" src="https://github.com/user-attachments/assets/a1cd2900-7be6-44c1-9a69-d34952299700" />

---

# 🧩 Naming Convention - Z, Y ve E Farkı 

SAP standartlarına göre: 

+ **Z** ve **Y**
  + Customer tarafından oluşturulan tüm nesnelerde kullanılır (Table, Structure, Data Element, Domain vb.)

+ **E**
  + **Lock Object** oluşturulurken kullanılır.

+ Örneğin:
  + ZEMPLOYEE (DB Table)
 
  + ZEMP_ID (Data Element)
 
  + **E_ZEMPLOYEE** (Lock Object)
 
Bu ayrım, SAP standardı ile custom geliştirmelerin karışmasını önler. 

---

# 🔍 Where-Used List -Bir Tablo Nelerde Kullanılıyor?-

Bir DB tablosu üzerinde çalışırken en önemli sorulardan biri şudur:

"**Bu tablo başka nerelerde kullanılıyor?**"

SE11 içinde ilgili tabloya girip **Where-Used List** seçeneği ile:

+ Programlar

+ Views

+ Function Modules

+ Structures

gibi tüm bağımlılıkları filtreleyerek görebiliriz. 

<img width="1045" height="649" alt="02_Where_Used_List" src="https://github.com/user-attachments/assets/9422d4b6-b742-48b5-bbf6-a5a4ac979a9d" />

<img width="694" height="748" alt="03_Where_Used_List_2" src="https://github.com/user-attachments/assets/0497481a-fd5b-4cd9-be20-9e1423f73a64" />

<img width="874" height="208" alt="03_Where_Used_List_3" src="https://github.com/user-attachments/assets/541cfdfd-0732-4319-a542-e3b491d7f61b" />

---

# 📔 Table Contents - Veriyi Görüntüleme 

Bir tablonun detaylı bir şekilde içeriğine bakmak için: 

```abap
SE11 -> Table -> Contents -> Execute
```

### 📌 Contents Ekranında Neler Yapılabilir ? 

+ Filtreleme (WHERE koşulları) 

+ Belirli alanları görüntüleme 

+ Kayıt sayısını kontrol etme 

+ Test verisi doğrulama 







