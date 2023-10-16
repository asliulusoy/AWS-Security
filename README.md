---
title: AWS Security Best Practices (Türkçe)
---

# Kimlik ve Erişim Yönetimi

## 1.1 Makineler ve İnsanlar İçin Kimlik Doğrulamayı Yönetmek 

**İnsan Kimlikleri:** Yöneticileriniz, geliştiricileriniz,
operatörleriniz ve kullanıcılarınız (end-users), AWS ortamlarınıza ve
uygulamalarınıza [erişim sağlamak için]{.underline} bir kimliğe ihtiyaç
duyar. Bunlar kaynaklarınıza bir web tarayıcısı, istemci uygulaması veya
etkileşimli komut satırı araçları aracılığıyla erişen kişilerdir.

**Makine Kimlikleri:** Hizmet uygulamalarınız, işletme araçlarınız ve iş
yükleriniz (workloads), AWS hizmetlerine [istekte bulunmak
için]{.underline} bir kimliğe ihtiyaç duyar. Bu kimlikler, Amazon EC2
instances veya AWS Lambda functions gibi AWS ortamınızda çalışan
makineleri içerebilir. Ek olarak, AWS dışında erişime ihtiyaç duyan
makineleriniz olabilir ve bu makinelerin de kimliklerini
yönetebilirsiniz.

### 1.1.1 Güçlü Oturum Açma Mekanizmalarını Kullanın 

Oturum açma esnasında (oturum açma kimlik bilgileri kullanarak kimlik
doğrulama), özellikle oturum açma kimlik bilgileri kazara ifşa edilmişse
veya kolayca tahmin ediliyorsa, çoklu faktörlü kimlik doğrulama (MFA)
gibi mekanizmalar kullanılmadığında riskler ortaya çıkabilir.

**Amaç**: AWS Kimlik ve Erişim Yönetimi (IAM) kullanıcıları, AWS hesap
kök kullanıcısı (account root user), AWS IAM Kimlik Merkezi ve üçüncü
taraf kimlik sağlayıcıları için güçlü oturum açma mekanizmalarını
kullanarak AWS\'deki kimlik bilgilerine istenmeyen erişim riskini
azaltın.

-   MFA gereksinimi

-   Güçlü parola politikalarını zorunlu kılma

-   Anormal giriş davranışlarını tespit etme

Risk Düzeyi: **Yüksek**

### 1.1.2 Geçici Kimlik Bilgileri Kullanın 

Her türlü kimlik doğrulama işlemini yaparken, kimlik bilgilerinin kazara
ifşa edilmesi, paylaşılması veya çalınması gibi riskleri azaltmak için
uzun vadeli kimlik bilgileri yerine geçici kimlik bilgilerini kullanmayı
tercih edin.

**Amaç:** Uzun vadeli kimlik bilgileri birçok risk oluşturur, örneğin bu
kimlik bilgileri kodlara yüklenebilir ve açık GitHub depolarına
yüklenebilir. Geçici kimlik bilgilerini kullanarak, kimlik bilgilerinin
tehlikeye girmesini önemli ölçüde azaltın.

-   Geliştiricilerin giriş yapmayı kullanarak CLI\'den geçici kimlik
    bilgileri almak yerine IAM kullanıcılarından uzun vadeli erişim
    anahtarlarını kullanması.

-   Geliştiricilerin uzun vadeli erişim anahtarlarını kodlarına
    yerleştirmesi ve bu kodları açık Git depolarına yüklemesi.

-   Geliştiricilerin uzun vadeli erişim anahtarlarını mobil uygulamalara
    yerleştirmesi ve bu uygulamaları uygulama mağazalarında sunması.

-   Kullanıcıların uzun vadeli erişim anahtarlarını diğer kullanıcılarla
    paylaşması veya çalıştıkları şirketten uzun vadeli erişim
    anahtarlarıyla ayrılmaları.

-   Makine kimlikleri için geçici kimlik bilgileri kullanılabilirken
    uzun vadeli erişim anahtarlarının kullanılması.

Risk Düzeyi: **Yüksek**

### 1.1.3 Gizli Bilgileri Güvenli Bir Şekilde Saklayın ve Kullanın 

