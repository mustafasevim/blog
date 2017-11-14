---
layout: post
title: Ubuntu'da tcpdump ile Ortamdaki 802.11 Paketlerini Yakalama
---
 

# Kablosuz Ağ Arabirim Çalışma Modları
<hr />
Kablosuz ağ adaptörleri kullandıkları sürücüye ve yapacağı işleve bağlı olarak  dört farklı modda çalışabilir.

 - `Managed Mod:` Bir erişim noktasına bağlanarak hizmet alan istemcinin bulunduğu mod.

  - `Monitor Mod:` Herhangi bir kablosuz ağa bağlanmadan pasif olarak ilgili kanaldaki tüm trafiğin izlenmesine olanak sağlayan mod. 

  - `Master Mod:` Etraftaki kablosuz ağ istemcilerine hizmet vermek için kullanılan mod. Erişim noktası olarak adlandırılan cihazlarda kablosuz ağ adaptörleri bu modda çalışır.

 - `Ad-Hoc Mod:` Arada bir AP olmaksızın kablosuz istemcilerin haberleşmesi için kullanılan mod.

# Ortamdaki Kablosuz Ağ Paketlerini Yakalama
<hr />
Öncelikle kullanılacak interface aşağıdaki komutlarla monitor moduna alınmalıdır.

	$ sudo ifconfig wlan0 down

	$ sudo iwconfig wlan0 mode monitor

	$ sudo ifconfig wlan0 up

Daha sonra dinlenecek kanal seçilmelidir.

	$ sudo iwconfig wlan0 chan 6

Komut satırından çalışan paket analizcisi tcpdump veya paketlerin grafik arayüz üzerinden izlenmesini sağlayan wireshark ile yakalanan paketleri inceleyebilirsiniz.

	$ sudo tcpdump -i wlan0
