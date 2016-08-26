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

```bash
############################################################
# Dockerfile to build MongoDB container images
# Based on Ubuntu
############################################################

# Set the base image to Ubuntu
FROM ubuntu

# File Author / Maintainer
MAINTAINER Mustafa Sevim <info@mustafasevim.net>

# Update the repository sources list
RUN apt-get update

################## BEGIN INSTALLATION ######################
# Install MongoDB Following the Instructions at MongoDB Docs
# Ref: http://docs.mongodb.org/manual/tutorial/install-mongodb-on-ubuntu/

# Add the package verification key
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10

# Add MongoDB to the repository sources list
RUN echo 'deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen' | tee /etc/apt/sources.list.d/mongodb.list

# Update the repository sources list once more
RUN apt-get update

# Install MongoDB package (.deb)
RUN apt-get install -y mongodb-10gen

# Create the default data directory
RUN mkdir -p /data/db

##################### INSTALLATION END #####################

# Expose the default port
EXPOSE 27017

# Default port to execute the entrypoint (MongoDB)
CMD ["--port 27017"]

# Set default container command
ENTRYPOINT usr/bin/mongod
```

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