Bir iş yükü (workload), veritabanlarına, kaynaklara ve üçüncü taraf
hizmetlere kimliğini kanıtlayabilme yeteneğine ihtiyaç duyar. Bu, API
erişim anahtarları, parolalar ve OAuth tokenleri gibi gizli erişim
kimlik bilgileri kullanılarak gerçekleştirilir. Bu kimlik bilgilerini
saklamak, yönetmek ve düzenli olarak değiştirmek için özel olarak
tasarlanmış bir hizmet kullanmak, bu kimlik bilgilerinin tehlikeye girme
olasılığını azaltır.

**Amaç:** Uygulama kimlik bilgilerini güvenli bir şekilde yönetecek bir
mekanizmayla potansiyel güvenlik açıklarını azaltın.

-   İş yükü için gereken gizli bilgileri tanımlama.

-   Uzun vadeli kimlik bilgilerini kısa vadeli kimlik bilgilerle
    değiştirerek gereken uzun vadeli kimlik bilgilerinin sayısını
    azaltma.

-   Kalan uzun vadeli kimlik bilgilerinin güvenli saklanması ve otomatik
    olarak değiştirilmesi.

-   İş yükünde bulunan gizli bilgilere erişimi denetleme.

-   Geliştirme süreci sırasında kaynak kodlarına gizli bilgi
    eklenmediğini doğrulamak için sürekli izleme (monitoring).

**Faydaları:**

-   Gizli bilgiler, depolanmış halde ve iletilirken şifrelenmiş olarak
    saklanır.

-   Kimlik bilgilerine erişim API aracılığıyla sınırlandırılır (bir tür
    kimlik bilgileri dağıtma makinesi olarak düşünün).

-   Kimlik bilgilerine erişim (hem okuma hem de yazma) denetlenir ve
    kaydedilir.

-   İşlevlerin ayrılması: Kimlik bilgilerinin döngüsü, geriye kalan
    yapıdan ayrılabilen ayrı bir bileşen tarafından gerçekleştirilir.

-   Kimlik bilgileri otomatik olarak yazılı bileşenlere talep
    üzerine(on-demand) dağıtılır ve döngü merkezi bir konumda
    gerçekleşir.

-   Kimlik bilgilerine erişim hassas bir şekilde kontrol edilebilir.

Risk Düzeyi: **Yüksek**

### 1.1.4 Merkezi Bir Kimlik Sağlayıcıya Güvenin 

İş gücü (workforce) kimlikleri (çalışanlar ve taşeronlar) için,
kimlikleri merkezi bir noktada yönetmenize izin veren bir kimlik
sağlayıcısı kullanın. Bu, birden çok uygulama ve sistem üzerinde erişimi
yönetmeyi daha kolay hale getirir, tek bir konumdan erişim sağlayabilir,
atayabilir, yönetebilir, geri çekebilir ve denetleyebilirsiniz.

**Amaç:** Kimlik doğrulama politikalarını merkezi bir kimlik sağlayıcı
üzerinden yönetin. Bu kimlik sağlayıcı aracılığıyla sistemlere ve
uygulamalara yetkilendirme sağlayın. İş gücü kullanıcıları, merkezi
kimlik sağlayıcılarına giriş yaparlar ve iç veya dış kaynaklara erişim
sağlamak için tek bir oturum açma işlemi kullanırlar, böylelikle
kullanıcıların birden fazla kimlik bilgisini hatırlamalarına gerek
kalmaz. Kimlik sağlayıcınızı, insan kaynakları (HR) sistemlerinizle
entegre edin. Personel değişikliklerinin otomatik olarak kimlik
sağlayıcınızla senkronize edin. Örneğin, bir çalışan organizasyonunuzu
terk ettiğinde, bu bilgi otomatik olarak erişim sağlanan uygulamalara ve
sistemlere iletilir ve böylece erişimleri otomatik olarak iptal edilir.

**Faydaları:**

İnsan kaynakları (HR) sistemlerinizle entegre olduğunuzda, bir kullanıcı
rol değiştirdiğinde bu değişiklikler kimlik sağlayıcıya senkronize
edilir ve atanan uygulamalar ve izinler otomatik olarak güncellenir.

Risk Düzeyi: **Yüksek**

### 1.1.5 Düzenli Olarak Kimlik Bilgilerini Denetleyin ve Yenileyin 

Kimlik bilgilerinin kaynaklarınıza erişim için ne kadar süreyle
kullanılabileceğini sınırlayın. Uzun vadeli kimlik bilgileri birçok risk
oluşturur ve bu riskler, uzun vadeli kimlik bilgilerini düzenli olarak
yenileyerek azaltılabilir.

