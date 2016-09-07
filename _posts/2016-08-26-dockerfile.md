---
layout: post
title: Dockerfile  
---

Docker imajları, kuralları ve komutları önceden belirlenmiş Dockerfile adı verilen özel formattaki dosyalar baz alınarak oluşturulabilir.  Dockerfile, basit bir dil kullanır. Her satırda imajı oluşturacak işlemler sıralanır ve sonunda build işlemi gerçekleştirilerek imaj oluşturulur.

**Dockerfile Kullanmanın Avantajları:**

* Oluşturduğunuz konteyneri ve imajları taşımaktansa daha küçük boyuttaki bu dosyaları taşımak daha kolaydır.
* Dosya içeriği incelenerek imajdan türeyecek konteynerlerin ne iş yaptığı kolaylıkla anlaşılabilir.
* Varsayılan imajdan konteyner oluşturulduktan sonra tek tek tüm ihtiyaç duyulan komutları yazmaktansa bir dosyadan doğrudan tüm komutları çalıştırmak zaman kaybının önüne geçecektir.
* Github üzerinde oluşturulmuş bir dockerfile otomatik olarak docker hub tarafından build edilebilir. Elde edilen imaj docker hub üzerinden paylaşılabilir.

**Örnek Dockerfile**

<script src="https://gist.github.com/mustafasevim/ec60b029dbaba41a6e1955b7775bd732.js"></script>

Bulunduğumuz dizinde yer alan Dockerfile isimli dosyadan host üzerine imaj üretmek için:

	$ sudo docker build -t my_mongodb .
	
* `FROM` ile  oluşturulacak imajın temelinde hangi imajın kullanılacağı belirtilir.
* `MAINTAINER` ile imajı geliştiren kişi ya da kişiler belirtilir.
* `RUN` ile imajın build işlemi sırasında koşturulması gereken komutları belirtilir.
* `EXPOSE` ile belirtilen port numarası konteyner içerisinden host sisteme açılır.
* `CMD:` ile argüman belirtmeden konteynerı çalıştırdığınızda  yürütülecek  varsayılan argüman belirtilir. Eğer bir argüman ekleyerek çalıştırılırsa CMD göz ardı edilir.
* `ENTRYPOINT` ile konteyner çalıştırıldığında çalışacak argüman belirtilir.
* `ENV`  ile çevresel değişkenler belirtilir.
* `ADD` ile host dosya sisteminden veya internetten dosya/klasör eklemek için kullanılır.
* `COPY` ile ADD instructionı ile aynı şekilde kullanılır fakat bu instruction ile internetten dosya indirilemez sadece host üzerindeki dosya ve klasörleri kopyalamak için kullanılır.
* `WORKDIR` ile çalışma dizinini belirtilir.

Diğer kullanımları ve ayrıntılı bilgiyi [buradan]( https://docs.docker.com/engine/reference/builder/) öğrenebilirsiniz.
