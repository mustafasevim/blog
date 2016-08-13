---
layout: post
title: Forwarding DNS Server Kurulumu
--- 

Bu makale kendinize özel Forwarding DNS Server kurulumu için hazırlanmıştır. Forwarding DNS Server, kendisine gelen bir istegi başka bir DNS sunucusuna yönlendiren DNS  sunucusuna verilen addır. Forwarding DNS Server olarak kullanılacak olan sunucuya bağlandıktan sonra aşağıdaki komut ile BIND9 kurulumu yapılmalıdır. 

	$ sudo apt-get update && sudo apt-get install bind9 -y
    
Kurulum tamamlandıktan sonra `/etc/bind/` dizinindeki `named.conf.options` dosyasının içereği aşağıdaki gibi degiştirilmelidir.

    options {
	directory "/var/cache/bind";

	// If there is a firewall between you and nameservers you want
	// to talk to, you may need to fix the firewall to allow multiple
	// ports to talk.  See http://www.kb.cert.org/vuls/id/800113

	// If your ISP provided one or more IP addresses for stable 
	// nameservers, you probably want to use them as forwarders.  
	// Uncomment the following block, and insert the addresses replacing 
	// the all-0's placeholder.

        forwarders {
                8.8.8.8;
                8.8.4.4;
        };
        forward only;
 
        max-ncache-ttl 3;

        allow-transfer { none; };
        allow-update-forwarding { none; };
        allow-notify { none; };

        allow-recursion { any; };
        allow-query { any; };
        allow-query-cache { any; };

	//========================================================================
	// If BIND logs error messages about the root key being expired,
	// you will need to update your keys.  See https://www.isc.org/bind-keys
	//========================================================================
	dnssec-validation auto;

	auth-nxdomain no;    # conform to RFC1035
	listen-on-v6 { any; };
	};
    

Dosya içerisinde  

	forwarders {
        8.8.8.8;
        8.8.4.4;
	};

kısmında yapılmak istenilen DNS sunucusunun sorgu yapacağı DNS sunucularının IP adresleri verilmiştir. Ben örnek olarak Google tarafından hizmete sokulmuş DNS sunucularının IP adreslerini kullandım. Diğer kısımlar hakkında ayrıntılı bilgiyi [buradan](http://www.zytrax.com/books/dns/ch7/xfer.html) öğrenebilirsiniz. 

Yapılandırma ayarlarının geçerli olması için DNS sunusunun yeniden başlatılması gerekmektedir.

	$ sudo service bind9 restart

