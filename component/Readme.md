# Component

Kubernetes, konteyner tabanlı uygulamaları yönetmek ve dağıtmak için popüler bir açık kaynaklı bir konteyner orkestrasyon platformudur. Kubernetes, uygulamaların güvenilir, ölçeklenebilir ve yüksek performanslı bir şekilde çalışmasını sağlamak için bir dizi özellik ve bileşen sunar. Bu bileşenler, Kubernetes'in komponent yapısını oluşturur.

## Master Komponentleri

Kubernetes cluster'ının kontrol düğümünü oluşturan ve cluster üzerindeki tüm kaynakların yönetimini sağlayan komponentlerdir.

- API Sunucusu (API Server): Kubernetes cluster'ıyla etkileşime geçmek için kullanılan merkezi bir arabirim sağlar. Kullanıcılar ve diğer komponentler, API sunucusu aracılığıyla Kubernetes ile etkileşimde bulunabilir.
- Etcd: Dağıtılmış bir key-value veritabanıdır ve Kubernetes cluster konfigürasyonu, durumu ve diğer meta verileri saklamak için kullanılır.
- Scheduler: Yeni oluşturulan pod'ların uygun nodlara (çalışma birimlerine) dağıtılmasından sorumludur. Scheduler, kaynak talepleri, afinite (node tercihleri) ve başka kısıtlamalar gibi faktörleri dikkate alarak en uygun nodları seçer.
- Kontrol Yöneticisi (Controller Manager): Pod, replica set, deployment gibi Kubernetes kaynaklarının durumunu sürekli olarak izleyen ve istenen duruma getirmek için gerekli eylemleri gerçekleştiren bir komponenttir.
  API İzleyici (API Server'in proxy'si): API sunucusu ile etkileşimde bulunmak için kullanılır ve API sunucusuna gelen talepleri dağıtmak için kullanılır.

## Node (İşçi) Komponentleri

Kubernetes cluster'ının çalıştığı ve uygulama konteynerlerinin çalıştığı fiziksel veya sanal makinelerdir.

- Kubelet: Her bir node üzerinde çalışan ve Kubernetes API'siyle iletişim kuran ana komponenttir. Kubelet, pod'ların çalışmasını sağlamak, sağlık durumunu kontrol etmek, talepleri node'a iletmek ve container'ları yönetmek gibi görevleri yerine getirir.
- Container Runtime: Konteynerleri çalıştırmak için kullanılan yazılımdır. Docker, Containerd, CRI-O gibi farklı konteyner çalışma zamanları Kubernetes tarafından desteklenir.
- Kube-Proxy: Pod'lara ağ erişimi sağlar ve ağ trafiklerini yönlendirir. Kube-Proxy, Service'lerin load balancing özelliğini de sağlar.

## Eklenti Komponentleri

Ek özellikler sunmak veya özelleştirmeler yapmak için kullanılan komponentlerdir.

- Network Plugin: Kubernetes cluster'ı içinde pod'ların ağa erişimini sağlayan ve ağ politikalarını uygulayan bir eklentidir. Calico, Flannel, Weave
  gibi farklı network plugin'leri kullanılabilir.
- Storage Plugin: Pod'lara kalıcı veri depolamak için kullanılan bir eklentidir. Bu plugin'ler, farklı veri depolama sistemlerini Kubernetes'e entegre etmek için kullanılır.

Bu komponentler, Kubernetes cluster'ının temel yapıtaşlarını oluşturur ve her biri farklı bir görevi yerine getirir. Birlikte çalışarak uygulama konteynerlerinin güvenilir ve ölçeklenebilir bir şekilde dağıtılmasını, yönetilmesini ve izlenmesini sağlarlar.

Örnek bir senaryoda, kullanıcılar API sunucusuna talepler göndererek bir uygulamanın dağıtılmasını veya ölçeklendirilmesini isteyebilir. API sunucusu bu talepleri alır, etcd üzerindeki konfigürasyonları günceller ve Scheduler, yeni pod'ları uygun nodlara yerleştirir. Kubelet, pod'ları başlatır ve Kube-Proxy, ağ erişimini sağlar. Kontrol Yöneticisi, uygulama kaynaklarını sürekli olarak izler ve istenen duruma getirmek için gerekli eylemleri gerçekleştirir.

Bu şekilde, Kubernetes komponent yapısı, bir uygulamanın dağıtılması, ölçeklendirilmesi ve yönetilmesi sürecini sağlamak için birlikte çalışan bir dizi bileşeni içerir. Bu yapı, yüksek kullanılabilirlik, ölçeklenebilirlik ve dayanıklılık gibi önemli özellikleri sunar ve konteyner tabanlı uygulamaların etkili bir şekilde yönetilmesine olanak sağlar.