**Amaç:** Uzun vadeli kimlik bilgilerini (long-term credentials) düzenli
olarak yenileyin. Kimlik bilgisi yenileme politikalarına uymayan
durumları düzenli olarak denetleyin ve düzeltin.

Risk Düzeyi: **Yüksek**

### 1.1.6 Kullanıcı Grupları ve Özelliklerini Kullanın 

Yönettiğiniz kullanıcı sayısı arttıkça, bunları ölçeklenebilir bir
şekilde yönetebilmek için nasıl düzenleyebileceğinizi belirlemeniz
gerekecektir. Ortak güvenlik gereksinimlerine sahip kullanıcıları,
kimlik sağlayıcınız tarafından tanımlanan gruplara yerleştirin.
Kullanıcı gruplarını ve özelliklerini yönetmek için AWS IAM Kimlik
Merkezi\'ni (IAM Identity Center) kullanabilirsiniz. Bireysel roller
yerine bu grupları kullanarak erişimleri kontrol edin.

Risk Düzeyi: **Düşük**

## 1.2 Makineler ve İnsanlar İçin İzinleri Yönetmek 

### 1.2.1 Erişim Gereksinimlerini Tanımlayın 

İş yükünüzün her bileşenine/kaynağına yöneticiler, kullanıcılar
(end-users) veya diğer bileşenler tarafından erişilmesi gerekebilir. Her
bileşene kimin veya neyin erişmesi gerektiğini net bir şekilde
tanımlayın, uygun kimlik türünü ve kimlik doğrulama ve yetkilendirme
yöntemini seçin.

-   Uygulamanızda gizli bilgileri sabit olarak saklamama.

-   Her kullanıcı için özel izinler vermeme.

-   Uzun süreli kimlik bilgilerini kullanmama.

Risk Düzeyi: **Yüksek**

### 1.2.2 En Az Ayrıcalığa Erişim Verme 

Erişimi sadece spesifik aksiyonları gerçekleştirmek için spesifik
kaynaklar ve spesifik koşullar altında vermelisiniz. Grup özelliklerini
kullanarak bu erişimleri dinamik olarak yönetebilirsiniz. Örneğin, bir
grup geliştiriciye sadece kendi projelerinin kaynaklarını yönetme
erişimi verebilirsiniz. Bu şekilde bir geliştirici proje ekibinden
ayrıldığında erişimi temel politikaları değiştirmeden otomatik olarak
iptal edebilirsiniz.

**Amaç:** Kullanıcılara yalnızca belirli bir görevi belirli bir sınırlı
süre içinde gerçekleştirmek için üretim ortamlarına erişim verilmelidir
ve bu görev tamamlandığında erişim iptal edilmelidir. İzinler, artık
gerekmeyen durumlarda, bir kullanıcının farklı bir projeye veya işlevine
geçtiğinde de iptal edilmelidir. Yönetici ayrıcalıkları, yalnızca
güvenilen birkaç yönetici grubuna verilmelidir. İzinler, izin sızmasını
(permission creep) önlemek için düzenli olarak gözden geçirilmelidir.
Makine veya sistem hesapları, görevlerini tamamlamak için gereken en
küçük izin setini almalıdır.

-   Kullanıcılara varsayılan olarak yönetici izinleri vermeme.

-   Günlük aktiviteler için kök kullanıcıyı (root user) kullanmama.

-   İzinleri inceleyerek en az ayrıcalık erişimine izin verip
    vermediğini anlama.

Risk Düzeyi: **Yüksek**

### 1.2.3 Acil Erişim Süreci Oluşturmak 

Acil erişim süreci oluşturmak, merkezi kimlik sağlayıcınızda beklenmeyen
bir sorun oluştuğunda iş yüklerinize acil erişim sağlama imkânı sunar.
Örneğin, normal koşullarda iş gücü kullanıcıları, iş yüklerini yönetmek
için merkezi bir kimlik sağlayıcısını kullanırlar. Ancak merkezi kimlik
sağlayıcınız arızalanırsa veya bulutta giriş yapılandırması
değiştirilirse, iş gücü kullanıcıları buluta giriş yapamayabilir. Acil
erişim süreci, ilgili sorunları gidermek için alternatif yöntemlerle
(örneğin alternatif bir giriş yöntemi veya doğrudan kullanıcı erişimi
gibi) bulut kaynaklarına erişim sağlar. Acil erişim süreci, normal
mekanizmayı yeniden sağlanana kadar kullanılır.

