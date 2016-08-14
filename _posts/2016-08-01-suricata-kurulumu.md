---
layout: post
title: Suricata Kurulumu

---
Bu makale Suricata' nın Ubuntu üzerine kurulumu için hazırlanmıştır.
Suricata, OISF (Open Information Security Foundation) topluluğu tarafından geliştirilen açık kaynak kodlu, GPLv2 lisansı ile dağıtılan, yüksek performanslı IDS, IPS ve NSM uygulamasıdır.


<script src="https://gist.github.com/mustafasevim/4e1f5391b35962bf1210cf552bc0b76a.js"></script>

Suricata IDS modunda sadece ağ trafiğini izler ve herhangi bir kötü niyetli trafik için engelleme yapmaz. Tüm paketler, kurallara dayalı bir şekilde izlenir ve sadece yönetici uyarılır. IDS özellikli kurmak için yukarıdaki script aşağıdaki komut ile çalıştırılmalıdır. 
	
	$ ./install.sh
    
IPS modunda ise Suricata host ve dış dünya arasında güvenlik duvarı gibi davranır. Uygulama kurallara dayalı bir şekilde ağ trafiğini izler ve kötü niyetli trafik engellenir. IPS özellikli kurmak için ise script aşağıdaki komut ile çalıştırılmalıdır. 

	$ ./install.sh -IPS 
	
# Kurallar 
Uygulamanın yüklenmesi varsayılan ayarlar ile tam yükleme yapıldığından `/etc/surica/rules` dizinindeki tüm kurallar [bu](https://rules.emergingthreats.net)  adresde bulunan herkese açık olarak sunulan en güncel kural veritabanı içermektedir. 

Uygulama, var olan kuralların düzenlenmesini veya sonradan kural eklenmesi mümkün kılan esnek bir kural tanımlama diline sahiptir. Her bir kural, başlık ve kural seçenekten oluşur.

<p align="center">
	<img alt="Git" src="https://redmine.openinfosecfoundation.org/attachments/download/440/intro_sig.png">
</p>

**Eylem:** Dört farklı eylem tipi vardır.

 * pass: paketi atlamak için kullanılır.
 * drop: paketi düşürmek için kullanılır.
 * reject: paketi reddetmek için kullanılır.
 * alert: sistem yöneticisine alarm vermek için kullanılır.

**Protokol:** Uygulamanın alabileceği protokol değerleri TCP, UDP veya ICMP olabilir. Ayrıca HTTP, FTP, TLS, SMB ve DNS gibi daha üst düzeylerde eşleşen protokolleri de içerebilir.

**Kaynak ve Hedef:** Protokolü belirttikten sonra kaynak adresini, kaynak portu, hedef adresi ve hedef portu belirtilmelidir.

**Yön:** Kuralın hangi yönde eşleşeceğini yön bilgisi verir. Örneğin, kaynak -> hedef (sadece kaynaktan hedefe) veya kaynak < > hedef (her iki yönde).

**Kural Seçenekleri:** Biçimi [isim : ayarlar] şeklindedir. Bazı metin içeren paketleri filtrelemek için de kullanılabilir. Örneğin, (içerik: ".torrent"). Ancak içerik şifrelenirse bu özellik çalışmaz.

# Yapılandırma

Uygulama `/etc/suricata` dizinin altında bulunan suricata.yaml dosyası ile yapılandırılmaktadır. Bu özel dosya ağlar, arayüzler, loglar, kurallar, dizinler gibi tüm yapılandırma değişkenlerini tutar.

**Kural etkinleştirme / devre dışı bırakma:** Kurallar varsayılan olarak /etc/surica/rules dizinde bulunmaktadır. Varsayılan kural yol değişkeni değiştirilerek varsayılan kurallar dizininin yolu değiştirilebilir. Kural dosyası veya kural dosyasının içindeki herhangi bir kural başına #(diyez) işareti konularak devre dışı bırakılabilir. 

**Log dizini:** Loglar varsayılan olarak `/var/log/suricata` dizinininde tutulmaktadır. Varsayılan dizin değiştirilebilir.

# Çalıştırma 
 
 Aşağıdaki komutla Suricata IDS modunda  çalıştırılır.
 
	$ sudo suricata −c /etc/suricata/suricata.yaml −i wlan0
 
 Suricata’ yı IPS modunda çalıştırmak için öncelikle iptables kuralları ile paketler kuyruklanmalıdır. 

	$ sudo iptables -I INPUT -j NFQUEUE

	$ sudo iptables -I OUTPUT -j NFQUEUE
    
Daha sonra aşağıdaki komut ile Suricata çalışırılmalıdır. 

	$ sudo suricata -c /etc/suricata/suricata.yaml -q 0


`Kaynak:` [ bhatuzdaname ](https://github.com/bhatuzdaname/suricata-configuration/blob/master/install.sh)


    
