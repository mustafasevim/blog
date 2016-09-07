---
layout: post
title: Docker Cheat Sheet  
---

**İmaj İşlemleri**

Sistemdeki Docker imajlarını listelemek için:

    $ docker images

Docker Hub üzerinden imaj indirmek için:

    $ docker pull name
   
Docker Hub üzerinde imaj aramak için:

    $ docker search name
    
Sistemde bulunan imajı silmek için:

    $ docker rmi <id>

Sistemde bulunan tüm imajları silmek için:

    $ docker rmi $(docker images -q)

**Konteyner İşlemleri**

Sadece çalışan  konteynerlarını listelemek için:

    $ docker ps

Tüm  konteynerları listelemek için:

    $ docker ps -a

En son çalışan konteynerleri görmek için:

    $ docker ps -l

Belirtilen konteyner üzerinde başlatma, durdurma ve yeniden başlatma işlemleri yapmak için:

    $ docker start/stop/restart <id>

Docker imajını kullanarak bir konteyner oluşturmak için:

    $ docker run <image>

 `--name:`  Docker imajına çalıştırken isim vermek için:

    $ docker run --name web tutum/hello-world

 `-it:` Intercative modda konteynerden terminale bağlantı yapmak için:

    $ docker run -it ubuntu

 `–d:` Konteyneri arka planda çalıştırmak için:

    $ docker run -d --name web -p 8081:80 tutum/hello-world

`-p:` Konteyner ile host sistem arasında port yönlendirmesi yapmak için:

    $ docker run -p 8080:80 tutum/hello-world


Konteynerde komut çalıştırmak için:

    $ docker exec <id> command

Konteyner bilgisini görmek için:

    $ docker inspect <id>

Çalışan bir konteynere bağlanmak için:

    $ docker attach <id>

Çalışan bir konteyner ne kadar kaynak tükettiğinin bilgisini almak için:

    $ docker stats <id>

Çalışan bir konteynerdeki processlerin bilgisini almak için:

    $ docker top <id> 
        
Konteyner silmek için:

    $ docker rm <id>

Sistemdeki tüm konteynerleri durdurmak için:

    $ docker stop $(docker ps -a -q)

Sistemdeki tüm konteynerleri silmek için:

    $ docker rm $(docker ps -a -q)


![alt text](https://ma.ttias.be/wp-content/uploads/2016/08/docker_cheat_sheet.png)