> **Amaç:**

-   Acil durum olarak kabul edilebilecek arıza modlarını tanımlama:
    Normal koşulları ve yönetim için bağımlı oldukları sistemleri
    düşünün. Bu bağımlılıkların her birinin nasıl başarısız
    olabileceğini saptama.

-   Bir arızanın bir acil durum olarak kabul edilmesi gereken adımları
    tanımlama: Örneğin, başlıca ve yedek kimlik sağlayıcılarınızın her
    ikisi de kullanılamazsa kimlik sağlayıcı arızası için bir acil durum
    süreci oluşturabilirsiniz.

-   Her tür acil durum için ayrı bir acil erişim süreci tanımlama: Acil
    erişim süreçleriniz, her sürecin ne zaman kullanılması gerektiği
    durumlarını açıklar ve her acil durumda aynı protokolün
    kullanılmasının önüne geçer.

-   Süreçlerinizi, hızlı ve verimli bir şekilde izlenebilecek ayrıntılı
    talimatlar ile iyi bir şekilde belgeleme: Bir acil durum olayı,
    kullanıcılarınız için stresli bir zaman olabilir ve yoğun zaman
    baskısı altında olabilirler, bu nedenle sürecinizi mümkün olduğunca
    basit tasarlayın.

**Sık Yapılan Hatalar:**

-   İyi belgelenmiş ve iyi test edilmiş acil erişim süreçleriniz
    olmadığında, kullanıcılarınız bu duruma hazırlıksızdır ve acil bir
    durum olayı meydana geldiğinde geçici çözümlere başvururlar.

-   Acil erişim süreçleriniz, normal erişim mekanizmalarınız gibi aynı
    sistemlere (örneğin merkezi bir kimlik sağlayıcı) bağımlıdır. Bu,
    engelleyemeyeceğiniz bir durum yaratır.

-   Acil erişim süreçleri, acil olmayan durumlarda kullanılır. Örneğin,
    kullanıcılarınız acil erişim süreçlerini değişiklikleri doğrudan
    yapmayı tercih etmeleri nedeniyle sık sık yanlış kullanırlar.

-   Acil erişim süreçleriniz süreçleri denetlemek için yeterli kayıt
    (logs) oluşturmaz veya kayıtlar süreçlerin kötüye kullanımı için
    potansiyel uyarılar için izlenmez (monitoring).

**Faydaları:**

-   İyi belgelenmiş ve iyi test edilmiş acil erişim süreçlerine sahip
    olarak, kullanıcılarınızın bir acil durum olayına yanıt verme ve
    çözme süresini azaltabilirsiniz. Bu, müşterilere sunduğunuz
    hizmetlerin daha az kesintiye uğramasına ve daha yüksek
    kullanılabilirlik sağlar.

-   Her acil erişim isteğini takip edebilir, izinsiz kullanım
    girişimlerini tespit edebilir ve uygunsuz kullanımı belirlemek için
    uyarı oluşturabilirsiniz.

> Risk Düzeyi: **Orta**

### 1.2.4 Sürekli Olarak İzinleri Azaltma 

**Amaç:** İzin politikaları, en az ayrıcalık ilkesine uygun olmalıdır.
İş görevleri ve roller daha iyi tanımlandıkça, gereksiz izinleri
kaldırmak için izin politikalarınızın gözden geçirilmesi gereklidir. Bu
yaklaşım, kimlik bilgileri ifşa edilirse veya yetkilendirme olmaksızın
erişilirse etki alanını azaltır.

**Sık Yapılan Hatalar:**

-   Kullanıcılara varsayılan olarak yönetici izinleri verme.

-   Artık gerekmeyen izin politikalarını saklama.

Risk Düzeyi: **Orta**

### 1.2.5 Kuruluşunuz İçin İzin Sınırları Belirleyin 

Kuruluşunuzdaki tüm kimliklerin erişimini kısıtlayan ortak denetimleri
oluşturun. Örneğin, belirli AWS Bölgelerine erişimi sınırlayabilir veya
operatörlerinizi, merkezi güvenlik ekibi için kullanılan bir IAM rolünü
silmek gibi ortak kaynakları silmekten engelleyebilirsiniz.

**Sık Yapılan Hatalar:**

-   Kurumsal yönetici hesabında iş yüklerini (workloads) çalıştırma.

