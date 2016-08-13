---
layout: post
title: Let's Encrypt ile Ücretsiz SSL Kurulumu
--- 


Bu makale Firefox, Chrome, Safari gibi önde gelen tüm tarayıcılar tarafından tanınan, Mozilla öncülüğünde, birçok şirketin destekleriyle geliştirilmiş olan ücretsiz SSL/TLS sağlayıcısının Apache web sunucusu ile kullanımı için hazırlanmıştır.

Öncelikle  Let's Encrypt deposu, işletim sisteminden bağımsız, sistem için zorunlu olmayan 3. parti kullanıcı programlarının bulunduğu `/opt` dizinine aşağıdaki komutlar ile klonlanmalıdır.

	$ sudo apt-get update && sudo apt-get upgrade
	$ sudo apt-get install git
	$ sudo git clone https://github.com/letsencrypt/letsencrypt /opt/letsencrypt
	$ cd /opt/letsencrypt

Ardından klasör içerisinde aşağıdaki komut SSL kullanılacak web sitesine göre düzenlenerek verilmelidir.

    $ ./letsencrypt-auto --apache -d mustafasevim.net -d www.mustafasevim.net
    
Eğer bir veya daha fazla subdomaine sahipseniz yukarıdaki komutun sonuda `-d` parametresi ile onlarıda eklemelisiniz. Bu şekilde ayarlama yapıldığı taktirde 90 gün geçerli ücretsiz SSL tarafınıza sunulacak ve kullanıma başlayacaksınız.

Sertifikanızın doğrulanıp doğrulanmadığını merak ediyor ve sorunsuz şekilde çalışıp çalışmadığını öğrenmek istiyorsanız [Entrust SSL Checker](https://entrust.ssllabs.com) online aracı kullanabilirsiniz.

Oluşturulan sertifikalar aşağıdaki dosya yolunda bulunmaktadır. 

	/etc/letsencrypt/live/mustafasevim.net/
    
SSL sertifikasını düzgün bir şekilde çalıştırdıktan sonra http protokolü üzerinden siteye bağlanan kullanıcıların https protokolüne yönlendirilmesi gerekmektedir. Ben bunun için Apache'nin [redirect](http://httpd.apache.org/docs/trunk/mod/mod_alias.html#redirect) özelliğini kullanarak http protokolünden gelen tüm bağlantı isteklerini [https://mustafasevim.net](https://mustafasevim.net) adresine yönlendiriyorum daha farklı yaklaşımlar ile de https protokolüne yönlendirilme yapılabilir. Apache, bağlantı ayarlamalarını http protokolü için `000-default.conf` yapılandırma dosyasında, https protokolü için de `default-ssl.conf` dosyasında tutuyor. 

	$ sudo pico /etc/apache2/sites-available/000-default.conf
    
    <VirtualHost *:80>
  	DocumentRoot /var/www/html
  	ServerName mustafasevim.net
  	ServerAlias www.mustafasevim.net
  	Redirect permanent / https://mustafasevim.net/
	</VirtualHost>

Yapılandırma ayarlarının geçerli olması için web sunucusunun yeniden başlatılması gerekmektedir.

    $ sudo service apache2 restart

Her 90 günde bir sertifikayı `./letsencrypt-auto renew` komutu yenileyebilir veya 

    $ sudo crontab -e

komutu ile açılan dosyanın sonuna aşağıdaki satırları ekleyerek bunun için görev planlayabilirsiniz.

	30 2 * * 1 /opt/letsencrypt/letsencrypt-auto renew >> /var/log/le-renew.log
	35 2 * * 1 /etc/init.d/apache2 restart > /dev/null 2>&1
	
`Kaynak:` [How To Secure Apache with Let's Encrypt on Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/how-to-secure-apache-with-let-s-encrypt-on-ubuntu-14-04)

