# Samet's Docker For Dummies
## **Docker Nedir?**
Docker, uygulamaları container adı verilen sanal ve hafif ortamlar içerisinde geliştirip dağıtmak için kullanılan bir containerlaştırma yazılımıdır.<br>

### ***Container Nedir?***
Sanallaştırmanın sanal makineler gibi ağır çözümleri yerine daha hafif ve atılgan bir hali olan containerlar sadece çalıştıracakları uygulamanın (veya mikroservisin) kendisi ile gerekli kütüphanelere ihtiyaç duyarlar, geri kalan her şey (işletim sistemi dahil olmak üzere) containerın içinde gelir, bu sayede uygulamalar neredeyse her yerde çalışabilir, bilgisayarınızda, sunucuda, bulutta, hatta telefonunuzda.<br>

### ***Container ile Sanal Makinenin Farkı***

Sanal makineler bilgisayar donanımını, yani bir bilgisayarın kullanılması için gereken her şeyi (İşlemci, bellek, anakart, disk vb.) simüle eder. <br>Hypervisorlar ise (Sanal makineleri yöneten yazılımlar) sadece bilgisayarı simüle etmekle kalmayıp ağ katmanlarını, grafik katmanlarını ve benzeri katmanları da simüle eder ki sanal ağlar, grafik kartları ve benzeri şeyler oluşturulup bilgi işlemede kullanılabilsin. Her bir ek katman ve sanallaştırılan bileşen daha fazla kod, ve bu kodu çalıştırmak için ana bilgisayara her kaynak bakımından daha fazla yük demektir.<br>

Aynı zamanda sanal makineye yükleyip çalıştırdığınız bir işletim sisteminde işletim sisteminin tamamı sanallaştırılır. 2 sanal makine 2 tane, 4 sanal makine 4 tane, n sanal makine n tane işletim sistemi sanallaştırır.<br>
Uydurduğumuz bu sanal makinelerin her birinin 2 çekirdekli bir işlemci ve 4 GB RAM ihtiyacı olduğunu düşünürsek 8 tane sanal makine için 16 çekirdek ve 32 GB ram ihtiyacımız var, ki 2 çekirdek ve 4 GB RAM modern işletim sistemleri için düşük değerlerdir.<br>

Containerlar ise donanım yerine işletim sistemini sanallaştırır, yerine göre kerneli bile altındaki host işletim sisteminden alır, ve sanal makinelere göre çok daha hafiftir. Bir containerda sadece bir uygulamayı ve gerektirdiği kütüphaneleri tutmamız mümkündür, bu sayede aynı kerneli 8 kez sanallaştırmayız veya kullanmayacağımız notepad.exe uygulamasını 8 farklı sanal bilgisayarda gereksiz yere tutmayız. Containerda neye ihtiyacımız varsa o bulunur.<br>

Kaynak tüketimi konusunda containerlar sanal makinelerin aksine kendilerine ayrılmış kaynak istemezler, ortak kaynakları mutlu mutlu kullanırlar.<br>

Containerların bize sunduğu farklı avantajlardan biri de çok daha rahat bir şekilde yatay genişletilebilme sağlamalıdır.<br>

Bir uygulamanın yatay genişletilebilmesi demek aynı uygulamadan n tane daha fazla çalıştırılabilmesi ve bunların uyum içinde çalışması demektir.<br>
Dikey genişletilebilme ise sistem kaynaklarını arttırmadır (Bir sanal makinede olan 4 GB RAM'i 8 GB'a çıkarmak gibi)<br>

Gerçek bir örnek üzerinden gidecek olursak, bir load-balancer (kendisine gelen istekleri konfigürasyonuna göre başka sunuculara dağıtan bir sunucu) arkasında 8 çekirdekli işlemcili ve 16 GB RAMli iki tane sunucumuz olduğunu düşünelim, bu sanal makinelerde de şirketimizin Muazzam Web Uygulaması bulunuyor olsun.<br> 
Bu sanal makinelerin işlemci ve RAM kullanımı hiçbir zaman %100 olmaz, %100 olması zaten en az bir tane daha sunucuya ihtiyacımız olduğu veya elimizdeki sunucuların özelliklerini iyileştirmemiz gerektiği anlamına gelir.<br>

