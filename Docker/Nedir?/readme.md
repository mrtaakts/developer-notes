# Docker Nedir?
Docker açık kaynaklı yeni nesil sanallaştırma platformudur. Aynı işletim sistemi üzerinde birbirinden bağımsız containerlar sayesinde sanallaştırma sağlayan bir teknolojidir.
<br /><br />

![Docker nedir](https://kodedu.com/wp-content/uploads/2015/06/vm-vs-docker-container.png)

## Container

Docker Daemon tarafından Linux çekirdeği içerisinde birbirinden izole olarak çalıştırılan process’lerin her birine verilen isimdir. Virtual Machine (Sanal Makina) analojisinde Docker’ı Hypervisor’e benzetirsek fiziksel sunucu üzerinde halihazırda koşturulmakta olan her bir işletim sisteminin (sanal sunucunun) Docker’daki karşılığı Container’dır. Container’lar milisaniyeler içerisinde başlatılabilir, istenen herhangi bir anda duraklatılabilir (Pause), tamamen durdurulabilir (Stop) ve yeniden başlatılabilirler.

## Image ve Dockerfile
![Docker nedir2](https://miro.medium.com/v2/resize:fit:1400/0*qgp3qPufxJepi5jc)

Docker Daemon ile çalıştırılacak Container’ların baz alacağı işletim sistemi veya başka Image’ı, dosya sisteminin yapısı ve içerisindeki dosyaları, koşturacağı programı (veya bazen çok tercih edilmemekle birlikte programları) belirleyen ve içeriği metin bazlı bir Dockerfile (yazımı tam olarak böyle, ne dockerfile ne DockerFile ne de DOCKERFILE) ile belirlenen binary’ye verilen isimdir.

Hatırlarsanız Docker’ın doğuşunu anlattığımız ilk bölümde, Docker’ın oyunu değiştiren bir hamle yaparak LXC’ye göre fonksiyon kıstığını ve böylece başarılı olduğunu söylemiştik. İşte Docker, koşturulacak Container’ların iskeletini oluşturan her bir Image’ın bir Dockerfile ile tanımlanmasını gerekli kılar. Bu Dockerfile içerisinde Image’ın hangi Image’ı baz aldığı (miras aldığı), hangi dosyaları içerdiği ve hangi uygulamayı hangi parametrelerle koşturacağı açık açık verilir.

## Docker Daemon (Docker Engine)

Docker ekosistemindeki Hypervisor’ün tam olarak karşılığıdır. Linux Kernel’inde bulunan LXC’nin yerini almıştır. İşlevi Container’ların birbirlerinden izole bir şekilde, Image’larda tanımlarının yapıldığı gibi çalışmaları için gerekli yardım ve yataklığı yani ortamı sağlamaktır. Container’ın bütün yaşam döngüsünü, dosya sistemini, verilen CPU ve RAM sınırlamaları ile çalışması vb bütün karmaşık (işletim sistemine yakın seviyelerdeki) işlerin halledildiği bölümdür.

## Docker CLI (Command-Line Interface) - Docker İstemcisi

Kullanıcının Docker Daemon ile konuşabilmesi için gerekli komut setini sağlar. Registry’den yeni bir Image indirilmesi, Image’dan yeni bir Container ayağa kaldırılması, çalışan Container’ın durdurulması, yeniden başlatılması, Container’lara işlemci ve RAM sınırlarının atanması vb komutların kullanıcıdan alınarak Docker Daemon’e teslim edilmesinden sorumludur. Bu blog’un ilerleyen bölümlerinde sık sık Docker CLI’ı kullanarak Docker’ı tanımaya devam edeceğiz.

## Docker Registry

Zaten bir teknoloji harikası olan Docker’ı daha da kullanılabilir ve değerli kılan bir özellik bütün açık kaynak sistemler gibi paylaşımı özendirmesi, adeta işin merkezine koymasıdır.  [DockerHub](https://hub.docker.com/)‘da topluluğun ürettiği Image’lar ücretsiz ve sınırsız indirilebilir, oluşturulan yeni Image’lar gerek topluluk ile gerekse kişisel veya şirket referansı için açık kaynaklı (ücretsiz) veya kapalı kaynaklı (ücretli) yüklenebilir ve sonradan indirilebilir. Cloud’da hizmet veren DockerHub’ın yanında Image’larını kendi Private Cloud’unda tutmak isteyenler için Docker’ın sunduğu Private Registry hizmeti de vardır.

Sözün özü Container’lar Image’lardan oluşturulur. Image’larsa ortak bir eforun sonucu olarak meydana gelir ve Docker Registry’lerde tutulur. Örnek olarak, Ubuntu’nun üreticisi  [Canonical](http://www.canonical.com/)  DockerHub’da Official Repository’ler tutmakta ve Ubuntu versiyonlarını  [bu repository](https://hub.docker.com/r/library/ubuntu/)‘lerde yayınlamaktadır. Ubuntu Image’ını kullanarak bir Container oluşturmak isteyen kişiler direkt olarak bu Image’ı kullanabilirler. İkinci bir kullanım senaryosu olarak, sağlanan bu Ubuntu Image’ını kullanarak başka bir işlev gerçekleştiren, örneğin Nginx ile statik web server hizmeti veren, bir Image yaratıp bunu DockerHub’da yayınlamak, yani hem kendi hem de başkalarının kullanımına sunmak verilebilir.

## Docker Repository

Bir grup Image’ın oluşturduğu yapıdır. Bir repository’deki değişik Image’lar Tag’lanarak etiketlenir böylece değişik versiyonlar yönetilebilir. İlerleyen bölümlerde Image versiyonlama konusunda örnekler vereceğiz.

<br /> <br /> <br /> <br /> <br />

# Docker’ın Kullanım Alanları ve Çözmeye Aday Olduğu Problemler
Bu bölüm kısa gelecekte muhtemelen eskiyecek ve yeniden yazıma ihtiyaç duyacaktır fakat gün itibariyle eldeki durumun bir fotoğrafının çekilmesi açısından burada yazılanları kavramak diğer bölümlere göre daha önemlidir. Blog’un başında Motivasyon başlığında yer verilen videoların ilkinde Docker’ın fikir babası ve birinci adamı Solomon Hykes aslında projeyi neden ortaya koyduklarını DotCloud üzerinden anlatıyor. Temel olarak söylediği gittikçe artan cloud (bulut) gerensinimlerine cevap vermek üzere kurulan özellikle PaaS sağlayıcılardan biri olan ve Solomon’un da sahibi olduğu DotCloud, Linux Containers (LXC) kullanarak daha az maliyetli, daha performanslı ve daha az down-time’lı bir hizmet sunuyor. İnsanlar Solomon’a nasıl yaptıklarını sorunca Solomon, Linux Kernel’indeki Container desteğinin pek bilinmediği, bilinse bile etkili kullanılamadığı sonucunu çıkarıyor ve yaptıkları kurmaylar toplantısında LXC’yi halka indirmeye karar veriyorlar. Daha önce de yazdığımız gibi Kernel’in sunduğu Container desteğini bir Image formatı ile standardize edip Container’ın etrafında onun bütün yaşam döngüsünü basitleştiren ve yönetimini kolaylaştıran bir ekosistem inşa ediyorlar. Bu işleri o kadar hızlı ve doğru yapıyorlar ki rakipleri daha nefes alamadan neredeyse bitiş çizgisine ulaşıyorlar.

Yukarıdaki paragrafta Docker projesinin başlatılma gerekçesini küçük yorumlar ekleyerek başlatan kişinin ağzından nakletmeye çalıştım. Tabii Docker belki Solomon’un da en başta hatta ortalarında hayal ettiği çizginin de ötesine geçti, çok farklı alanlarda kendine çok geniş kullanım alanları buldu buluyor. Şimdi biraz bunların üzerinden gidelim.

## Benim Makinemde Çalışıyor (Works on my Machine) Problemine Çözüm Sağlaması

Yazılım geliştirme işi ile belirli bir süre uğraşan her yazılımcı, en az bir kere sahadan bildirilen problemi kendi geliştirme ortamında tekrarlayamama talihsizliğini yaşamıştır. Geliştirici ortamı ile uygulamanın deploy edildiği saha şartları arasında çoğu durumda gerek ölçek, gerek üzerinde koşulan platformun konfigürasyonu, gerekse de kullanıcı sayısı bakımından büyük farklılıklar bulunmaktadır. Docker’la birlikte uygulamalarımızı Docker altyapısı ile paketlediğimiz için aynı Image’ı hem geliştirici ortamında hem UAT (User Acceptance Test - Kullanıcı Kabul Test) ortamında ve PROD (Production - Canlı) ortamda kullanabiliriz. DEV (Development - Geliştirici) ve UAT ortamlarında sadece tek bir Container’ın çalıştırılması yeterli olacakken PROD ortamda gerektiği kadar Container ayağa kaldırılarak yük bir dengeleyici yardımıyla dağıtılabilir. Docker ile birlikte uygulamamızın koştuğu platformun DEV, UAT ve PROD ortamlarında aynı olmasını sağlamış ve platform farkından kaynaklı değişiklikleri de sıfırlamış oluruz. PROD ortamında Docker Container’da operasyon sırasında yapılmış bir değişiklik kolaylıkla geliştiricinin bilgisayarına yeni bir Image olarak getirebilir ve eğer oluşan problem ilgili değişiklerden kaynaklı ise geliştirici ortamında kolaylıkla tekrarlanabilir hale getirilir.

## Geliştirme Ortamı Standardizasyonu Sağlaması

Geliştirme ortamları projeden projeye farklılık göstermektedir. Aynı projenin farklı müşterilerde olan kurulumları bile farklı bileşenler içermekte müşterilerin birinde çıkan problemin çözümü için aynı ortamın geliştirici ortamında kurulması bile çok ciddi bir zaman almaktadır. Bazı yazılım evleri, farklı müşterilerde farklı sürümlerde bulunan kurulumları geliştirici ortamlarında tekrarlayabilmek için geliştirici ortamlarını sanal sunucularda tutmaktadır. Bir sanal makinanın ortalama 20GB olduğunu düşünürsek tek bir projenin 5 müşteride kurulu olması halinde her bir geliştiricinin bilgisayarında 100GB’lık bir alan gerekli olacaktır.

Sanal sunucu temin etmenin kaynak kullanımında yarattığı problemi görmezden geldiğimizde bile geliştirmesi aktif olarak devam eden projelerde her yapısal değişiklikte, web sunucu değişimi, database şema veya referans data değişikliği vb binary formatta olan master sanal makinanın bütün geliştiricilere dağıtılması ve varsa geliştiricilerin kendi özelleştirmelerini bu makinalara tekrar uygulamalarını gerektirmektedir.

Bir başka geliştirme ortamı problemi de sanal sunucu kullanılmayan geliştirme ortamlarında ekibe yeni katılan kişilerin bilgisayarlarının gerekli araç gereçlerle kurulması işlemi için harcanan zamandır. Bu işlem genellikle ekibin deneyimli bir elemanı tarafından yapılır, bu durumda hem ekibin deneyimli bir elemanının hem de işe yeni başlayan elemanın efektif olarak kullanabilecekleri bir zaman dilimi kaybedilmiş olur.

Docker’dan önce ortaya çıkan  [Vagrant](https://www.vagrantup.com/)  aslında yukarıda sıralanan bütün bu problemlere sanal sunucu bazlı bir çözüm sunmaktadır. Docker’ın ortaya çıkması ile birlikte Vagrant, sanal sunucu yerine Docker kullanılmasına da imkan sağlamaktadır. Vagrant yaptığı işte başarılı olması ve ilk olması dışında Docker blog serimizin  [üçüncü](https://gokhansengun.com/docker-compose-nasil-kullanilir/)  blog’unda anlatılacak olan Docker-Compose aracı ile yapamayacağımız fazla bir özellik sunmamakta dolayısıyla paletimize yeni bir araç eklememize yeterli gerekçeleri sunmamaktadır. Docker-Compose geliştirici ortamının yapısını metin tabanlı olarak tutar ve tek bir komutla  `docker-compose up`  geliştirici ortamını geliştiricinin çalışmasına hazır hale getirebilmektedir.

## Test ve Entegrasyon Ortamı Kurulumu ve Yönetimini Kolaylaştırması

Popülerleşen DevOps kavramı ile birlikte, CI (Continuous Integration - Sürekli Entegrasyon) ortamları her geçen gün daha fazla şirket ve geliştirme ekibi tarafından kullanılmaktadır. Docker’ın sağladığı bağımlılığı azaltan fonksiyonlar sayesinde yeni bir Continuous Integration Pipeline oluşturulması ve var olan CI’ların bakımının yapılması kolaylaşmakta ve CI için kullanılan araçlara (Jenkins, Travis CI, vb) bağımlılığı azaltmakta ve farklı CI araçları arasında geçiş yapmayı kolaylaştırmaktadır.

Bu blog serisinin bir parçası olmamakla birlikte ilerleyen zamanlarda Docker ile bir CI ortamı kurulumunun tekniği ve adımları ile ilgili de bir blog yazma planım var.

## Mikroservis Mimari için Kolay ve Hızlı Bir Şekilde Kullanıma Hazır Hale Getirilebilmesi


Docker, işletim sistemi çekirdeği seviyesinde bir sanallaştırma sağladığı için Hypervisor’lerle sağlanan sanallaştırmaya göre çok daha maliyetsiz (lightweight) ve hızlı bir sistem sunmaktadır. Hypervisor’lerle kurulan bir sanallaştırma altyapısında yeni bir node’un (sanal makina) sisteme eklenmesi için öncelikle işletim sisteminin hypervisor üzerinde boot edilerek başlatılmasının beklenmesi gerekmektedir. İşletim sisteminin başlatılması için ise ön yükleyicinin (boot loader) işletim sistemini belleğe yüklemesi, sanallaştırılan donanım bileşenlerinin (sanal disk, sanal ekran kartı, vb) kullanıma hazır hale getirilmesi ile işletim sistemi modüllerinin kullanıma hazır hale getirilmesinin beklenmesi gereklidir. Bütün bu işlemler en iyi ihtimalle 20-25 saniye sürmektedir. Docker ile yeni bir node (Container) eklenmesi ise milisaniyeler mertebesinde (50 - 100 ms) gerçekleştirilebilmektedir.

Mikroservis mimarilerde kullanım oranları artan servislerin kolaylıkla genişletilebilmesi yani yeni node’ların hızlı bir şekilde sisteme eklenebilmesi ve artık çok fazla kullanılmayan node’ların da hızlı bir şekilde kapatılarak kaynaklarının sisteme iade edilmesi gerekmektedir. Bu özellikler Docker’ın mikroservis mimariler için biçilmiş kaftan olmasını sağlamaktadır.

Açılıp kapanma performansına ek olarak Docker’ın teşvik ettiği her bir Container içinde sadece bir uygulamanın çalıştırılması zaten mikroservis sistemlerin sahip olması gereken ideal özelliklerin başında gelmektedir. Bir Container içinde sadece bir uygulamanın yani servisin çalıştırılması, ilgili servisin genişletilmesi (yeni node’lar eklenmesi) gerektiğinde, diğer servislerden bağımsız olarak ilgili Image’dan yeni Container’lar oluşturulark maliyet-etkin ve çakışma yaşanmayacak bir biçimde genişleme sağlanmasına da olanak tanımaktadır.

## Kaynakların Etkili ve Efektif Bir Biçimde Kullanılmasını Sağlaması

Hatırlarsanız tarihçe kısmında Docker’ın Hypervisor’lere göre donanım kaynaklarının nasıl daha etkin kullanılmasını sağlandığını örneklemiştik. Docker’ın uygulamaları birbirinden ayırmak için Hypervisor’ler gibi farklı işletim sistemlerine ihtiyaç duymaması, her bir uygulama için yeni bir işletim sistemi kurulması gereğini ortadan kaldırarak ciddi bir kaynak tasarrufu sağlamıştır.

Tarihçe bölümünde detaylandırılan ve yukarıda özetlenen özelliğe ek olarak Docker’ın kaynakların daha etkili kullanılması için sağladığı başka fonksiyonlar da vardır. Bilindiği gibi Hypervisor’ler tarafından sağlanan sanal makinaların fiziksel makinalara göre en önemli avantajlarından birisi aynı fiziksel sunucuyu mantıksal parçalara bölerek farklı amaçlar için kullanabilmeleridir. Hypervisor’lerde bulunan bu mekanizma büyük bir avantaj sağlamasının yanında değişen ihtiyaçlara cevap verme noktasında Docker kadar esnek değildir. Aynı fiziksel sunucu üzerinde verilen A, B ve C servislerinden A’nın CPU ihtiyacının arttığını ve artık daha güçlü başka bir sunucuya taşınması gerektiğini düşünelim. Hypervisor teknolojisi ile A sanal sunucusu öncelikle halihazırda çalıştığı fiziksel sunucuda durdurulmalı, yeni sunucusuna kopyalanmalı ve tekrar çalıştırılmalıdır. Taşıma işlemi sırasında uygulama ve data’sının yanında işletim sistemi de taşındığı için bu işlem uzun sürmekte ve genellikle bakım periyodunda (maintenance period) yapılmakta ve değişik ihtiyaçların karşılanması için esnek bir yapı sunmamaktadır. Docker’ın sunduğu sanallaştırma ile Container durdurulur, yeni sunucusuna taşınır ve hızlı bir şekilde yeniden başlatılabilir. Gereken süre Hypervisor’e oranla daha kısa olduğu için değişikliğin yapılması için bakım periyodunun gelmesi beklenmeyebilir.

## Multitenant Sistemlerde Tenancy Mantığını Uygulama Seviyesinden Çıkarmayı Sağlaması


Docker ile birlikte gelen az maliyetli ve esnek sanallaştırma ile birlikte artık uygulama kodlarının içerisine multitenancy (çok kiracılılık) mantığının konmasına gerek kalmamıştır. Multitenancy aynı uygulamanın birden çok müşteri için sanki farklı Instance’larda koşuyormuş izlenimi veren bir yazılım mimarisidir. Örnek olarak Konferans’lardaki oturum ve katılımcı bilgilerini yöneten bir uygulama, farklı müşterilerin farklı konferansları için tek bir instance’da hizmet verebilir. Her bir müşteri için ayrı birer web sunucu ve/veya uygulama sunucusu kurulması gerekmez böylelikle bakım maliyetleri düşürülür fakat uygulama seviyesinde Tenant’ları (kiracı) birbirinden ayıracak mantıklar (logic)’ler eklemek gerekmektedir ve bu da tahmin edebileceğiniz gibi hataya çok açık bir özelliktir. İşe yeni başlayan bir yazılımcı yeni tasarladığı bir ekranı Multitenancy özelliğini göz önünde bulundurmadan kodlarsa bütün tenant’lar diğer tenant’ların bilgilerini görebilir.

Docker ile birlikte Tenancy mantığı uygulama kodundan kaldırılabilir. Uygulama sadece tek bir tenant varmış gibi çalışacak şekilde öncekine göre daha basit bir şekilde tasarlanır. Her bir Tenant için ilgili Image’dan yeni bir Container yaratılarak Tenant ile ilişkilendirilir böylece Tenant yönetimi daha karmaşık olan uygulama seviyesinden alınarak platform seviyesine çekilmiş olur ve daha yönetilebilir bir altyapı sağlar.
