---
layout: post
title: Docker-Compose
---

Docker-Compose, herhangi bir uygulama için birden fazla konteyner çalıştırılmak istendiğinde bu konteynerlerin yapılandırılması ve yönetimi için kullanılır. `docker-compose.yml` dosyasında detayları verilen ve birbirleri ile ilişkileri tanımlanan konteynerler tek bir komut ile çalıştırılabilir, birbirleri ile iletişime geçmesi sağlanabilir veya durdurulabilir. 

# Docker-Compose Kurulumu

Docker Engine, sistemde kurulu ise aşağıdaki komutlar ile Docker-Compose kurulumu yapılabilir.

	$ curl -L https://github.com/docker/compose/releases/download/1.8.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
```bash
$ chmod +x /usr/local/bin/docker-compose
```

# Docker-Compose Dosyasının Yapısı ve Konfigürasyon Opsiyonlarını

* `image:` Üretilecek konteyner imajının adını belirtmektedir. 
* `build:` Eğer imaj bir Dockerfile'dan üretilecekse bu dosyanın bulunduğu dosya yolunu belirmektedir 
* `command:` Konteyner başlatılığında çalışacak komut ya da komutlar dizisini belirtmektedir 
* `links:` Konteynere bağlanacak diğer konteynerlerin listesini belirtir. 
* `ports:` Konteynerin hangi portunun açılacağını belirtir. 
* `volumes:` Konteynere bir dizin bağlama işlemi sırasında kullanılır. 
* `environment:` Konteyner içerisinde çevre değişkeni tanımlamak için kullanılır

Diğer kullanımları ve ayrıntılı bilgiyi [buradan](https://docs.docker.com/compose/compose-file/) öğrenebilirsiniz.


**Örnek `docker-compose.yml`** 

	db:
		image: mysql:latest
		environment:
			- MYSQL_ROOT_PASSWORD=12345
	redis:
		image: redis:latest
	web:
		build: .
		ports:
			- “80:80”
		link:
			- db
			- redis

Konteyner için gerekli olan imaj hazırlama ve yazılımların birlikte çalışma yapılandırması tamamlandıktan sonra uygulama çalıştırılması için `docker-compose up` komutunun verilmesiyle, `docker-compose.yml`deki sıralamaya bağlı olarak öncelikle MySQL servisinin çalıştığı db isimli bir konteyner oluşturulur. Bunun ardından önbellek sistemi olan Redis servisinin çalıştığı redis isimli bir konteyner oluşturulur. Son olarak projenin bulunduğu dizindeki “Dockerfile” dikkate alınarak proje için bir adet imaj üretilerek bu imajdan da “web” adında bir konteyner üretilir ve çalıştırılır.


# Temel Docker-Compose Komutları

`docker-compose.yml` dosyası oluşturulduktan sonra dosyanın yer aldığı dizinde aşağıdaki komutlar ile konteynerler yönetilebilir.

    $ docker-compose version

Docker-Compose versiyon bilgisini öğrenmek için kullanılır.

	$ docker-compose build
    
Tanımlanan konteynerleri teker teker veya toplu halde build etmek için kullanılır.
    
     $ docker-compose up

Docker konteynerlerini kullanıma hazır hale getirir ve başlatır.
    
    $ docker-compose ps 
  
Konteynerlerin listesini görüntüler.
    
    $ docker-compose stop
    
Konteynerlerdeki servisleri durdurur.
    
    $ docker-compose logs 
    
Konteynerlerden çıktı görmek için kullanılır.

Diğer komutlar için ayrıntılı bilgiyi [buradan](https://docs.docker.com/compose/reference/) öğrenebilirsiniz.