Ama Muazzam Web Uygulamamız ne kadar popüler olursa olsun sunucularımız hiçbir zaman %100 dolu olmayacaktır. Sunucuların %100 dolu olmaması kullanılmayan kaynakların boşa gittiği anlamına gelir, bu da şirket için ek maliyet demektir.<br>
Yeni sunucu satın alınması yatay genişletmedir. Sunucuların özelliklerini iyileştirmek dikey genişletmedir.<br>

Containerlar için dikey genişletme diye bir şey yoktur, dikey genişletme çalıştığı bilgisayar için söz konusudur. <br>
Yatay genişletme ise yeni containerlar başlatmaktan ibarettir, ki bu da çok basittir.<br>


![Docker Containerları ve Sanal Makineler](https://phoenixnap.com/kb/wp-content/uploads/2021/04/container-vs-virtual-machine.png "Docker Containerları ve Sanal Makineler")

## **Kullanım**

Containerlar güzel şeyler, peki nasıl kullanılır?<br>
Docker containerları kullanmak için öncelikle bilgisayarımıza Docker Engine kurmamız gerekiyor.<br>
[Docker Web Sitesi](https://www.docker.com/)<br>
Linux için doğrudan paketi kurup servisi çalıştırmak mümkünken Windows için WSL2 kurulup Docker'ın içinde kullanılması öneriliyor.<br>

Ubuntu ve türevleri bir Linux'a Docker kurmak için:<br>
`sudo apt install docker.io` <br>
komutunu yazın.<br>
Paketin kurulmasının ardından<br>
`sudo systemctl enable --now docker` <br>
komutu ile Docker servisini hem çalışır hem de otomatik başlatır hale getirin.

### Docker Container Başlatma
Docker'ımız doğru çalışıyor mu test etmek için komut satırına<br>
`sudo docker run hello-world` <br>
komutunu yazın.<br>
Eğer kurulumda herhangi bir sorun yoksa ilk başta <br>
`Unable to find image 'hello-world:latest' locally` <br>
çıktısını alırsınız, ve docker çalıştırmak istediğiniz imaj olan hello-world imajının son halini otomatik olarak indirir ve çalıştırır.<br>
Çalıştırdığımız container komut satırına çalıştığını belirten bir çıktı yazdırır.<br>
Artık istediğimiz docker imajını ve etiketini `sudo docker run imajismi:etiket` ile çalıştırabiliriz.<br>
Hatta `sudo docker run --rm -it imajismi:etiket çalıştırılacakkomut` syntaxine uyulması durumunda interaktif terminaliniz bile olabilir.<br>
### Çeşitli Temel Docker Komutları
- **docker ps**<br>
Docker containerlarını listelemeyi sağlar. Listelemekle kalmayıp bize, imaj ismi, çalıştırdığı komut, çalışma süresi, durumu, kullandığı portlar gibi faydalı bilgiler de sağlar.<br>
Bu halinde sadece çalışan containerları listelerken -a bayrağı eklenirse tüm containerları listeler.<br>
- **docker run**<br>
En temel komutlarımızdan biri olan `run` komutu bir tane imajdan bir container çalıştırmamızı sağlar.<br>
`docker run imajismi:etiket komut` şeklinde çalıştırılırsa containerda istenilen herhangi bir komut o komut containerda bulunduğu sürece çalıştırılabilir. Alabildiği bayraklar aşağıda bulunan Çeşitli Bayraklar başlığında incelenecektir.
- **docker create**<br>
`run` ile aynı işlevdedir, tek farkı `run` containerı hemen çalıştırırken `create`'in containerı hemen çalıştırmamasıdır.<br>
- **docker start|stop**<br>
`run` veya `create` ile oluşturulan bir containerın sırasıyla başlatılmasını (`docker start containerismi`) ya da durdurulmasını (`docker stop containerismi`) sağlar.<br>
- **docker pull**<br>
Ardından gelecek imaj ismi ve etiketini indirir ve saklar.<br>
Kullanımı `docker pull imajismi:etiket` şeklindedir.<br>
- **docker images**<br>
Bilgisayarda bulunan (indirilmiş veya buildlenmiş) tüm imajları isimleriyle, etiketleriyle, idleriyle, oluşturulma zamanlarıyla ve boyutlarıyla beraber listeler.<br>
Kullanımı `docker images` veya `docker image ls` şeklindedir.<br>
- **docker rmi**<br>
Belirtilen imajı siler.<br>
Kullanımı `docker rmi imajismi:etiket` veya `docker rmi imajidsi` şeklindedir.<br>
imajidsi `docker images komutu` ile elde edilebilir.<br>
- **docker exec**<br>
Çalışan bir containerda komut çalıştırmayı sağlar.<br>
Kullanımı `docker exec containerismi komut` şeklindedir.<br>
Çeşitli Bayraklar başlığında incelenecek çeşitli bayraklar alabilir.<br>
- **docker rm**<br>
Durmuş bir containerın silinmesini sağlar.<br>
Kullanımı `docker rm containerismi` şeklindedir.<br>


### Çeşitli Bayraklar
Linux'taki aşağı yukarı her komutta olduğu gibi Docker'da da belli bayraklar bulunmaktadır ve bu bayraklar çok işe yarar şeyler olabilir. 
- *--detach | -d*<br>
`run` veya `exec` ile çalıştırılabilir, komutun arkaplanda çalıştırılmasını sağlarve terminali meşgul etmez.<br>
- *--interactive| -i*<br>
Bağlı olunmasa bile STDIN'i açık tutar, --tty | -t ile çok iyi arkadaşlardır ve genelde birlikte kullanılırlar.<br>
- *--tty | -t*<br>
Containerda çalıştırdığınız komut için sahte bir terminal oluşturur, --interactive ile çok iyi arkadaşlardır ve beraber kullanıldığı zaman containerda çalıştırılan bir komutu interaktif olarak (mesela başka bir kabuk) kullanmanızı sağlar.<br>
- *--rm*<br>
Containerın tek seferlik çalıştırılmasını, ardından yok edilmesini sağlar.<br>

- *--publish | -p*<br>
Kullanımı `-p hostportu:containerportu` şeklindedir.<br>
Belirtilen container portu ile host portunun bağlanmasını sağlar. 80 portundan yayın yapan bir Nginx containerımız olduğunu düşünelim, ve biz bu Nginx'e kendi bilgisayarımızın 8081 portundan erişmek istiyoruz.<br>
Containerı oluştururken `-p 8081:80` şeklinde bir atama yaparsak containerın 80 portunu bilgisayarımızın 8081 portuna bağlarız. Containerın ve içindeki web sunucusunun çalışmasının ardından http://localhost:8081 adresine giderek container içindeki Nginx'imizin 80 portuna gidebiliriz.<br>

- *--volume | -v*<br>
Kullanımı `docker run -v "yolyadavolume:/containerdaki/adres" ubuntu:focal` şeklindedir.
Host ile container arasında yol veya volume paylaşımı yapılmasını sağlar. Bu sayede container'ın verileri container'ın silinme durumunda korunup daha sonra yeniden çalıştırılacak bir container'da kullanılabilir.<br>
`/containerdaki/adres` her zaman tam bir yol olmalıdır.<br>
Bunu gerçekleştirmenin yol ve volume paylaşımı olmak üzere iki yolu vardır.<br>
  - Yol paylaşımı:<br>
Containera yol paylaşmak için `yolyadavolume` yerine başında **/** olan tam bir yol (örn. `/srv/muazzamwebuygulamasi`) yazılmalıdır. Paylaşılan dizinlerde yapılan herhangi bir değişiklik anında diğer tarafa yansıtılır.
  - Volume paylaşımı:<br>
Containera volume paylaşmak için `yolyadavolume` yerine başında **/** olmayan bir kelime (örn. `muazzamwebuygulamasi-data`) yazılmalıdır.



## **Geliştirme**
### Neden Uygulamamızı Dockerize Etmeliyiz?
"Ama benim bilgisayarımda çalışıyor" serzenişini yok etmek ve yazdığınız MuazzamUygulamanın başka bilgisayarlarda çalıştırmaya çalışırken ortam farklılıkları sebebiyle ortaya çıkan sıkıntıları tamamen ortadan kaldırmak için mesela.<br>
Veya MuazzamUygulamamızın kurulumu için 10 milyor milyar adım çalıştırmak yerine sadece imajın olduğu bir container çalıştırılıp gerekli olan değişkenleri ortak bir konfigürasyon dosyası veya ortam değişkenleri yoluyla aktarabilip fiziğin doğasına aykırı bir şekilde hem zamandan hem de işten tasarruf etmek için de olabilir.<br>
Veya yukarıda **Container ile Sanal Makinenin Farkı** başlığında bahsettiğim yatay genişletmeyi yeni containerlar başlatarak çok rahat yapabilmek içindir.<br>
Belki de uygulamayı dockerlayınca havalı durur ve dockerlayamayan geliştiricilere hava atabilirsiniz, size kalmış.<br>

### Uygulama Dockerize Etme
Bir uygulamayı Linux üzerinde dockerize etmek için öncelikle uygulamamızın Linuxta çalışıyor olması gerekiyor, bunun yanı sıra uygulamamızın ihtiyacı olan tüm kütüphanelerin bir listesini elimizde bulundurmalıyız. <br>
Bir uygulamanın dockerizasyonu için gereken malzeme listesi
- Dockerfile
- Docker
- Bir adet geliştirici (sen)
- Aldığı kadar su

Dockerfile için gereken malzeme listesi:
- Kaynak imaj
- Çalıştıracağı komut
- Diğer minik detaylar
- Bunların hepsini yazacak bir adet geliştirici (sen)

![At Çizmek İçin Gerekenler](https://pics.me.me/how-to-draw-a-horse-by-van-oktop-draw-2-63818650.png "At Çizmek İçin Gerekenler")

Dockerfile anatomisine gelecek olursak, aşağıda örnek bir Dockerfile bulunmaktadır:<br>
```
FROM ubuntu:focal

RUN apt update && apt install python3 && apt autoclean

EXPOSE 8000

CMD ["python3 -m http.server"]
```

Python'ın HTTP sunucusunu çalıştıran bir imaj hazırlamak bu kadar basit. Her satırın ne yaptığına gelecek olursak:<br>
**FROM** imajismi:etiket<br>
imajismi isimli imajın etiket etiketinin bu imaj için baz alınmasını sağlar.<br> Herhangi bir imajın herhangi bir versiyonu olabilir, veya `FROM scratch` ile tamamen sıfırdan başlayabilirsiniz.<br>
**RUN** komut<br>
İmaj hazırlanırken bir komut çalıştırmanızı sağlar. Komutları mümkün olduğunca && kullanarak (komutların zincirleme çalıştırılmasını sağlar) tek satırda yazmaya dikkat edilmelidir. Bunun sebebi Docker'ın imaj oluştururken her bir talimatta farklı katman oluşturmasıdır, talimat sayısı azaltılarak imaj boyutu da azaltılır.<br>
**EXPOSE** portnumarası<br>
İmajın bir portunu kullanılabilir hale getirmenizi sağlar. Bu sayede container içinde bulunacak MuazzamWebUygulamamız erişilebilir hale gelir.<br>
**CMD** [komut1, komut2, ...]<br>
Container çalıştırıldığı zaman çalıştırılacak komutları belirtir.<br>

**Örneğin incelenmesi**
- FROM ubuntu:focal<br>
Ubuntu imajının focal etiketini baz alır, bu sayede apt ile paket kurabiliriz.<br>
- RUN<br>
`apt update` ile repoları günceller (ki sıfır imaj olduğu için bu gerekli yoksa paket ismi bulamaz), `apt install python3` ile python3 paketini kurar, `apt autoclean` ile indirdiği paketlerin kurulum dosyalarını temizler ki imaj boyutu küçülsün. `&&` ile komutları zincirler, `komut1 && komut2` ile zincirlenmiş iki komutta ilk komutun bitmesinin ardından ikinci komut çalışır.<br>
- EXPOSE<br>
`EXPOSE 8000` ile python3 http server'ın kullanacağı default port 8000 erişilebilir hale gelir, bu satır olmazsa container portunu paylaşsanız bile erişim sağlayamazsınız.<br>
- CMD<br>
Komutun tamamı `python3 -m http.server`'dır. Komut tek parça olarak veya parçalara bölünerek dizi şeklinde verilebilir.