-   Üretim ve üretim dışı iş yüklerini aynı hesapta çalıştırma.

Risk Düzeyi: **Orta**

### 1.2.6 Erişimi Yaşam Döngüsüne Göre Yönetme 

Operatör ve uygulama yaşam döngüsü ile merkezi giriş sağlayıcınızı
entegre etmek için erişim kontrollerini yönetin. Örneğin, bir kullanıcı
organizasyondan ayrıldığında veya rollerini değiştirdiğinde erişimini
kaldırın.

Ayrı hesaplar kullanarak iş yüklerini yönettiğinizde, bu hesaplar
arasında kaynakları paylaşmanız gereken durumlar olacaktır. AWS Kaynak
Erişim Yöneticisi (AWS RAM) kullanmanızı öneririz.

Risk Düzeyi: **Düşük**

### 1.2.7 Kamu ve Farklı Hesaplara Erişimi Analiz Edin 

**Amaç:** AWS kaynaklarınızın hangilerinin kimlerle paylaşıldığını
bilin. Paylaşılan kaynaklarınızı sürekli olarak izleyin ve denetleyin,
yalnızca yetkilendirilmiş prensiplerle paylaşıldığını doğrulayın.

**Sık Yapılan Hatalar:**

-   Paylaşılan kaynakların envanterini tutmamak.

-   Farklı hesaplara veya kamu erişimini kaynaklara onaylama sürecini
    takip etmemek.

> Risk Düzeyi: **Düşük**

### 1.2.8 Kuruluş İçinde Kaynakları Güvenli Bir Şekilde Paylaşma 

İş yüklerinin sayısı arttıkça, bu iş yüklerindeki kaynaklara erişimi
paylaşmanız veya kaynakları birden çok hesap üzerinde birden çok kez
sağlamanız gerekebilir. Geliştirme, test ve üretim ortamlarıyla
çevrenizi bölümlendirmek gibi yapılarınız olabilir. Ancak ayrı yapıları
oluşturmanız, aynı kaynağı birden çok kez oluştururken neyi kaçırmış
olabileceğinizi tahmin etme ihtimalinizi ortadan kaldırmaz.

**Amaç:** Kuruluşunuz içinde kaynakları güvenli yöntemlerle paylaşarak
istenmeyen erişimi en aza indirmek ve veri kaybını önlemek temel
amaçtır.

-   Bireysel bileşenleri yönetmeye kıyasla operasyonel iş yükünüzü
    azaltmak.

-   Aynı bileşeni manuel olarak birden çok kez oluşturmadan hataları
    azaltmak ve iş yüklerinizin ölçeklenebilirliğini artırmak.

-   Çoklu nokta arıza senaryolarında çözüm süresini azaltmak ve bir
    bileşenin artık gereksiz olduğunu belirlemek.

**Sık Yapılan Hatalar:**

-   Beklenmeyen dış paylaşımları sürekli olarak izlememek ve otomatik
    olarak uyarı oluşturmak için sürekli bir süreç planlamamak.

-   Nelerin paylaşılması gerektiği ve nelerin paylaşılmaması gerektiği
    konusunda temel eksikliği.

-   Gerekli olduğunda temel kaynakları manuel olarak oluştururken
    örtüşme oluşturma.

Risk Düzeyi: **Orta**

### 1.2.9 Üçüncü Bir Taraf ile Kaynakları Güvenli Bir Şekilde Paylaşma 

Kuruluşunuz, verilerinizin bir bölümünü yönetmek için üçüncü bir tarafa
güvenebilir. Üçüncü taraf tarafından yönetilen sistemin izin yönetimi,
geçici kimlik bilgileri kullanarak en az ayrıcalık ilkesini takip
etmelidir.

**Amaç:** IAM güven (trust) politikasındaki harici kimlik (external ID)
için evrensel benzersiz tanımlayıcı (UUID) kullanarak ve IAM
politikalarını IAM rolüne ekli tutarak, üçüncü tarafa verilen erişimin
çok fazla izinli olmadığını denetleyebilir ve doğrulayabilirsiniz.

**Sık Yapılan Hatalar:**

-   Koşullar olmadan varsayılan IAM güven (trust) politikasını kullanma.

-   Uzun vadeli IAM kimlik bilgileri ve erişim anahtarlarını kullanma.

-   Harici kimlikleri yeniden kullanma.

Risk Düzeyi: **Orta**
