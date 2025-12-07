ABAP'ta **Database Table** oluşturmak için şu adımları izlememiz gerekiyor:

📑 1. **SE11** transaction koduyla **ABAP Dictionary** ekranını açıyoruz. En üstte bulunan "Database table" seçeneğinin karşısına tablomuzun adını girerek "Create" tuşuna basıyoruz.

<img width="488" height="382" alt="Database_Table_1" src="https://github.com/user-attachments/assets/8e31287a-e8ec-4268-b267-8c0978000616" />

--- 

📑 2. "Delivery and Maintenance" sekmesi karşımıza çıkıyor. Üstte bulunan "Short Description" alanına tablomuz hakkında kısa bir açıklama yazıyoruz. "Delivery Class" olarak Application Table seçeneğini işaretliyoruz.

+ **Delivery Class**, SAP'de tabloların nasıl taşınacağını (transport) ve müşteri/sistem bağımlı olup olmadığını belirleyen bir tekniktir.

+ **A - Application Table (Müşteri Verisi)**
  + En çok kullanılan sınıftır.
  + Z ile oluşturulan tablolar genelde A seçilir.
  + İçindeki veriler **her client'ta ayrı**dır.
  + Transport edildiğinde **sadece tablo yapısı taşınır**, veriler taşınmaz.
  + Örnek: Öğrenciler, çalışanlar, ürün bilgisi (Z tablolar).

+ **C - Customizing Table (Client Bağımlı Özelleştirme Verisi)**
  + Sistemler ayarları, customizing verileri içerir.
  + Veriler **client-depent** yapıdadır.
  + Transport edilirken hem yapı hem veri taşınabilir.
  + IMG üzerinden düzenlenen ayar tabloları
  + Örnek: KDV oranları, şirket kodu ayarları, mali yıl konfigürasyonu
 
+ **L - Table for Storing Temporary Data (Local Data)**
  + Geçici veriler tutulur.
  + Transport edilmez yani veri taşınmaz.
  + Batch job geçici tabloları
  + Örnek: Geçici rapor sonuçları, buffer tabloları.
 
+ **G - Customizing, Cross-Client (Client Bağımsız Customizing)**
  + Ayarlar **client-independent** yapıdadır.
  + Transport edilmesi zorunludur.
  + Sistem bütünlüğü için önemli ayarlar içerir.
  + Örnek: Para birimi tanımları, ülkeler, ölçü birimleri

+ **E - System Table, Transport Always Allowed**
  + SAP sistemi için kritik tablolardır.
  + Transport zorunludur.
  + Client bağımlı olabilir ama ayarlar tekildir.
  + Örnek: Sistem konfigürasyonu, dil ayarları.

+ **S - System Table, Delivery Class, No Transport Allowed**
  + SAP tarafından kullanılan **sistem içi** tablolardır.
  + Transport yapılmaz, verileri değişmez.
  + Sadece SAP tarafından güncellenir.

+ **W - Repository Object (Kod, Ekran, Tablo Yapıları)**
  + Tablonun kendisi repository objesi olarak taşınır.
  + Genellikle SAP'nin kendi özel tabloları.
  + Veriler transport edilmez, sadece yapı taşınır.  

<img width="822" height="660" alt="Database_Table_2" src="https://github.com/user-attachments/assets/7d595570-f636-4461-87e3-130089a2d543" />

---

📑 3. "Fields" sekmesine giderek **Field** ve **Data Element** alanlarını doldurmaya başlayabiliriz.

  Öncelikle "Client Field" alanı olan MANDT yazıp enter tuşuna basıyoruz. Kalan kısımlar otomatik doluyor. 

  Bu işlemden sonra kendi belirlediğimiz field alanları doldurabiliriz. 

  Student ID anlamına gelen STD_ID değerini field alanına giriyoruz. Database tablosunu kendimiz sıfırdan oluşturduğumuz için Data Element kısmını kendimiz girip oluşturmamız lazım. Z harfi ile başlayarak ona da ZSTD_ID değerini giriyoruz. 

  Data Element ismini yazdık ama şu an herhangi bir data element oluşmadı. Onun için ZSTD_ID isminin üstüne çift tıklıyoruz. Kısa yoldan Data Element oluşturma ekranını açacaktır. 

<img width="734" height="496" alt="Database_Table_3" src="https://github.com/user-attachments/assets/06ff4a8a-b74d-4e23-88ed-83406a68a373" />

---

📑 4. ZSTD_ID üstüne çift tıkladıktan sonra Data Element oluşturma onay ekranı karşımıza çıkmış bulunuyor. 
   
<img width="576" height="441" alt="Database_Table_4" src="https://github.com/user-attachments/assets/bfc2bb1b-3314-4b8f-a384-5c1a7dce1ab4" />

---

📑 5. Gelen "Change Data Element" ekranında oluşturacağımız data element için kısa bir açıklama giriyoruz.

   Elemantary Type -> Domain seçerek (otomatik geliyor) Data Element özelliklerini belirteceğimiz **Domain** yapısının adını giriyoruz.

   "ZSTD_ID" domain adının üstüne çift tıklıyoruz. Bu işlem de kısa yoldan Domain oluşturma ekranına yönlendirecektir. 

<img width="688" height="494" alt="Database_Table_5" src="https://github.com/user-attachments/assets/4a077bbe-ab88-42b5-92f7-c819bd5b3896" />

--- 

📑 6. ZSTD_ID üstüne çift tıkladıktan sonra Domain oluşturma onay ekranı karşımıza çıkmış bulunuyor.
   
<img width="569" height="461" alt="Database_Table_6" src="https://github.com/user-attachments/assets/9816a8d8-b5bb-4488-a81b-48ad8f053c4f" />

---

📑 7. Gelen "Change Domain" ekranında, oluşturacağımız domain için kısa bir açıklama giriyoruz.

   Ardından id değeri gireceğimiz için Data Type olarak **NUMC** seçiyoruz ve uzunluğunu 15 olarak giriyoruz.

   Oluşturduğumuz Domain'i activate edip bir önceki ekrana dönebiliriz. 
   
<img width="700" height="453" alt="Database_Table_7" src="https://github.com/user-attachments/assets/ee95a8de-e8c7-4730-966b-9d8b5e2c7385" />

---

📑 8. Oluşturduğumuz domain'i activate ettikten sonra "Change Data Element" ekranına geri dönüyoruz.

Ekranı küçülttükçe ya da büyüttükçe yazıların nasıl görüneceğini bu alandan seçiyoruz. 

Aynı şekilde son ayarlarını yaptığımız data element'i de activate ettikten sonra bir önceki ekrana geri dönüyoruz.

<img width="727" height="331" alt="Database_Table_8" src="https://github.com/user-attachments/assets/896b62c3-767c-4dfe-87fd-7d71870dc30c" />

---

📑 9. ZSTD_ID Data Element'ini ve ona bağlı ZSTD_ID Domain'ini oluşturma adımları bitti. Artık Field kısmında data element'in ismini yazıp enter'a bastğımızda kalan kısımlar otomatik olarak dolacaktır.

Geri kalan field alanlar için işlemler aynı şekilde devam etmektedir. Database tablomuzun son görünümü aşağıda bulunan görseldeki gibi olacaktır. 

<img width="935" height="468" alt="Database_Table_9" src="https://github.com/user-attachments/assets/c304e462-ee4b-4614-8664-22e78e8b5669" />

