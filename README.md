<h1 align="center">
  <img alt="logo" src="https://cdn.dribbble.com/users/530731/screenshots/14753568/kuber-spave_2.png" width="224px"/><br/>Kubernetes Türkçe Doküman
</h1>

🚀 Bu GitHub Reposunda amacım Kubernetes'e dair öğrendiğim bilgileri (makalelerden, kurslardan, notlarımdan) paylaşarak bu alana daha hızlı ve bilgilini girmeni sağlamaktır. Zamanında araştırırken zorlandığım ve vakit alan kısımlarında içerisine alan bu repo sayesinde umarım, DevOps kariyerine Kubernetes teknolojisini hızla katabileceksin

⭐️ Repoyu beğenirseniz daha fazla kişiye ulaşması için yıldız atabilir ve sosyal medya hesaplarınızda paylaşabilirsiniz

⚡️ Bu eğitim serisini aynı zamanda [Kubernetes Dokümanı](https://docs.furkangulsen.com/kubernetes) web sitesinden de takip edebilirsiniz

## Bölümler:

- [Component](#component)
- [minikube](#minikube)
- [Pod](#pod)
- [Label](#label)
- [Annotation](#annotation)
- [Namespace](#namespace)
- [Deployment](#deployment)
- [Replicaset](#replicaset)
- [Rollout ve Rollback](#rollout-ve-rollback)
- [Ağ Kuralları](#ağ-kuralları)
- [Service](#service)
- [Liveness Probe](#liveness-probe)
- [Readiness Probe](#readiness-probe)
- [Resource Limits](#resource-limits)
- [Environment Variables](#environment-variables)
- [Volume](#volume)
- [Node Affinity](#node-affinity)
- [Pod Affinity](#pod-affinity)

---

<br>

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

<br><br>

# Minikube

Minikube, Kubernetes'in yerel bir sürümünü çalıştırmak ve geliştirmek için kullanılan bir araçtır. Minikube, kullanıcıların tek bir bilgisayarda Kubernetes'i keşfetmelerine, uygulamalarını test etmelerine ve yerel olarak geliştirmelerine olanak tanır. Ayrıca, birden fazla Kubernetes versiyonunu destekleyerek farklı versiyonlar arasında geçiş yapmayı da kolaylaştırır.

Minikube, tek bir düğüm (node) üzerinde çalışan basit bir Kubernetes cluster'ı oluşturur. Bu düğüm, yerel bir sanal makine (Virtual Machine - VM) veya konteyner çalıştırma ortamı (container runtime) üzerinde çalışabilir. Minikube, tüm Kubernetes bileşenlerini (master, etcd, kubelet, kube-proxy vb.) otomatik olarak yapılandırır ve başlatır.

## Özellikler

- Hızlı Kurulum ve Başlatma: Minikube, hızlı bir şekilde kurulabilen ve başlatılabilen basit bir araçtır. Kullanıcılar, tek bir komutla Minikube'ı çalıştırabilir ve hızlıca yerel bir Kubernetes cluster'ını kullanıma hazır hale getirebilir.

- Esnek Konfigürasyon Seçenekleri: Minikube, çeşitli konfigürasyon seçenekleri sunar. Bellek boyutu, CPU sayısı, Kubernetes versiyonu gibi özellikleri isteğe bağlı olarak ayarlayabilirsiniz. Bu, farklı uygulama senaryolarına uygun cluster'lar oluşturmanızı sağlar.

- Multi-Node Desteği: Minikube, tek düğüm (node) üzerinde çalışmasının yanı sıra birden fazla düğümü destekleyebilir. Böylece, birden fazla Minikube düğümünü birleştirerek daha karmaşık ve ölçeklenebilir senaryoları simüle edebilirsiniz.

<br><br>

# Pod

Pod, Kubernetes ortamında bir veya daha fazla konteynerin bir araya getirildiği en küçük dağıtım birimidir. Pod, aynı evsahibi ortamı paylaşan ve aynı ağ ve depolama kaynaklarını kullanabilen konteynerleri gruplar. Bu sayede birbirleriyle iletişim kurabilir ve birlikte çalışabilirler.

## Pod'un Avantajları

- **İzolasyon**: Her Pod, kendi IP adresi, bellek alanı, dosya sistemleri ve diğer kaynaklarıyla tamamen izole edilmiştir. Bu, Pod içindeki konteynerlerin birbirlerini etkilemeden çalışmasını sağlar.

- **Birlikte Dağıtım**: Bir Pod içindeki konteynerler, aynı zaman dilimi içinde ve aynı fiziksel veya sanal makinede dağıtılır. Bu, konteynerlerin birlikte çalışma yeteneklerini artırır ve performansı iyileştirir.

- **İletişim ve Paylaşım**: Pod içindeki konteynerler, localhost üzerinden birbirleriyle iletişim kurabilir. Ayrıca, Pod içindeki konteynerler aynı depolama kaynaklarını ve ağ bileşenlerini paylaşabilir.

- **Yüksek Erişilebilirlik**: Podlar, birden çok örneğini çalıştırarak yüksek kullanılabilirlik sağlayabilir. Bir Pod'daki bir konteyner çöktüğünde, diğer konteynerler hala çalışmaya devam eder.

## Pod Özellikleri

- **IP Adresi**: Her Pod, Kubernetes ağındaki benzersiz bir IP adresine sahiptir. Pod içindeki tüm konteynerler bu IP adresini kullanarak birbirleriyle iletişim kurabilir.

- **Konteynerler**: Bir Pod içinde bir veya daha fazla konteyner barındırılabilir. Bu konteynerler, birlikte çalışmak için oluşturulan mikro hizmetlerin parçalarıdır.

- **Depolama Kaynakları**: Pod, depolama alanı olarak kullanılabilen bir veya daha fazla birimde depolama kaynaklarına sahip olabilir. Bu, pod içindeki konteynerlerin veri paylaşmasını sağlar.

- **Ağ Ayarları**: Pod'un birincil IP adresi dışında, diğer ağ ayarlarına da sahip olabilir. Bu ayarlar, Pod'un dış dünyayla iletişimini ve diğer Pod'larla iletişimini sağlar.

- **Hedeflenen Birim**: Bir Pod, Kubernetes tarafından yönetilen bir çalıştırma birimidir ve çeşitli yönetim operasyonlarının hedefidir. Örneğin, ölçeklendirme, otomatik yeniden başlatma ve dağıtım işlemleri Pod üzerinde gerçekleştirilebilir.

## Pod'ların Kullanım Senaryoları

- **Tek Konteyner Pod'ları**: Bir Pod, yalnızca bir konteyner içeriyorsa, tek konteyner Pod'u olarak adlandırılır. Bu senaryoda, konteyner içindeki uygulama doğrudan çalıştırılır.

- **Çok Konteyner Pod'ları**: Bir Pod, birden çok konteyner içeriyorsa, çok konteyner Pod'u olarak adlandırılır. Bu senaryoda, birincil konteyner yanında yardımcı konteynerler de çalıştırılabilir. Yardımcı konteynerler, birincil konteynerin yan görevlerini gerçekleştirebilir.

## Pod Yaratma ve Yönetme

Pod'lar, Kubernetes API'sini kullanarak tanımlanır ve yönetilir. Pod oluşturulurken, Pod'a ait konteynerler, depolama kaynakları ve ağ ayarları tanımlanır. Bu Pod tanımı, Kubernetes tarafından yorumlanır ve uygun kaynaklar ayrılır. Pod'un durumu izlenir ve gerektiğinde yeniden başlatılır veya ölçeklendirilir.

Pod'lar, genellikle bir Replication Controller, ReplicaSet veya Deployment gibi daha yüksek seviye bir kaynak tarafından oluşturulur ve yönetilir. Bu, Pod'ların ölçeklenebilirlik, yüksek kullanılabilirlik ve güncelleme yetenekleri sağlar.

<br><br>

# Label

Label'lar, Kubernetes ortamında kaynaklara (örneğin Pod'lar, Servisler, ReplicaSet'ler vb.) metaveriler eklemek için kullanılan anahtar-değer çiftleridir. Bir kaynağa bir veya daha fazla Label atanabilir ve bu Label'lar, kaynakların gruplandırılması, sorgulanması ve seçilmesi için kullanılabilir. Label'lar, birçok farklı amaç için kullanılabilir, örneğin çevresel ayrımlar, uygulama bileşenleri, versiyon kontrolü vb.

Label'lar, kaynak tanımlama dosyalarında veya Kubernetes API'si aracılığıyla tanımlanır. Label'lar, genellikle key-value (anahtar-değer) çifti olarak ifade edilir. Örneğin, bir Pod'a "app=backend" veya "env=production" gibi Label'lar atanabilir.

```yaml
metadata:
  labels:
    key1: value1
    key2: value2
```

## Label'ların Avantajları

- Gruplama ve Kategorizasyon: Label'lar, kaynakların belirli kriterlere göre gruplandırılmasını sağlar. Örneğin, bir uygulama içindeki tüm frontend Pod'larını bir Label aracılığıyla gruplayabilirsiniz.

- Seçme ve Filtreleme: Label'lar, belirli özelliklere veya koşullara sahip kaynakları seçmek veya filtrelemek için kullanılabilir. Örneğin, "env=production" Label'ına sahip olan tüm kaynakları seçmek istediğinizde bu Label'ı kullanabilirsiniz.

- Versiyon Kontrolü: Label'lar, kaynakların versiyon kontrolü için kullanılabilir. Örneğin, bir uygulama için farklı sürümleri etiketleyerek, kaynaklarınızı kolayca yönetebilirsiniz.

## Uygulama

Örnek Senaryo: Bir e-ticaret uygulaması için frontend ve backend olmak üzere iki ayrı uygulama bileşeni bulunmaktadır. Bunları Kubernetes ortamında dağıtmak ve yönetmek istiyoruz.

1. Öncelikle, frontend ve backend uygulamaları için ayrı ayrı Pod tanımları oluşturuyoruz. Frontend uygulaması için "app=frontend" ve backend uygulaması için "app=backend" şeklinde Label'ları tanımlıyoruz.

```yaml
# frontend-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: frontend-pod
  labels:
    app: frontend
spec:
  containers:
    - name: frontend-container
      image: frontend-image

# backend-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: backend-pod
  labels:
    app: backend
spec:
  containers:
    - name: backend-container
      image: backend-image
```

2. Kaynak tanımlarını Kubernetes'e uyguluyoruz:

```bash
kubectl apply -f frontend-pod.yaml
kubectl apply -f backend-pod.yaml
```

3. Oluşturulan Pod'ları kontrol ederken, Label'ları kullanarak belirli bileşenleri seçebiliriz:

```bash
kubectl get pods -l app=frontend
kubectl get pods -l app=backend
```

4. Eğer frontend uygulamasında bir güncelleme yapmak istiyorsak, Pod tanımında değişiklik yaparız ve güncellemeyi uygularız:

```bash
kubectl apply -f frontend-pod.yaml
```

Bu şekilde, Label'lar sayesinde frontend ve backend uygulamalarını kolayca gruplayabilir, seçebilir ve güncelleyebiliriz. Ayrıca, farklı sürümleri veya çevreleri temsil etmek için ek Label'lar ekleyebilir ve bu şekilde kaynakları daha iyi yönetebiliriz.

<br><br>

# Annotation

Annotation'lar, Kubernetes ortamında kaynaklara (örneğin Pod'lar, Servisler, Deployment'lar vb.) ilişkili olmayan metaveriler eklemek için kullanılan key-value (anahtar-değer) çiftleridir. Annotation'lar, Label'lar gibi kaynaklara metaveri eklemek için kullanılır, ancak Label'ların aksine kaynakların gruplandırılması veya seçilmesinde kullanılmazlar. Annotation'lar, kaynaklara ilgili ek bilgileri eklemek için kullanılır ve genellikle açıklama, sürüm numarası, oluşturma tarihi gibi bilgileri içerir.

## Annotation'ların Avantajları

- **Ek Bilgi Saklama**: Annotation'lar, kaynaklarla ilgili ek bilgileri saklamak için kullanılır. Bu, kaynakların anlaşılmasını ve yönetilmesini kolaylaştırır. Örneğin, bir Pod'un neden oluşturulduğu veya ne işe yaradığı gibi açıklamaları Annotation'lar aracılığıyla ekleyebilirsiniz.

- **Entegrasyon ve İş Akışı**: Annotation'lar, farklı araçlar, hizmetler veya otomasyon iş akışlarıyla entegrasyon sağlamak için kullanılabilir. Örneğin, CI/CD araçları, Annotation'ları okuyarak ve onlara göre işlem yaparak süreçlerinizi otomatikleştirebilir.

- **Üçüncü Taraf Araçlar ve Uzantılar**: Annotation'lar, Kubernetes ekosistemindeki üçüncü taraf araçlar veya uzantılar tarafından kullanılmak için kullanılabilir. Bu, farklı hizmetleri veya özellikleri etkinleştirmek veya yapılandırmak için kullanılabilir.

## Annotation'ların Kullanımı ve Yönetimi

Annotation'lar, kaynak tanımlama dosyalarında veya Kubernetes API'si aracılığıyla tanımlanır. Annotation'lar, kaynak oluşturulurken veya daha sonra kaynağa eklenebilir. Ayrıca, Annotation'lar güncellenebilir ve silinebilir.

Kubernetes, Annotation'ları doğrudan kullanmasa da, üçüncü taraf araçlar veya uzantılar tarafından kullanılarak çeşitli işlemler gerçekleştirilebilir. Örneğin, bir izleme aracı, belirli Annotation'lara sahip olan kaynakları izleyebilir veya bir otomasyon aracı, Annotation'ları kullanarak belirli işlemler gerçekleştirebilir.

<br><br>

# Namespace

Kubernetes'de Namespaces, bir Kubernetes kümesi içinde kaynakların mantıksal olarak izole edilmesini sağlayan bir kavramdır. Bir Namespace, birçok Kubernetes nesnesini (örneğin Pod'ları, Service'leri, Deployment'ları) gruplandırmak için kullanılır ve bu nesnelerin benzersiz bir ad alanında çalışmasını sağlar.

Namespaces, farklı takımlar, projeler veya ortamlar arasında kaynak çakışmasını önlemek için kullanılabilir. Örneğin, bir firma içindeki farklı ekipler kendi Namespaces'lerinde çalışabilir ve birbirlerinin kaynaklarını etkilemeden kendi uygulamalarını yönetebilirler. Aynı şekilde, farklı ortamlar (örneğin, geliştirme, üretim) için farklı Namespaces'ler oluşturarak bu ortamlar arasında izolasyon sağlanabilir.

Namespaces, varsayılan olarak "default" adında bir Namespace içerir. Ancak, ihtiyaçlarınıza göre kendi özel Namespaces'lerinizi oluşturabilirsiniz. Bir Namespace oluşturduğunuzda, o Namespace içindeki kaynaklar, diğer Namespaces'lerin kaynaklarından izole edilir. Bu, kaynakların birbirleriyle çakışmadan çalışmasını sağlar.

Namespaces aynı zamanda erişim kontrolünü kolaylaştırır. Örneğin, farklı ekipler arasında roller ve izinler atanabilir ve bu ekipler sadece kendi Namespaces'leri üzerinde çalışabilir. Bu şekilde, güvenlik artırılır ve bir ekip diğer ekiplerin kaynaklarına erişemez.

## Uygulama

Bir e-ticaret platformu üzerinde çalışan bir Kubernetes kümesini ele alalım. Bu platformda, hem kullanıcı arayüzü hem de arka plandaki hizmetler bulunmaktadır. Projenizde, farklı ekiplerin bu platform üzerinde çalışacağını ve her bir ekibin kendi kaynaklarını izole etmek istediğini varsayalım.

- Öncelikle, farklı ekiplerin projelerini izole edebilmeleri için her bir ekip için ayrı bir Namespace oluşturun. Örneğin, "frontend-team" ve "backend-team" gibi isimler kullanabilirsiniz.

- "frontend-team" Namespace'i içinde, kullanıcı arayüzüne ait olan Pod'ları, Service'leri ve Deployment'ları gruplandırın. Bu, frontend ekibinin kendi kaynaklarını yönetebilmesini ve diğer ekiplerin kaynaklarından etkilenmemesini sağlar.

- Benzer şekilde, "backend-team" Namespace'i içinde, arka plandaki hizmetlere ait olan Pod'ları, Service'leri ve Deployment'ları gruplandırın. Bu, backend ekibinin kendi kaynaklarını yönetebilmesini ve diğer ekiplerin kaynaklarından etkilenmemesini sağlar.

- Her bir Namespace için uygun rol ve izinler tanımlayın. Örneğin, "frontend-team" için sadece frontend ekibine ait kullanıcıların erişebileceği bir rol tanımlayabilirsiniz. Benzer şekilde, "backend-team" için sadece backend ekibine ait kullanıcıların erişebileceği bir rol tanımlayabilirsiniz. Bu şekilde, her bir ekip kendi Namespace'i içinde çalışabilir ve diğer ekiplerin kaynaklarına erişimi kısıtlanmış olur. İzolasyon ve güvenlik sağlanmış olur.

- Her bir Namespace içindeki kaynaklar, kendi Namespace'i içindeki diğer kaynaklarla ilişkilidir. Örneğin, "frontend-team" Namespace'i içindeki Pod'lar, sadece aynı Namespace içindeki Service'lere yönlendirilebilir. Bu sayede, bir ekibin kaynakları diğer ekibin kaynaklarından etkilenmez ve her bir ekip kendi Namespace'i içinde kendi kaynaklarını yönetebilir.

## Dikkat Edilmesi Gerekenler

- Mantıklı Adlandırma: Namespaces'i oluştururken, anlaşılır ve tanımlayıcı isimler kullanmaya özen gösterin. İsimlendirme, Namespaces içindeki kaynakların amacını veya sahibini anlatmalıdır. Böylece, başkaları Namespace'in içeriğini daha kolay anlayabilir.

- İzolasyonu Sağlamak: Namespaces, kaynakları birbirinden izole etmek için kullanılır. Farklı Namespaces'ler arasında kaynak çakışmasını önlemek için her Namespace'e yalnızca ihtiyaç duyduğu kaynakları yerleştirin. Aşırı kaynak yüklemesi veya gereksiz bağımlılıklar, küme performansını etkileyebilir.

- Güvenlik ve Erişim Kontrolü: Namespaces, güvenlik ve erişim kontrolü sağlamak için kullanılabilir. Farklı ekipler veya kullanıcılar arasında roller ve izinler atanabilir. Her bir Namespace, kaynaklara erişim yetkilerini kısıtlayarak izolasyonu güçlendirir.

- Kaynakların Gruplandırılması: Kaynakları Namespaces içinde mantıklı bir şekilde gruplandırmak önemlidir.

<br><br>

# Deployment

Deployment, Kubernetes'in temel bileşenlerinden biridir ve uygulamalarınızı Kubernetes kümelerinde çalıştırmak için kullanılır. Deployment'lar, uygulama özelliklerinizi, dağıtım stratejilerinizi ve ölçeklendirme seçeneklerinizi tanımlamanıza yardımcı olan bir YAML dosyası kullanılarak oluşturulur.

Bir Deployment oluşturduğunuzda, Kubernetes, belirli bir uygulama sürümünü çalıştırmak için bir dizi Pod oluşturur ve yönetir. Bir Pod, birden çok konteynerden oluşabilen en küçük çalıştırılabilir birimdir. Deployment, istediğiniz sayıda Pod'un oluşturulmasını sağlar ve aynı zamanda bu Pod'ların sürekli olarak çalışmasını sağlar.

Deployment'lar ayrıca hata durumlarına karşı otomatik iyileştirme sağlayabilir. Eğer bir Pod başarısız olursa, Deployment bunu algılar ve otomatik olarak yeni bir Pod oluşturarak hizmetin kesintisiz devam etmesini sağlar.

Bir Deployment ayrıca güncelleme süreçlerini yönetmenize de olanak tanır. Örneğin, uygulamanızın yeni bir sürümünü yayınlamak istediğinizde, Deployment stratejilerini kullanarak güncellemeyi kontrollü bir şekilde gerçekleştirebilirsiniz. Bu stratejiler, yavaş yavaş güncelleme, aynı anda tümünü güncelleme veya sıfır kesintiyle güncelleme gibi farklı yaklaşımları destekler.

Son olarak, Deployment'lar, ölçeklendirme işlevselliği sağlar. Uygulamanız yoğun trafik altında daha fazla kaynak talep ederse, Deployment onu otomatik olarak ölçeklendirerek yükü dengeleyebilir. Böylece, uygulamanızın performansı ve kullanılabilirliği artar.

## Uygulama

### Adım 1: Kaynak Tanım Dosyası (deployment.yaml) Oluşturma

İlk adımda, Deployment'ı tanımlayan bir Kaynak Tanım Dosyası (deployment.yaml) oluşturmanız gerekmektedir. Örnek olarak aşağıdaki YAML dosyasını kullanabilirsiniz:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: my-app-container
          image: my-app-image:latest
          ports:
            - containerPort: 8080
```

Bu YAML dosyası, "my-app-deployment" adında bir Deployment oluşturur. Bu Deployment, 3 replika Pod oluşturacak şekilde yapılandırılmıştır. "my-app" adında bir etiket kullanılarak Pod'lar seçilir ve bu Pod'lar için "my-app-container" adında bir konteyner oluşturulur. Konteyner, "my-app-image:latest" adlı bir imajı kullanarak 8080 portunda çalışır.

### Adım 2: Deployment'ı Kubernetes Kümelerine Uygulama

Oluşturduğunuz deployment.yaml dosyasını kullanarak Deployment'ı Kubernetes kümelerine uygulayabilirsiniz. Bunun için aşağıdaki komutu kullanabilirsiniz:

```shell
kubectl apply -f deployment.yaml
```

Bu komut, deployment.yaml dosyasını kullanarak Deployment'ı Kubernetes kümelerine uygular.

### Adım 3: Deployment Durumunu Kontrol Etme

Deployment'ın başarıyla uygulanıp uygulanmadığını kontrol etmek için aşağıdaki komutu kullanabilirsiniz:

```shell
kubectl get deployments
```

Bu komut, mevcut Deployment'ları listeler ve bunların durumunu gösterir. "my-app-deployment" adında bir Deployment listelenmeli ve "3/3" gibi bir replika sayısı göstermelidir, bu da 3 Pod'un başarıyla oluşturulduğunu gösterir.

### Adım 4: Pod'ların Durumunu Kontrol Etme

Deployment ile oluşturulan Pod'ların durumunu kontrol etmek için aşağıdaki komutu kullanabilirsiniz:

```shell
kubectl get pods
```

Bu komut, mevcut Pod'ları listeler ve bunların durumunu gösterir. "my-app-deployment-" ile başlayan adlara sahip 3 adet Pod listelenmeli ve durumları "Running" olmalıdır.

### Adım 5: Deployment Güncelleme

Uygulamanızı güncellemek için deployment.yaml dosyasında ilgili değişiklikleri yapın. Örneğin, imaj sürümünü değiştirin.

Son olarak, güncellenmiş deployment.yaml dosyasını kullanarak Deployment'ı güncellemek için aşağıdaki komutu kullanabilirsiniz:

```yaml
kubectl apply -f deployment.yaml
```

Bu komut, güncellenmiş YAML dosyasını kullanarak Deployment'ı günceller.

## Dikkat Edilmesi Gerekenler

- **Güvenliğe Dikkat Edin:** Deployment'larınızda güvenlik en önemli faktörlerden biridir. Konteyner imajlarınızı güvenli ve güncel tutun, ayrıca RBAC (Rol Bazlı Erişim Kontrolü) gibi güvenlik önlemlerini kullanın.

- **Kaynak Kısıtlamalarını Belirleyin:** Deployment'larınızda kaynak kısıtlamalarını (CPU, bellek vb.) belirleyerek konteynerlerinizin kaynak tüketimini kontrol altında tutun. Bu, kümelerinizin performansını ve istikrarını sağlar.

- **Otomatik İyileştirme Ayarlarını Yapın:** Deployment'larınızda Pod'larda hata oluştuğunda otomatik olarak iyileştirme yapacak şekilde ayarlar yapın. Bu, hizmet kesintilerini en aza indirir ve uygulamanızın sürekli olarak çalışmasını sağlar.

- **Dağıtım Stratejilerini Dikkatlice Seçin:** Yeni bir sürümü dağıtırken veya güncellerken, dağıtım stratejilerini dikkatlice seçin. Yavaş yavaş güncelleme, aynı anda tümünü güncelleme veya sıfır kesintiyle güncelleme gibi stratejileri kullanarak uygulamanızın kullanılabilirliğini etkilemeden güncellemeleri yönetin.

- **İzleme ve Günlükleme:** Deployment'larınızı izlemek için Kubernetes üzerinde uygun izleme araçlarını kullanın. Ayrıca, günlükleri toplamak ve analiz etmek için uygun günlükleme stratejilerini uygulayın.

<br><br>

# ReplicaSet

## ReplicaSet Nedir?

ReplicaSet, Kubernetes'te çalışan bir grup Pod'un belirtilen sayıda çalışmasını sağlamak için kullanılan bir Kubernetes kaynağıdır. ReplicaSet, Pod'ların sağlıklı bir şekilde çalışmasını sağlamak için Pod'ların gereksinimlerini tanımlayan bir şart tanımlar.
​

## Görevleri Nelerdir?

- **Belirtilen sayıda kopya oluşturmak:** ReplicaSet, özellikle yüksek kullanılabilirlik gerektiren uygulamalar için kullanılır. Belirtilen sayıda pod oluşturarak uygulamaların yüksek kullanılabilirliğini sağlar.
- **Pod'ların düzgün çalıştığından emin olmak:** ReplicaSet, belirtilen sayıda kopya oluşturarak, pod'ların her zaman düzgün çalıştığından emin olur. Bu sayede, pod'larda oluşabilecek hatalar veya arızalar durumunda, ReplicaSet yeni bir pod oluşturarak uygulamanın düzgün çalışmasını sağlar.
- **Otomatik ölçeklendirme:** ReplicaSet, uygulama trafiğindeki artışları algılayarak, otomatik olarak yeni pod'lar oluşturarak uygulamaların ölçeklendirilmesini sağlar. Böylece, uygulama trafiğindeki artışlar sonucu oluşabilecek performans sorunları önlenir.
- **Pod'ların dağıtımını yönetmek:** ReplicaSet, pod'ların dağıtımını yöneterek, birden fazla node'da çalışan pod'ların belirtilen sayıda kopya oluşturulmasını sağlar. Bu sayede, uygulamaların yüksek kullanılabilirliği artar ve performans sorunları önlenir.
- **Update'leri yönetmek:** ReplicaSet, uygulamaların yeni sürümlerinin yayınlanması durumunda, yeni sürümü pod'lara yükleyerek, update işlemlerini yönetir. Bu sayede, uygulama sürümlerinin yönetimi kolaylaşır ve uygulama güncellemeleri daha hızlı bir şekilde yapılabilir.
  ​

## Uygulama

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: my-replicaset
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: my-container
          image: nginx
```

Bu YAML dosyasında, bir ReplicaSet kaynağı tanımlanıyor. metadata kısmı, ReplicaSet'in ismini içerir. spec kısmı, replikaların sayısını belirler ve hangi pod'ların ReplicaSet'e ait olduğunu belirlemek için bir selector tanımlar.

Ayrıca, ReplicaSet altındaki tüm pod'lar için bir şablon (template) belirler.

Şablon kısmında, pod'ların özelliklerini belirleyen bir spec tanımlanır. Bu örnekte, tek bir container'ı olan nginx imajı kullanılmıştır. ReplicaSet'in 3 adet replika ile çalışacağı belirtilmiştir. Bu, ReplicaSet'in her zaman 3 replikaya sahip olacağı anlamına gelir.

Bu örnek dosyayı kullanarak, kubectl ile ReplicaSet'i Kubernetes kümenize uygulayabilirsiniz:

```shell
kubectl apply -f my-replicaset.yaml
```

ReplicaSet'i scale etmek için kubectl scale komutu kullanılır. Örneğin, ReplicaSet'inizi üç replica olarak oluşturduysanız, aşağıdaki komutu kullanarak 5 replica'ya scale edebilirsiniz:

```shell
kubectl scale rs my-replicaset --replicas=5
```

Bu komut, my-replicaset adlı ReplicaSet'in replica sayısını 5'e ayarlar. Eğer ReplicaSet'inizi daha önce isimlendirmediyseniz, kubectl get rs komutu ile ismini öğrenebilirsiniz.

Yeni replica'lar, ReplicaSet tarafından belirtilen pod template'ine göre oluşturulur ve Kubernetes cluster'ınızın kaynaklarına göre dağıtılır.

Eğer yapılan değişiklikeri geri almak isterseniz rollout undo komutunu kullanarak ReplicaSet ile uygulanan işlemi geri alabilirsiniz:

```shell
kubectl rollout undo deployment my-replicaset
```

## Dikkat Edilmesi Gerekenler

- ReplicaSet, Pod'ları kontrol eder ve bir kopya sayısı belirtilen durumun özelliğine uygun olarak ölçeklendirir.
- ReplicaSet, Pod'ların sürdürülebilirliğini sağlamak için Pod'ların yeniden başlatılmasını otomatik olarak yönetir.
- ReplicaSet, Pod'ların konumunu veya dağılımını kontrol etmez, sadece belirli bir sayıda kopyanın çalıştırılmasını sağlar.
- ReplicaSet, aynı zamanda RollingUpdate gibi güncelleme stratejileriyle de kullanılabilir.
  ReplicaSet'in Pod şablonu, Pod oluşturulduğunda kullanılan şablonu tanımlar.
- ReplicaSet, Pod'ları otomatik olarak oluşturur ve yönetir, bu nedenle Pod'ları doğrudan yönetmek yerine ReplicaSet üzerinden yönetmek daha uygun olabilir.

<br><br>

# Rollout ve Rollback

## Rollout Nedir?

- Bir uygulamanın yeni bir sürümünün dağıtımını başlatmak için kullanılır.
- Bir önceki sürümden farklı olarak yeni bir sürümde yer alan güncellemeleri sağlar.
- Yeni bir sürümün başarılı bir şekilde dağıtımı için bir dizi adımı otomatikleştirir.
- Yeni bir sürümün yavaş yavaş tüm pod'lara dağıtılmasını ve eski sürümün aşamalı olarak kaldırılmasını sağlar.
- Uygulamanın yeni sürümüne geçiş sırasında yaşanabilecek olası kesintileri en aza indirir.

## Rollback Nedir?

- Uygulamanın bir önceki sürümüne geri dönmek için kullanılır.
- Bir uygulama sürümünün yanlışlıkla veya hatalı bir şekilde dağıtıldığı durumlarda kullanılır.
- Uygulama sürümünün hızlı bir şekilde geri alınmasını ve bir önceki çalışan sürüme geçiş yapılmasını sağlar.
- Geri alma işlemini otomatikleştirir ve hızlı bir şekilde tamamlanmasını sağlar.

## Uygulama

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
  labels:
    app: myapp
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
        - name: myapp
          image: myapp:v1
          ports:
            - containerPort: 80
```

Bu deployment nesnesinde image alanı, myapp:v1 olarak tanımlanmıştır. Eğer bu image güncellenmek istenirse, örneğin myapp:v2 sürümüne geçmek istenirse, aşağıdaki komut kullanılabilir:

```shell
kubectl set image deployment/myapp-deployment myapp=myapp:v2
```

Bu komut, deployment nesnesindeki myapp isimli container için kullanılan image'ı myapp:v2 olarak güncelleyecektir. Bu güncelleme, varsayılan olarak, tüm replica setlerinde aynı anda gerçekleştirilir. Rollout işlemi sırasında, deployment nesnesinin status alanı güncellenir. Aşağıdaki komut kullanılarak deployment nesnesinin rollout durumu takip edilebilir:

```shell
kubectl rollout status deployment/myapp-deployment
```

Bu komut, deployment nesnesinin rollout işleminin tamamlanıp tamamlanmadığını kontrol eder. Eğer güncelleme işlemi tamamlanmışsa, aşağıdaki komut kullanılarak yeni image sürümüne uygun pod'ların listesi alınabilir:

```shell
kubectl get pods -l app=myapp
```

Bu komut, app=myapp label'ına sahip pod'ların listesini verir. Artık pod'ların yeni image sürümüne sahip olması beklenir. Eğer güncelleme işlemi hatalı bir şekilde sonuçlanırsa, rollback işlemi kullanılarak önceki sürüme geri dönülebilir.

## Strateji Belirleme

> Deployment'ın yeniden oluşturulma stratejisini strategy adında bir ifade belirler. Bu ifadenin default değeri RollingUpdate

RollingUpdate stratejisi, yeni bir versiyonu yavaş yavaş deployment'a dahil ederek eski versiyonun yerini değiştirirken, Recreate stratejisi mevcut tüm pod'ları öldürür ve yeni versiyon için tamamen yeni bir set oluşturur. RollingUpdate stratejisi, kullanıcılara sıfır downtime sağlamak için daha çok tercih edilirken, Recreate stratejisi daha basit bir yaklaşım sunar ve eski versiyondan tamamen kurtulmak istenildiğinde kullanılabilir.

### Recreate Kullanımı

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: rcdeployment
  labels:
    team: development
spec:
  replicas: 3
  selector:
    matchLabels:
      app: recreate
  strategy:
    type: Recreate # Recreate stratejisi burada tanımlanıyor
  template:
    metadata:
      labels:
        app: recreate
    spec:
      containers:
        - name: nginx
          image: nginx:1.20 # yeni bir sürüm tanımlanıyor
          ports:
            - containerPort: 80
```

Bu YAML dosyasında strategy bölümünde Recreate stratejisi tanımlandı. Bu durumda güncelleme işlemi sırasında, önceki versiyon silinir ve yeni versiyon oluşturulur. Bu, tüm pod'ların yeniden oluşturulması anlamına gelir ve bu nedenle geçici bir çalışmazlık süresine yol açabilir.

### RollingUpdate Kullanımı

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:1.19.3
          ports:
            - containerPort: 80
      terminationGracePeriodSeconds: 10
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1
      maxSurge: 1
```

Bu örnekte, Deployment nesnesi, spec bölümünde belirtilen özelliklere sahip bir ReplicaSet oluşturur. ReplicaSet daha sonra bir veya daha fazla Pod'u barındıracak şekilde scale edilir. strategy bölümünde, type: RollingUpdate olarak ayarlandığı için, yeni bir güncelleme yayınlandığında, Kubernetes önce yeni Pod'ları oluşturur ve eski Pod'ları siler, bu sayede sıfır kesinti süresi (zero-downtime) elde edilir. rollingUpdate bölümü, aynı anda kaç tane Pod'un güncellenebileceğini ve kaç tane yeni Pod oluşturulabileceğini belirtir. Bu örnekte, maxUnavailable değeri 1 ve maxSurge değeri de 1 olarak ayarlandı, bu da Kubernetes'in aynı anda sadece bir Pod'u devre dışı bırakacağı ve bir Pod daha oluşturacağı anlamına gelir.

### Record Kullanımı

**--record** parametresi, kubectl komutlarındaki değişikliklerin kaydedilmesine ve bu değişikliklerin sonrasında kullanılabilecek bir kayıt oluşturulmasına olanak tanır. Bu parametre, kubectl kullanarak bir nesne oluşturulduğunda, güncellendiğinde veya silindiğinde, bir kayıt oluşturulmasını sağlar.

Bu kayıt, sonrasında kubectl rollout history komutu kullanılarak geçmiş değişiklikleri görüntülemek veya bir önceki sürüme geri dönmek gibi işlemlerde kullanılabilir. Özellikle güncelleme işlemleri için kayıt oluşturmak, geri alma işlemlerinde oldukça faydalıdır.

Örnek olarak, kubectl create veya kubectl apply komutlarının sonuna --record parametresi eklenerek kayıt oluşturulabilir:

```yaml
kubectl apply -f deployment.yaml --record
```

### Geçmişe Dönme

Kubernetes'te Deployment'lar üzerinden kaydedilen kayıtların arasında geçiş yapmak için kubectl rollout undo komutu kullanılabilir. Aşağıdaki örnek, my-deployment adlı bir Deployment'ın son yapılandırmasına geri dönmek için bir kayıt geçişini gerçekleştirir:

```shell
kubectl rollout undo deployment/my-deployment
```

Bu komut, my-deployment Deployment'ının son yapılandırmasına geri döner. --record seçeneği kullanılarak, geri alma işleminin kaydı tutulabilir:

```shell
kubectl rollout undo deployment/my-deployment --to-revision=2 --record
```

Bu komut, my-deployment Deployment'ının 2. yapılandırmasına geri döner ve geri alma işleminin kaydını tutar. Daha sonra kubectl rollout history komutu kullanılarak kaydedilen kayıtların listesi görüntülenebilir:

```shell
kubectl rollout history deployment/my-deployment
```

Bu komut, my-deployment Deployment'ının tarihçesini görüntüler. --revision seçeneği kullanılarak belirli bir kaydın detaylarına bakılabilir:

```shell
kubectl rollout history deployment/my-deployment --revision=3
```

Bu komut, my-deployment Deployment'ının 3. kaydının detaylarını görüntüler.

## Status Kullanımı

Bu komutlar kullanıldığında, deployment üzerindeki güncelleme işleminin durumu kubectl rollout status komutu ile görüntülenebilir. Bu komut, deployment'in güncelleme işleminin ne durumda olduğunu gösterir ve güncelleme işlemi tamamlanana kadar bekleyebilirsiniz.

Örnek olarak, aşağıdaki komut ile myapp-deployment deployment'inin güncelleme işlemi durdurulabilir:

```shell
kubectl rollout pause deployment/myapp-deployment
```

Daha sonra, güncelleme işlemi devam ettirilebilir:

```shell
kubectl rollout resume deployment/myapp-deployment
```

## Pause ve Resume Kullanımı

Kubernetes'te deployment üzerinde yapılan bir güncelleme işlemi sırasında, belirli bir noktada işlemleri durdurmak ve sonra devam ettirmek isteyebilirsiniz. Bu durumda rollout pause ve rollout resume komutları kullanılır.

- **rollout pause:** Güncelleme işlemini durdurmak için kullanılır.

Örneğin, aşağıdaki komut ile deployment üzerindeki güncelleme işlemini durdurabilirsiniz:

```shell
kubectl rollout pause deployment/myapp-deployment
```

- **rollout resume:** Güncelleme işlemini devam ettirmek için kullanılır.

Örneğin, aşağıdaki komut ile deployment üzerindeki güncelleme işlemini devam ettirebilirsiniz:

```shell
kubectl rollout resume deployment/myapp-deployment
```

## Dikkat Edilmesi Gerekenler

1. Deployment'ın yaml dosyasındaki strateji kısmı doğru belirlenmelidir. Bu, deployment'ın nasıl yayınlanacağını, kaç pod'un çalışacağını, eski pod'ların nasıl ele alınacağını vb. belirler.
2. Rollout işlemi sırasında, kubernetes objelerinde yapılan değişikliklerin doğru bir şekilde kontrol edilmesi önemlidir.
3. Rollout işlemi sırasında, yeni deployment versiyonuna geçerken kaynak tüketimi ve performans konuları göz önünde bulundurulmalıdır.
4. Rollout sırasında takip edilebilir bir strateji uygulamak için, kubectl gibi araçlar kullanarak rollout sırasında pod'ların ve deployment'ın durumunu takip etmek önemlidir.
5. Rollout sırasında, hata durumunda geri alma (rollback) işlemi için hazırlıklı olmak gerekir. Bu nedenle, deployment'ın eski versiyonlarına kolayca geri dönülebilmesi için geçmiş versiyonların saklanması önemlidir.

<br><br>

# Ağ Kuralları

## Ağ Kuralları Nedir?

Kubernetes ağ kuralları, Kubernetes kümesindeki pod'ların hangi ağ kaynaklarına erişebileceğini kontrol etmek için kullanılır. Ağ kuralları, pod'lar arasındaki ağ trafiğini kontrol etmek, pod'ların dış dünya ile iletişim kurmasını sağlamak ve kümedeki kaynakların güvenliğini sağlamak için kullanılabilir.

Kubernetes ağ kuralları aşağıdaki şekillerde tanımlanabilir:

- **Pod seviyesinde:** Pod'a doğrudan erişimi kontrol etmek için pod özelliklerini kullanarak ağ kısıtlamaları tanımlanabilir.
- **Namespace seviyesinde:** Namespace seviyesinde ağ politikaları oluşturularak tüm pod'ların ağ trafiğini kontrol etmek mümkündür.

- **Küme seviyesinde:** Tüm pod'ların ağ trafiğini kontrol etmek için küme seviyesinde ağ politikaları oluşturulabilir.

Ağ kısıtlamaları, ağ politikaları aracılığıyla uygulanabilir. Ağ politikaları, belirli bir pod veya pod seti tarafından üretilen trafiği kontrol etmek için kullanılan bir Kubernetes öğesidir. Ağ politikaları, gelen ve/veya giden trafiği filtrelemek ve düzenlemek için belirli kriterlerle tanımlanabilir.

Örneğin, bir ağ politikası, belirli bir etikete sahip pod'ların sadece belirli bir başka etikete sahip pod'larla iletişim kurmasına izin verebilir. Bu, pod'lar arasındaki trafiği sınırlandırarak ağ güvenliğini artırabilir.

<br>

## Senaryolar

Konun daha iyi anlaşılması için aşağıdaki yapı üzerinden 3 farklı senaryo oluşturacağız.

<img src="https://965066212-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FIWxgpIZWYV2RbSJFQUb6%2Fuploads%2FAKmptqFNgG0KxYsAtqgz%2Fimage.png?alt=media&token=b043e314-0944-4e43-80eb-1f5067f2615d">

- **pod(sarı):** Kubernetes içindeki en küçük ve en temel uygulama öğesi olan, bir veya birden fazla konteynırdan oluşan ve aynı ağ ve depolama kaynaklarını paylaşan uygulama gruplarıdır.
- **Router:** cluster içerisindeki pod'lara gelen ağ trafiğini doğru pod'a yönlendirmek için kullanılan bir bileşendir.
- **Virtual Bridge (mavi):** Ağdaki sanal makinelerin birbirleriyle iletişim kurabilmesi ve ağ trafiğini yönetebilmek için kullanılan bir sanal ağ bileşenidir.
- **--pord-network-cidr:** Kubernetes cluster'ında Podlar için kullanılacak olan IP adreslerini belirlemek için kullanılır. Bu CIDR notasyonu, Podların IP adreslerinin atanacağı ağ adres aralığını tanımlar.
- **NAT (natlanmak):** Ağ bağlantısı yapılandırması sırasında kullanılan bir terimdir. "NAT" (Network Address Translation) olarak kısaltılan "Ağ Adresi Çevirisi", bir ağ üzerindeki bir bilgisayarın veya cihazın, diğer bir ağa erişmesini sağlar. NAT, özel IP adreslerini internete yönelik genel IP adresleriyle değiştirir ve bu sayede özel ağdaki cihazlar internete bağlanabilir. "Natlanmak" terimi, NAT kullanılarak bir ağdaki cihazların internete erişimini ifade eder.

<br>

### Senaryo - 1

<img src="https://965066212-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FIWxgpIZWYV2RbSJFQUb6%2Fuploads%2F5SKDPNS5QzImKyli9D8R%2Fimage.png?alt=media&token=a13381e9-c1a2-40c2-ae89-8c4bdfb35ea0">

**Senorya 1 için:** Burada podA'nın dış dünyada bulunan bir sunucu ile iletişimi hedefleniyor.

Bu durumda podA iletişim paketlerini virtual bridge'e teslim edecek, paket buradan worker node'un eth0 interface'ine ulaşıp natlanacak. Daha sonra paket en son olarak Router'a ulaşıp burada da natlandıktan sonra 1.1.1.1 sunucu ile iletişim sağlanacak. Bu sunucudan geri gelen pakette aynı şekilde natlanıp podA'a ulaşacak. Bu senaryoda bir problem yok.

> eth0 => Ethernet ağ teknolojisi üzerinden ağ bağlantısı sağlar ve ağa bağlı cihazlarla iletişim kurmak için kullanılır.

<br>

### Senaryo - 2

<img src="https://965066212-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FIWxgpIZWYV2RbSJFQUb6%2Fuploads%2FzJlRNeUBgIdEjB2JezRA%2Fimage.png?alt=media&token=6bec05d8-d9aa-4132-9393-a89f15a33ac7">

**Senaryo 2 için:** Burada podA aynı worker-1 içerisinde bulunan podB ile iletişime geçmesi hedefleniyor.

podA paketi virtual bridge'e teslim edecek. Virtual Bridge podA'dan gelen paketi sorunsuz bir şekilde podB'e iletecek. podB'den gelen cevapta aynı şekilde; podB cevabı virtual bridge'e iletecek, bridge gelen cevap paketini podA'a iletecek. Bu senaryoda da sıkıntı yok.

<br>

### Senaryo - 3

<img src="https://965066212-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FIWxgpIZWYV2RbSJFQUb6%2Fuploads%2FR4fMzf7lZdIExyL63vd2%2Fimage.png?alt=media&token=1d72dab5-6b26-413d-8096-191ed252de61">

Senaryo 3 için: worker-1'de bulunan PodA, worker-2'de bulunan PodC ile iletişime geçmesi hedefleniyor.

> Podlar birbiri ile NAT'a ihtiyaç duymadan iletişim kurabilmeliydi. Senaryo2'de bunu kanıtladık. Fakat burada, podA'nın Virtual Bridge'e ilettiği paket podC ile aynı worker üzerinde olmadığı için NAT'sız bir iletişim nasıl söz konusu olabiliyor?

Burada bunu çözecek birkaç düzenlemede olsa bunlar her cluster yapısı için düzgün çalışmayacaktır. Farklı clusterlar arası iletişimde kullanılabilecek Kubernetes içerisinde olan "Container Network Interface (CLI)" bu sorunu çözebilecek en uygun yapıdır.

CNI, Kubernetes'te kullanılan bir arayüzdür ve ağların podlar arasında iletişim kurmasını sağlar. Podlar arasındaki iletişim, CNI tarafından kurulan ağ arabirimleri sayesinde gerçekleştirilir.

CNI kullanarak podA ve podC arasında iletişim kurmak için şu adımlar takip edilebilir:

- **CNI eklentisi kurulur:** Öncelikle, Kubernetes cluster'ında CNI eklentisi yüklenmelidir.
- **Ağ yapılandırması oluşturulur:** Ardından, podA ve podC'nin ağ yapılandırmaları oluşturulmalıdır. Bunun için, CNI tarafından sağlanan konfigürasyon dosyası kullanılabilir.
- **Podlar oluşturulur:** Son olarak, podA ve podC oluşturulmalıdır. Bu podlar, CNI tarafından oluşturulan ağ yapılandırmaları üzerinde çalışacaklardır.

Böylece podA ve podC arasındaki iletişim, CNI tarafından sağlanan ağ arabirimleri sayesinde gerçekleştirilebilir.

<br>

### Çözüm

CNI, Kubernetes üzerinde network pluginleri kullanarak network bağlantısını yapılandırır. Podlar arasındaki iletişimi sağlamak için kullanılan CNI pluginlerinden biri olan Multus ile birden fazla ağ arabirimi tanımlanabilir ve podlar bu arabirimler aracılığıyla iletişim kurabilir.

Multus, Kubernetes CNI arabirimini kullanan bir ağ pluginidir. Multus'un konfigürasyon dosyası Multus CNI CRD (Custom Resource Definition) olarak tanımlanır ve Kubernetes API'sinde saklanır. Örnek bir Multus CNI konfigürasyon dosyası şöyle görünebilir:

```yaml
apiVersion: 'k8s.cni.cncf.io/v1'
kind: NetworkAttachmentDefinition
metadata:
  name: example-multus-network
  annotations:
    k8s.v1.cni.cncf.io/resourceName: my-net
spec:
  config: '{
    "cniVersion": "0.3.1",
    "type": "bridge",
    "bridge": "mybridge",
    "ipam": {
    "type": "host-local",
    "subnet": "192.168.0.0/16",
    "routes": [
    { "dst": "0.0.0.0/0" }
    ]
    }
    }'
```

<br><br>

# Service

## Service Nedir?

Kubernetes, containerized uygulamaları yönetmek ve dağıtmak için kullanılan bir açık kaynaklı sistemdir. Service, Kubernetes ortamında çalışan uygulamalar arasında ağ trafiği yönlendirmek için kullanılan bir objedir. Service, belirli bir IP adresi ve port numarasına sahiptir ve podlar gibi diğer Kubernetes objeleriyle birlikte çalışır.

Bir örnek senaryo ile açıklamak gerekirse, diyelim ki bir e-ticaret uygulaması çalıştırıyorsunuz ve birden fazla podda çalışan bir web uygulamasına sahipsiniz. Bu podlar, uygulamanın farklı özellikleri için hizmet verirler, örneğin kullanıcı hesap sayfaları, ürün sayfaları ve ödeme işlemleri gibi. Her podun kendine ait IP adresi vardır ve podlar arasındaki trafiği yönlendirmek için Service objesi kullanılır.

Service objesi, podların IP adreslerini dinler ve gelen istekleri doğru podlara yönlendirir. Örneğin, bir kullanıcının ürün sayfasını açmak istediğinde, tarayıcısı Service objesinin IP adresine istek gönderir ve Service objesi isteği doğru pod'a yönlendirir.

Bu senaryoda Service objesi, web uygulamasının farklı özelliklerini tek bir IP adresi altında birleştirerek, uygulama kullanıcılarına kolay bir erişim sağlar. Ayrıca, yeni bir pod eklenirse veya bir pod kaldırılırsa, Service objesi bu değişiklikleri otomatik olarak algılayacak ve trafiği doğru şekilde yönlendirecektir.

## Uygulama

Bir Kubernetes kümenizde üç adet Apache web sunucusu pod'u çalıştırıyorsunuz. Her pod farklı bir web sitesi için hizmet veriyor ve bir Service objesi kullanarak bu podların trafiğini yönlendirmek istiyorsunuz.

İlk olarak, Service objesi için bir YAML dosyası oluşturmanız gerekiyor. Aşağıdaki örnek YAML dosyasını kullanabilirsiniz:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-service
spec:
  selector:
    app: apache
  ports:
    - name: http
      protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP
```

Bu YAML dosyası, web-service adında bir Service objesi tanımlar. selector özelliği, Service objesinin yönlendireceği podları belirler. Bu örnekte, app: apache etiketi olan podlar seçilir. ports özelliği, Service objesinin dinleyeceği portu belirler. Bu örnekte, HTTP trafiği için 80 numaralı port kullanılır. targetPort özelliği, Service objesinin istekleri yönlendireceği podların hedef portunu belirler. Bu örnekte, podların da 80 numaralı portu dinlediği varsayılmıştır. type özelliği, Service objesinin türünü belirler. Bu örnekte, ClusterIP kullanılır.

YAML dosyanızı kaydettikten sonra, kubectl apply komutunu kullanarak Service objesini Kubernetes kümenize yükleyebilirsiniz:

```bash
kubectl apply -f web-service.yaml
```

Bu komut Service objesini yükler ve Kubernetes kümenizdeki tüm podlarla eşleşen app: apache etiketini arar.

Artık Service objeniz hazır. Bir web tarayıcısı açarak Kubernetes kümenizin IP adresini veya DNS adını kullanarak Service objenize istek gönderebilirsiniz. Service objesi isteği doğru pod'a yönlendirecektir.

Örneğin, http://<kubernetes-cluster-ip>/ adresine istek göndererek, Service objesi tarafından yönlendirilen bir Apache pod'unuzda çalışan bir web sitesi görebilirsiniz. Aynı şekilde, http://<kubernetes-cluster-ip>/site2 adresine istek göndererek, Service objesi tarafından yönlendirilen başka bir Apache pod'unuzda çalışan bir başka web sitesi görebilirsiniz.

Bu örnekte Service objesi, farklı podların trafiğini tek bir IP adresi altında birleştirerek, uygulama kullanıcılarına kolay bir erişim sağlıyor. Ayrıca, yeni bir pod eklediğinizde veya bir pod kaldırdığınızda, Service objesi bu değişiklikleri otomatik olarak algılayacak ve trafiği doğru şekilde yönlendirecektir.

## Service Obje Türleri

### ClusterIP

<img src="https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FIWxgpIZWYV2RbSJFQUb6%2Fuploads%2FoeJikcOSmT9QyPK8Lggy%2Fimage.png?alt=media&token=4c107f8f-0cd6-49fa-84cf-dd2edad0a3aa">

ClusterIP, Kubernetes Service objelerinin varsayılan türüdür ve yalnızca Kubernetes cluster'ı içinde erişilebilir. Bu Service türü, pod'ları tek bir IP adresi altında gruplandırır ve belirli bir port numarası üzerinden erişilebilir hale getirir.

Örneğin, bir web uygulaması çalıştırdığınızı ve web uygulamasının arkasında üç farklı pod olduğunu varsayalım. Bu pod'lar, uygulama sunucusu olarak hizmet vermektedir. ClusterIP Service objesi, bu pod'ları tek bir IP adresi altında gruplandırır ve belirli bir port numarası üzerinden erişilebilir hale getirir.

Aşağıdaki YAML belgesi, bir ClusterIP Service objesi oluşturmak için kullanılabilir:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-webapp-service
spec:
  selector:
    app: my-webapp
  ports:
    - name: http
      protocol: TCP
      port: 80
      targetPort: 8080
```

Bu Service objesi, my-webapp adlı pod'ları hedef alır ve 80 numaralı bir porttan erişilebilir hale getirir. Service objesi, hedef pod'ların 8080 numaralı portuna yönlendirir.

Bu ClusterIP Service objesi, diğer pod'lar veya servisler tarafından kolayca erişilebilir hale getirir. Örneğin, bir başka pod, bu Service objesi üzerinden web uygulamasına erişmek istediğinde, my-webapp-service adını kullanarak erişebilir. Bu, web uygulamasının, Service objesi aracılığıyla diğer pod'lar veya hizmetlerle haberleşebileceği anlamına gelir.

### NodePort

<img src="https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FIWxgpIZWYV2RbSJFQUb6%2Fuploads%2FA2Op4aHsPfm2u4vfUYUi%2Fimage.png?alt=media&token=300e21ae-167f-47f5-b637-72e14a9aac10">

NodePort, Kubernetes'teki Service objelerinden biridir ve cluster içindeki herhangi bir node üzerinde özel bir port numarası atar ve bu port numarası üzerinden Service objesine gelen tüm trafiği bu node'a yönlendirir. Bu, cluster dışındaki kaynakların, Service objesi tarafından yönlendirilen trafiğe erişmesini sağlar.

Örneğin, bir web uygulaması çalıştırdığınızı ve web uygulamasının arkasında üç farklı pod olduğunu varsayalım. Bu pod'lar, uygulama sunucusu olarak hizmet vermektedir. NodePort Service objesi, bu pod'ları tek bir IP adresi altında gruplandırır ve belirli bir port numarası üzerinden erişilebilir hale getirir. Ayrıca, bu Service objesi, her node'a özel bir port numarası atayarak, cluster dışındaki kaynakların bu node'a erişmesine izin verir.

Aşağıdaki YAML belgesi, bir NodePort Service objesi oluşturmak için kullanılabilir:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-webapp-service
spec:
  selector:
    app: my-webapp
  type: NodePort
  ports:
    - name: http
      protocol: TCP
      port: 80
      targetPort: 8080
```

Bu YAML belgesi, my-webapp-service adlı bir NodePort Service objesi oluşturur. Bu Service objesi, my-webapp adlı pod'ları hedef alır ve 80 numaralı bir porttan erişilebilir hale getirir. Service objesi, hedef pod'ların 8080 numaralı portuna yönlendirir ve her bir node üzerinde rastgele bir port numarası atar.

Bu NodePort Service objesi, her node üzerinde atanan port numarası üzerinden, cluster dışındaki kaynaklar tarafından da erişilebilir hale getirilir. Örneğin, bir kullanıcı, node_ip:node_port şeklindeki bir URL ile web uygulamasına erişebilir. Bu, web uygulamasının, NodePort Service objesi aracılığıyla cluster dışındaki kaynaklarla haberleşebileceği anlamına gelir.

> ClusterIP Service objesi, pod'lar arasındaki iletişimi kolaylaştırmak için kullanılırken, NodePort Service objesi, uygulama servislerinin cluster dışından erişilebilir hale getirilmesi için kullanılır.

### LoadBalancer

<img src="https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FIWxgpIZWYV2RbSJFQUb6%2Fuploads%2FgRd7w3HFf51qUhNXYTwf%2Fimage.png?alt=media&token=e220e40b-effd-44c3-87bb-21ae2911cb0f">

LoadBalancer, cloud ortamındaki sağlayıcının yönettiği bir yük dengeleyici oluşturur ve uygulama servislerini yük dengeleyici üzerinden dış dünyaya açar.

Örneğin, bir web uygulaması çalıştırdığınızı ve bu web uygulamasının arkasında üç farklı pod olduğunu varsayalım. Bu pod'lar, uygulama sunucusu olarak hizmet vermektedir. LoadBalancer Service objesi, bu pod'ları tek bir IP adresi altında gruplandırır ve belirli bir port numarası üzerinden erişilebilir hale getirir. Ayrıca, cloud sağlayıcısı tarafından yönetilen bir yük dengeleyici oluşturur ve uygulama servislerini yük dengeleyici üzerinden dış dünyaya açar.

Aşağıdaki YAML belgesi, bir LoadBalancer Service objesi oluşturmak için kullanılabilir:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-webapp-service
spec:
  selector:
    app: my-webapp
  type: LoadBalancer
  ports:
    - name: http
      protocol: TCP
      port: 80
      targetPort: 8080
```

Bu YAML belgesi, my-webapp-service adlı bir LoadBalancer Service objesi oluşturur. Bu Service objesi, my-webapp adlı pod'ları hedef alır ve 80 numaralı bir porttan erişilebilir hale getirir. Service objesi, hedef pod'ların 8080 numaralı portuna yönlendirir ve cloud sağlayıcısı tarafından yönetilen bir yük dengeleyici oluşturur.

Bu LoadBalancer Service objesi, cloud sağlayıcısı tarafından yönetilen yük dengeleyici üzerinden dış dünyaya açılabilir hale getirilir. Bu sayede, uygulama servisleri, dış dünyadan gelen isteklere karşılık verebilir hale gelir. Cloud sağlayıcısı, LoadBalancer Service objesi için özel bir IP adresi atar ve uygulama servisleri bu IP adresi üzerinden erişilebilir hale gelir.

### ExternalName

Uygulama servislerine DNS adları üzerinden erişmek için kullanılır. ExternalName Service objesi, cluster içinde bir DNS ismi olarak erişilebilen, farklı bir dış DNS adıyla eşleştirilir.

Örneğin, bir web uygulaması çalıştırdığınızı ve bu web uygulamasına erişmek için bir DNS adı kullanmak istediğinizi varsayalım. Ancak, web uygulaması bir başka cloud ortamındaki kaynakta çalışıyor ve erişilebilirliği sağlamak için bir DNS adı kullanıyor. ExternalName Service objesi, bu senaryoda kullanılabilir.

Aşağıdaki YAML belgesi, bir ExternalName Service objesi oluşturmak için kullanılabilir:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-webapp-service
spec:
  type: ExternalName
  externalName: my-webapp.example.com
```

Bu YAML belgesi, my-webapp-service adlı bir ExternalName Service objesi oluşturur. Bu Service objesi, cluster içinde my-webapp.example.com DNS adı olarak erişilebilir hale getirir. Bu DNS adı, dış dünyadaki bir web uygulamasına yönlendirilir ve uygulamaya erişmek için kullanılabilir.

Bu ExternalName Service objesi, cluster içinde DNS adı olarak kullanılabilir hale gelir. Bu sayede, diğer pod'lar, bu Service objesini hedefleyerek, my-webapp.example.com DNS adı üzerinden uygulamaya erişebilirler. Bu erişim, ExternalName Service objesi tarafından yönlendirilerek, dış dünyadaki uygulamaya yönlendirilir.

## Dikkat Edilmesi Gerekenler

- Pod ve Service objeleri arasındaki etiketleme tutarlılığına dikkat edin. Service objesi, etiketlerle belirlenen podları yönlendirecektir, bu nedenle Service ve pod etiketlerinin örtüşmesi gerekir.
- Service objesi türünü doğru şekilde belirleyin. ClusterIP, NodePort, LoadBalancer veya ExternalName türleri mevcuttur ve her birinin farklı kullanım durumları vardır. Bu nedenle, ihtiyacınıza en uygun olan Service türünü belirlemeniz önemlidir.
- Hedef podların sağlığını kontrol edin. Service objesi, yönlendirdiği podların sağlığını kontrol etmez. Bu nedenle, hedef podlarının sağlığını kontrol eden bir readinessProbe veya livenessProbe belirlemek önemlidir.
- Service objesi konfigürasyonunu güncellediğinizde, podların otomatik olarak yeniden başlatılmayacağına dikkat edin. Bu nedenle, Service objesi konfigürasyonunu değiştirdiğinizde, ilgili podları da manuel olarak yeniden başlatmanız gerekebilir.
- Service objelerinin trafik yönlendirmesi için kullanılan portları iyi bir şekilde belirleyin. Service objelerinin, diğer podlar veya hizmetler ile çakışmadığından emin olmak için benzersiz bir port numarası kullanın.

<br><br>

# Liveness Probe

Kubernetes, bir konteyner orkestrasyon platformudur ve birçok uygulama çalıştırmak için kullanılır. Kubernetes Liveness Probes, bir uygulamanın canlı kalıp kalmadığını denetlemek için kullanılan bir özelliktir.
Liveness Probes, bir uygulamanın çalıştığını doğrulamak için belirli bir aralıkta düzenli olarak kontrol eder. Eğer uygulama yanıt vermezse, Kubernetes uygulamayı otomatik olarak yeniden başlatır veya bir yedek uygulamayı devreye alır. Bu, uygulama çökmelerinin önüne geçerek uygulamanın sürekli çalışmasını sağlar.

## Uygulama

Aşağıdaki örnek, bir Deployment objesi için Liveness Probe özelliğini kullanır. Bu Deployment objesi, bir Nginx web sunucusunu çalıştırmak için kullanılır ve her 10 saniyede bir "/healthz" endpoint'ine HTTP GET isteği göndererek web sunucusunun sağlıklı olup olmadığını denetler. Eğer web sunucusu yanıt vermezse, Kubernetes web sunucusunu yeniden başlatır:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-web-server
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx-web-server
  template:
    metadata:
      labels:
        app: nginx-web-server
    spec:
      containers:
        - name: nginx
          image: nginx:latest
          ports:
            - containerPort: 80
          livenessProbe:
            httpGet:
              path: /healthz
              port: 80
            initialDelaySeconds: 30
            periodSeconds: 10
```

Bu örnekte, "livenessProbe" alanında belirtilen HTTP GET isteği "/healthz" endpoint'ine gönderilir. "initialDelaySeconds" alanı, Deployment objesinin başlatıldıktan sonra kaç saniye bekleyeceğini belirtir. "periodSeconds" alanı, her bir Liveness Probe kontrolü arasındaki zaman aralığını belirtir. Bu örnekte, web sunucusu her 10 saniyede bir kontrol edilir ve eğer yanıt vermezse, Kubernetes otomatik olarak yeniden başlatır.

## Dikkat Edilmesi Gerekenler

- Liveness Probe'un yanlış yapılandırılması, uygulamanın gereksiz yere yeniden başlatılmasına veya hizmet kesintilerine neden olabilir. Bu nedenle, HTTP GET isteği gönderilen endpoint ve istek sıklığı gibi ayarların doğru belirlenmesi gereklidir.
- Liveness Probe'un uygulamanın sağlığını doğru bir şekilde ölçmediği durumlarda, uygulamanın yanlış zamanda yeniden başlatılması veya hatalı olarak çalışması riski vardır. Bu nedenle, Liveness Probe'un, uygulamanın gerçek sağlık durumunu doğru bir şekilde yansıtacak şekilde yapılandırılması önemlidir.
- Liveness Probe, sadece uygulamanın sağlıklı olup olmadığını denetler ve uygulamanın performansı veya kaynak kullanımı hakkında bilgi sağlamaz. Bu nedenle, Liveness Probe, uygulamanın sağlığı hakkında bilgi sağlayan bir araç olmakla birlikte, performans ve kaynak kullanımı için farklı araçlar kullanılmalıdır.
- Liveness Probe'un uygulamanın çalışma süresi ve uygulamanın yapılandırılmasına bağlı olarak farklı ayarları gerekebilir. Bu nedenle, Liveness Probe'un ayarları, uygulamanın ihtiyaçlarına göre özelleştirilmelidir.

<br><br>

# Readiness Probe

Readiness Probe, bir Kubernetes Pod'unun çalışmaya hazır olup olmadığını belirlemek için kullanılan bir mekanizmadır. Bir Pod'un Ready olabilmesi için tüm containerların çalışması gerektiği durumlarda, tüm containerların Ready olması gerekmektedir. Ancak bazı durumlarda bir container'ın çalışmaya hazır olması yeterlidir. Bu noktada Readiness Probe devreye girer ve ilgili container'ın belirtilen koşulları karşılayıp karşılamadığını belirleyerek, Kubernetes'e bu bilgiyi bildirir.

Bir Readiness Probe, bir HTTP isteği göndererek, belirli bir portun açık olup olmadığını kontrol ederek veya bir işlemi kontrol ederek gerçekleştirilebilir. Eğer bir container'in belirtilen koşulları karşılamadığı tespit edilirse, Kubernetes bu container'a yeni istekler göndermeyi durdurabilir. Bu sayede, hatalı container'lardan kaynaklanan servis kesintileri önlenebilir.

## Uygulama

Aşağıdaki örnekte, bir Pod'un içinde çalışan bir konteynırın hazır olup olmadığını belirlemek için bir Readiness Probe tanımlanmıştır. Konteynır, /healthz yoluna GET istekleri alacak ve yanıt olarak HTTP durum kodu 200 verecek.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
    - name: mycontainer
      image: myimage
      readinessProbe:
        httpGet:
          path: /healthz
          port: 80
        initialDelaySeconds: 5
        periodSeconds: 10
```

Bu tanım, Pod'un oluşturulmasından 5 saniye sonra ve her 10 saniyede bir /healthz yoluna istek göndererek konteynırın hazır olup olmadığını kontrol edecek. Konteynır yanıt veremezse, Kubernetes tarafından yeniden başlatılacaktır.

## Readiness ve Liveness Probe Farkı

Kubernetes'te Liveness Probe ve Readiness Probe, bir konteynerin durumunu izlemek için kullanılan araçlardır. Ancak ikisi arasında birkaç farklılık vardır:

- **Liveness Probe:** Bir konteynerin çalışıp çalışmadığını belirler. Eğer konteyner liveness probe tarafından sağlanan testi geçemezse, Kubernetes tarafından yeniden başlatılır. Liveness probe genellikle bir web sunucusunun servis verip vermediğini kontrol eder.

- **Readiness Probe:** Bir konteynerin isteklere yanıt verebilecek durumda olup olmadığını belirler. Eğer bir konteyner hala başlatılırken olduğu gibi ayarlanıyorsa, ancak isteklere yanıt veremiyorsa, readiness probe tarafından sağlanan testi geçemez ve bu nedenle servis trafiğinden çıkarılır. Readiness probe genellikle bir web sunucusunun isteklere yanıt verip vermediğini kontrol eder.

Bu nedenle, Liveness Probe bir konteynerin "canlı" olup olmadığını belirlerken, Readiness Probe bir konteynerin "hazır" olup olmadığını belirler.

## Dikkat Edilmesi Gerekenler

- Probenin başarısız olması, pod'un doğrudan silinmesine neden olabilir ve bağlı olduğu hizmetlerin kullanılamaz hale gelmesine sebep olabilir. Bu nedenle, özellikle kritik hizmetlerde, uygun bir bekleme süresi ve yeniden deneme sayısı ayarlamak önemlidir.
- Probenin sıklığı da önemlidir. Sık sık yapılan istekler, gereksiz yere kaynak tüketebilir ve sistemi yavaşlatabilir. Aksine, nadiren yapılan istekler hataların tespit edilmesini geciktirebilir.
- Probenin yapıldığı yol, pod'un durumunu etkileyebilir. Örneğin, bir dosya yoksa veya bir bağlantı sağlanamazsa, pod başarısız olarak işaretlenebilir ve hizmetlerde kesintiye neden olabilir. Bu nedenle, doğru yol belirtilmelidir.

<br><br>

# Resource Limits

Kubernetes'de, bir konteynerin kullanabileceği maksimum CPU ve bellek miktarını belirlemek için "resource limits" kullanılır. Bu, Kubernetes kubernetes-cluster üzerinde çalışan tüm konteynerlerin adil bir şekilde kaynakları kullanmalarını sağlamaya yardımcı olur. "Resource limits" aynı zamanda konteynerlerin, kullanılabilecek kaynakları aşan yüksek kullanım durumlarında uygulamanın çökmesini veya yavaşlamasını önlemeye yardımcı olur.

"Resource limits" belirli bir konteyner için iki ayrı değer içerir:

- **CPU limiti:** Bir konteynerin maksimum kullanabileceği CPU sayısıdır.
- **Bellek limiti:** Bir konteynerin maksimum kullanabileceği bellek miktarıdır.
  Kubernetes, "resource limits" özelliği sayesinde belirli bir miktar CPU veya bellek kullanımı aşıldığında otomatik olarak uygulamayı yeniden başlatarak kaynak tükenmesi durumlarını engellemeye yardımcı olur.

## Uygulama

Aşağıdaki örnek, bir Pod'daki konteyner için CPU ve bellek limitlerini belirlemektedir:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: resource-limits
spec:
  containers:
    - name: my-container
      image: my-image
      resources:
        limits:
          cpu: '1'
          memory: '2Gi'
        requests:
          cpu: '500m'
          memory: '1Gi'
```

Bu Pod'da yer alan my-container adlı konteyner için, CPU limiti olarak 1 core ("1") ve bellek limiti olarak 2 gigabayt ("2Gi") tanımlanmaktadır. Ayrıca, CPU isteği olarak yarım core ("500m") ve bellek isteği olarak 1 gigabayt ("1Gi") belirtilmiştir.

> 1 cpu = cpu="1" = cpu: "1000" = cpu="1000m"

## Dikkat Edilmesi Gerekenler

- Limits değerleri belirlenirken, podların ihtiyaç duyduğu kaynakları doğru şekilde belirlemek ve bu değerlerin yeterli olacağından emin olmak önemlidir. Aksi halde podlar yeterli kaynaklara sahip olmadıklarından dolayı hata verebilir ya da düşük performans sergileyebilirler.
- Limits değerleri, cluster'ın toplam kaynaklarını aşmamalıdır. Aksi halde diğer podlar ve node'lar üzerinde sorunlara yol açabilir.
- Limits değerleri belirlenirken, podların kullanım senaryosu da dikkate alınmalıdır. Örneğin, bir pod'un CPU kullanımı sabit olabilirken, aynı pod'un RAM kullanımı dinamik olabilir. Bu nedenle, Limits değerleri belirlenirken podların özelliklerine göre farklı sınırlar belirlemek gerekebilir.
- Limits değerleri, doğru ölçü birimleri ile belirlenmelidir. Örneğin, CPU kullanımı millicore cinsinden, RAM kullanımı ise gigabyte cinsinden ifade edilir.
- Limits değerleri belirlenirken, podların aynı zamanda Liveness ve Readiness Probe'larına da yeterli kaynak ayrılmalıdır. Aksi halde bu Probe'lar doğru şekilde çalışmayabilir veya yanıltıcı sonuçlar verebilirler.

<br><br>

# Environment Variables

Kubernetes ortam değişkenleri, bir Kubernetes pod veya container'ına ilişkin değişkenlerdir. Pod veya container'ın çalıştığı zaman çevresel değişkenler olarak kullanılabilirler.

Bu değişkenler, genellikle önceden tanımlanmış ve sabit değerlerden oluşur ve pod veya container'ın hangi durumda çalıştırılacağına bağlı olarak belirlenir. Kubernetes, podlar ve konteynerler için önceden tanımlanmış bazı ortam değişkenlerini sağlar, ancak kullanıcıların da kendi değişkenlerini tanımlayabileceği esnek bir yapı sunar.

Örneğin, bir uygulamanın veritabanı bağlantı dizesi genellikle bir ortam değişkeni olarak kullanılır. Böylece uygulamanın farklı veritabanlarına bağlanması gerektiğinde sadece ortam değişkeni değiştirilir ve uygulama yeniden başlatılır.

Ortam değişkenleri, Kubernetes pod ve container konfigürasyon dosyalarında belirtilir. Bunlar genellikle YAML formatında tanımlanır. Örneğin:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-container
      image: my-image
      env:
        - name: MY_ENV_VAR
          value: my-env-value
```

Yukarıdaki örnekte, MY_ENV_VAR adlı bir ortam değişkeni tanımlanmıştır ve my-env-value değeri atanmıştır. Bu pod başlatıldığında, my-container içindeki herhangi bir işlemin MY_ENV_VAR değişkenini kullanabilmesi için hazır olacaktır.

<br><br>

# Volume

## Kubernetes Volume nedir?

Kubernetes Volume, Kubernetes podları içinde kullanılan veri depolama birimleridir. Bir Kubernetes podu, bir veya daha fazla konteyneri içerebilir ve bu konteynerler arasında veri paylaşımı gerektiğinde, bir pod seviyesinde bir depolama birimi kullanılması gerekmektedir.

Kubernetes Volume, bir pod içindeki konteynerler arasında veri paylaşımını sağlamak için kullanılan bir Kubernetes kaynağıdır. Bu depolama birimi, podun yaşam döngüsü boyunca kullanılabilir ve verilerin silinmesi, depolanması ve geri yüklenmesi işlemlerini otomatik olarak yönetir.

Kubernetes Volume'lerinin birçok türü vardır, bunlar arasında Azure Disk, Amazon EBS, NFS, iSCSI, ConfigMap, Secret ve EmptyDir gibi seçenekler bulunur. Podlarda kullanılan Volume'ler, podlar oluşturulurken veya güncellenirken tanımlanabilir ve kullanılan depolama kaynağına göre yapılandırılabilir.

Kubernetes Volume'lerinin avantajları arasında, veri paylaşımını kolaylaştırması, veri kaybını önlemesi ve podların bağımsızlığını artırması yer alır. Dezavantajları ise, disk alanı tüketebilmesi, veri depolama maliyetlerini artırması ve hata ayıklama işlemlerini zorlaştırması olabilir.

## Volume Tipleri

- EmptyDir
- HostPath
- GitRepo
- ConfigMap
- Secret

## EmptyDir

emptyDir, Kubernetes'te kullanılan bir volume tipidir ve geçici verilerin depolanması için kullanılır. Bu volume türü, bir pod çalıştırıldığında oluşturulur ve bu pod silindiğinde de otomatik olarak temizlenir.

Bir emptyDir volume'ü oluşturduktan sonra, pod'ların içinde çalışan tüm container'lar bu volume'e erişebilirler. Bu, bir pod'daki container'ların birbirleriyle geçici bir şekilde veri paylaşmasına izin verir. emptyDir volume'ü, özellikle bir pod'da birden fazla container kullanıldığında, container'lar arasında geçici bir veri paylaşımı gerektiğinde veya pod'un bir sonraki aşamasında kullanmak üzere geçici bir depolama alanına ihtiyaç duyulduğunda kullanışlı olabilir.

Ancak, emptyDir volume'leri pod silindiğinde tamamen silinir, bu nedenle verilerin kalıcı olması gerekiyorsa başka bir volume türü kullanmak gerekmektedir.

Aşağıdaki YAML konfigürasyon örneğinde, emptyDir adında bir Volume oluşturuluyor ve web adındaki Pod'a bağlanıyor:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: web
spec:
  containers:
    - name: nginx
      image: nginx
      volumeMounts:
        - name: cache-volume
          mountPath: /cache
  volumes:
    - name: cache-volume
      emptyDir: {}
```

Yukarıdaki örnekte, emptyDir Volume'unun ismi cache-volume olarak belirtiliyor. Pod'un içindeki nginx konteynerinde /cache dizinine bağlanıyor. Bu sayede Pod içindeki konteynerler arasında geçici veri saklamak için kullanılabilir.

## HostPath

hostPath, Kubernetes cluster'ındaki bir node'un dosya sistemi yolunu bir volume olarak kullanmamızı sağlayan bir volume tipidir. Böylece pod'lar doğrudan host makinanın dosya sistemi ile iletişim kurabilir.

Bu volume tipi, genellikle test, geliştirme veya single-node cluster'ların oluşturulmasında kullanılır. Ancak, multi-node cluster'larında hostPath kullanmak, yüksek kullanılabilirlik ve ölçeklenebilirlik açısından önerilmez.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
    - name: mycontainer
      image: myimage
      volumeMounts:
        - name: myvolume
          mountPath: /mnt/mydata
  volumes:
    - name: myvolume
      hostPath:
        path: /var/data
        type: Directory
```

Yukarıdaki örnekte, mypod adlı pod, /var/data dizinini mycontainer adlı container'da /mnt/mydata mount point'inde kullanır. Bu, host makinanın /var/data dizinindeki tüm dosyaların, mypod pod'unda /mnt/mydata altında kullanılabileceği anlamına gelir.

## GitRepo

GitRepo, bir Git deposundan alınan bir kaynak kodunu Kubernetes pod'una bir volume olarak eklemek için kullanılan bir tipdir. GitRepo volume tipi, bir depo adı, bir branch veya tag adı, bir volume adı ve kullanılacak Git depo kimlik bilgileri gibi birkaç parametre gerektirir. Bu volume tipi, özellikle bir uygulamanın bir Git deposundan alınan kaynak kodunu kullanarak çalıştırılması gerektiğinde yararlıdır.

Örnek olarak, aşağıdaki YAML, bir Git deposundan bir kaynak kodu alarak bir volume oluşturur ve bu volume'u bir Pod'a bağlar:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-container
      image: my-image
      volumeMounts:
        - name: git-volume
          mountPath: /my-app
  volumes:
    - name: git-volume
      gitRepo:
        repository: https://github.com/my-user/my-repo
        revision: master
        directory: my-app
```

Yukarıdaki örnekte, gitRepo volume tipi kullanılarak my-user/my-repo deposundan master dalındaki my-app dizinindeki kaynak kodu alınır. Bu kaynak kodu, my-pod adlı Pod'da /my-app dizinine bağlanan git-volume adlı bir volume'a eklenir. Bu sayede, my-container adlı konteyner, /my-app dizinindeki kaynak kodunu kullanabilir.

## ConfigMap

Kubernetes'te configMap bir yapılandırma ayarlarını depolamak için kullanılan bir objedir. Bir configMap nesnesi, podların kullanabileceği birden çok yapılandırma ayarını depolayabilir. Her bir ayar, bir anahtar-değer çifti olarak depolanır. configMap ile depolanan ayarlar, env, arg veya bir dosya olarak kullanılabilir.

Örneğin, bir uygulamanız var ve farklı ortamlarda (geliştirme, test ve prod) kullanmak üzere ayarlarınız var. Bu ayarları her ortam için farklı bir şekilde saklamanız gerekiyor. configMap'leri kullanarak, bu ayarları her ortam için farklı bir configMap objesi olarak depolayabilirsiniz.

Örneğin, aşağıdaki gibi bir configMap objesi tanımlayabilirsiniz:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  DB_HOST: db.example.com
  DB_PORT: '5432'
  API_URL: http://api.example.com
```

Bu örnekte, app-config adlı bir configMap objesi oluşturulur. DB_HOST, DB_PORT ve API_URL gibi anahtar-değer çiftleri, farklı ortamlarda kullanılmak üzere ayarlar olarak tanımlanır.

Daha sonra, bu configMap objesi podlarda kullanılabilir. Örneğin, aşağıdaki gibi bir pod tanımı kullanarak configMap objesindeki ayarları env olarak kullanabilirsiniz:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-app
      image: my-image
      env:
        - name: DB_HOST
          valueFrom:
            configMapKeyRef:
              name: app-config
              key: DB_HOST
        - name: DB_PORT
          valueFrom:
            configMapKeyRef:
              name: app-config
              key: DB_PORT
        - name: API_URL
          valueFrom:
            configMapKeyRef:
              name: app-config
              key: API_URL
```

Bu örnekte, my-pod adlı bir pod tanımlanır ve app-config adlı configMap objesi kullanılarak pod içindeki konteynerlerin env değişkenleri ayarlanır. configMapKeyRef özelliği, configMap objesindeki ayarların nasıl alınacağını belirtir.

## Secret

Kubernetes'de Secret Volume, verileri şifrelenmiş bir şekilde depolamak için kullanılan bir depolama türüdür. Bu tür bir volume ile veriler, Kubernetes Secret nesnelerinde tutulur ve pod'larda bu secret volume'una bağlanarak kullanılabilir.

Örneğin, bir uygulama veritabanı şifreleri gibi hassas bilgiler içeriyorsa, bu bilgilerin açık şekilde kaydedilmesi uygun olmaz. Bu tür hassas bilgileri, Kubernetes Secret nesnelerinde saklayabiliriz. Daha sonra bu Secret nesnelerini pod'lara Volume olarak bağlayarak, bu hassas bilgilere pod'larda erişebiliriz.

Örneğin bir deployment'ta bir container çalıştırdığımızı ve bu container'ın bir secret dosyasına ihtiyacı olduğunu varsayalım. Öncelikle bir secret oluşturmalıyız:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: mysecret
type: Opaque
data:
  username: dXNlcg==
  password: cGFzc3dvcmQ=
```

> Kubernetes Secret objesi içerisinde data ve stringData alanlarına sahip olabilir. Her iki alan da Secret objesi içerisinde değer saklamak için kullanılır ancak aralarında bazı farklılıklar vardır.

> data alanı, Base64 kodlu verileri saklamak için kullanılır. Bu nedenle, verileri kaydetmek için genellikle bir Base64 kodlayıcı kullanmak gerekir. Veriler, anahtar-değer çiftleri şeklinde saklanır. Bu alan, gizli anahtarları ve sertifikaları gibi hassas verileri saklamak için kullanılır.

> stringData alanı ise Base64 kodlaması yapmadan doğrudan açık metin olarak verileri saklamak için kullanılır. Bu nedenle, verileri saklamak için herhangi bir kodlayıcı kullanmak gerekmez. Veriler yine anahtar-değer çiftleri şeklinde saklanır. Bu alan, genellikle basit metin verileri gibi hassas olmayan verileri saklamak için kullanılır.

> Özetle, data alanı hassas verileri saklamak için kullanılırken, stringData alanı hassas olmayan verileri saklamak için kullanılır.

Bu secret dosyasını oluşturduktan sonra, pod'da kullanmak için aşağıdaki şekilde kullanabiliriz:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
    - name: mycontainer
      image: myimage
      volumeMounts:
        - name: secret-volume
          mountPath: /etc/mysecret
          readOnly: true
  volumes:
    - name: secret-volume
      secret:
        secretName: mysecret
```

Bu YAML dosyası, mypod adlı bir pod yaratır ve mycontainer adlı bir container içerir. Bu container, /etc/mysecret yoluna read-only bir volume bağlar. Volume'un adı secret-volume olarak belirtilir ve bu volume, mysecret adlı secret dosyasını kullanarak oluşturulur.

Artık bu pod çalıştırıldığında, mycontainer adlı container /etc/mysecret yolunda mysecret dosyasına erişebilir ve içerdiği username ve password bilgilerini kullanabilir. Bu şekilde, hassas bilgileri container'ımızın içine gömmek yerine secret dosyaları kullanarak daha güvenli bir yöntemle saklayabiliriz.

### ConfigMap vs Secret Volume

Kubernetes'te ConfigMap ve Secret, uygulama yapılandırması ve gizli bilgilerin depolanması için kullanılan iki kaynak türüdür. ConfigMap, anahtar-değer çiftleri aracılığıyla yapılandırma bilgilerini depolamak için kullanılırken, Secret, duyarlı bilgileri (örneğin, şifreler, API anahtarları) depolamak için kullanılır.

ConfigMap'lar, bir uygulamanın çeşitli yapılandırma ayarlarını depolamak için kullanışlıdır. Örneğin, bir web uygulamasında kullanılan farklı yapılandırma dosyalarının tümünü tek bir ConfigMap içinde depolayabilirsiniz. Bu, yapılandırma değişikliklerini daha kolay yönetmenizi sağlar. ConfigMap'lar, uygulama yapılandırmasını değiştirmek için manuel olarak veya bir konfigürasyon yönetimi aracı (örneğin, Helm) kullanarak güncellenebilir.

Secret'lar, gizli verilerin depolanması için kullanılır. Örneğin, bir veritabanı şifresi veya bir API anahtarı gibi duyarlı bilgileri Secret içinde depolayabilirsiniz. Secret'lar, ConfigMap'lar gibi anahtar-değer çiftleri aracılığıyla depolanır ve kullanım sırasında otomatik olarak deşifre edilir. Secret'lar, genellikle bir uygulamanın Pod'larında bulunan konteynerlerin gizli bilgilere erişmesi gerektiğinde kullanılır.

Özetle, ConfigMap'lar uygulama yapılandırması için, Secret'lar ise duyarlı bilgilerin depolanması için kullanılır.

## Dikkat Edilmesi Gerekenler

- Verilerinizin kaybolmasını önlemek için doğru depolama sınıfını seçin. Örneğin, Ephemeral Volume'ler önbellek için kullanılabilirken, Persistant Volume'ler daha kalıcı veri saklama için tercih edilir.
- Volume'lerin hangi pod'lar tarafından kullanılacağını belirleyin. Pod'larda birden fazla konteyner olduğunda, tüm konteynerlerin aynı volume'ü kullanması gerekiyorsa bunu belirtmelisiniz.
- Farklı sınıflardaki Volume'leri farklı pod'lar kullanıyorsa, birden fazla pod üzerindeki değişiklikleri takip etmek için kaynak kısıtlamalarını tanımlayın.
- Volume'lerin veri bütünlüğünü korumak için doğru şekilde yapılandırıldığından emin olun. Örneğin, veri bütünlüğü sağlamak için hazırda bekletilmiş ve ayrıcalıklı bir konumda depolanması gereken verilerin kullanımı için HostPath Volume kullanmak uygun değildir
- Yapılandırmanızda Volume'lerin temizlenmesi veya silinmesi için gerekli prosedürleri belirleyin. Örneğin, bir Pod veya Deployment silindiğinde Volume'ün otomatik olarak silinmesini sağlayabilirsiniz.

<br><br>

# Node Affinity

Kubernetes'de, node affinity, bir pod'un hangi node üzerinde çalışacağını belirlemek için kullanılan bir mekanizmadır. Node affinity, podların belirli bir node'a planlanmasını sağlamak için kullanılan bir yöntemdir.

Node affinity, bir pod'un belirli bir nod üzerinde çalışmasını sağlamak için iki tür kısıtlama sunar:

- **RequiredDuringSchedulingIgnoredDuringExecution:** Bu kısıtlama, bir pod'un belirli bir node üzerinde çalışması gerektiğini belirtir. Eğer belirtilen node bulunamazsa, pod çalıştırılmaz. Bu kısıtlama, pod'un belirli bir veri merkezi bölgesinde veya belirli bir donanım özelliğine sahip node üzerinde çalışmasını sağlamak için kullanılabilir.
- **PreferredDuringSchedulingIgnoredDuringExecution:** Bu kısıtlama, bir pod'un belirli bir node üzerinde çalışması tercih edilir ama mutlaka o node üzerinde çalışması gerekmez. Bu kısıtlama, belirli bir node üzerindeki kaynak kullanımını dengeli bir şekilde dağıtmak için kullanılabilir.

Node affinity, Kubernetes'de ölçeklenebilir uygulamaların dağıtımını ve yönetimini kolaylaştırır. Bu sayede, uygulamanızın performansını artırabilir ve daha verimli bir şekilde çalışmasını sağlayabilirsiniz.

## Uygulama

Aşağıdaki örnekte, bir pod'un belirli bir node üzerinde çalışması için node affinity kullanılır:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-container
      image: my-image
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
          - matchExpressions:
              - key: node-type
                operator: In
                values:
                  - worker
```

Bu örnekte, "my-pod" adında bir pod oluşturuluyor. Pod, "my-image" adlı bir konteyneri çalıştırıyor. Pod, "node-type" adlı bir etiketle belirtilen "worker" tipindeki bir nod üzerinde çalışması için ayarlanmıştır. "requiredDuringSchedulingIgnoredDuringExecution" kısıtlaması kullanılmıştır. Bu, pod'un belirtilen bir node üzerinde çalışması gerektiğini belirtir. Eğer belirtilen node bulunamazsa, pod çalıştırılmaz.

"nodeSelectorTerms" ve "matchExpressions" kullanılarak bir node seçimi yapılmıştır. "nodeSelectorTerms" bölümü, "matchExpressions" listesinde belirtilen koşullara göre node'ları seçer. "key", "operator" ve "values" özellikleri, koşulların nasıl belirleneceğini tanımlar.

## Dikkat Edilmesi Gerekenler

- Node affinity'yi kullanmadan önce, Kubernetes cluster'ınızdaki tüm nod'ların doğru şekilde etiketlendiğinden emin olun. Doğru etiketler kullanılmazsa, pod'lar doğru şekilde planlanamayabilir ve hatalar oluşabilir.
- Pod'un birden fazla node'a planlanması gerektiği durumlarda, "PreferredDuringSchedulingIgnoredDuringExecution" kısıtlaması kullanılabilir. Bu kısıtlama, pod'un belirli bir node üzerinde çalışması tercih edilir ama mutlaka o node üzerinde çalışması gerekmez.
- Node affinity'yi kullanarak pod'ların belirli bir node üzerinde çalışmasını sağlamak, node üzerindeki yükü dengeli bir şekilde dağıtmak için bir yöntemdir. Ancak, bazen node'ların arızalanması veya kullanılamaz hale gelmesi durumunda pod'ların yeniden planlanması gerekebilir. Bu durumlarda, pod'ların farklı bir node üzerinde çalışacak şekilde ayarlanması gerekir.

<br><br>

# Pod Affinity

Kubernetes'de Pod Affinity, bir pod'un diğer podlarla ilişkisini belirlemek için kullanılan bir mekanizmadır. Pod Affinity, bir pod'un belirli bir node üzerinde çalışması gerektiği durumlarda kullanılır.

Pod Affinity'nin kullanımı, pod'ların planlanması sırasında diğer podlarla olan ilişkilerine bağlıdır. Örneğin, bir pod'un belirli bir başka pod'un yakınında çalışması gerektiği durumlarda Pod Affinity kullanılabilir.

Pod Affinity, "requiredDuringSchedulingIgnoredDuringExecution" ve "preferredDuringSchedulingIgnoredDuringExecution" olmak üzere iki kısıtlama türüne sahiptir. "requiredDuringSchedulingIgnoredDuringExecution" kısıtlaması, bir pod'un belirli bir pod veya pod grubuyla aynı node üzerinde çalışmasını gerektirir. "preferredDuringSchedulingIgnoredDuringExecution" kısıtlaması ise pod'un belirli bir pod veya pod grubuyla aynı node üzerinde çalışmasını tercih eder, ancak mutlaka aynı node üzerinde çalışması gerekmez.

Pod Affinity, Kubernetes cluster'ında pod'ların doğru şekilde planlanmasına yardımcı olur. Pod'ların doğru şekilde planlanması, uygulamanın performansını artırabilir ve Kubernetes cluster'ının daha verimli çalışmasını sağlayabilir.

Aşağıda, Pod Affinity kullanarak pod'ların belirli bir pod veya pod grubuyla aynı node üzerinde çalışması sağlanan bir örnek gösterilmektedir:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-container
      image: my-image
  affinity:
    podAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        - labelSelector:
            matchExpressions:
              - key: app
                operator: In
                values:
                  - my-app
          topologyKey: 'kubernetes.io/hostname'
```

Bu örnekte, "my-pod" adında bir pod oluşturuluyor. Pod, "my-image" adlı bir konteyneri çalıştırıyor. Pod, "app" adlı bir etiketle belirtilen "my-app" pod grubuyla aynı node üzerinde çalışması için ayarlanmıştır.

Bu örnekte, "requiredDuringSchedulingIgnoredDuringExecution" kısıtlaması kullanılmıştır. Bu, pod'un belirtilen bir pod veya pod grubuyla aynı node üzerinde çalışması gerektiğini belirtir. Eğer belirtilen pod veya pod grubuyla aynı node bulunamazsa, pod çalıştırılmaz.

## Dikkat Edilmesi Gerekenler

- **Döngüsel etkileşimlerin önlenmesi:** Pod Affinity, pod'ların birbirleriyle etkileşimde bulunmasını sağlayabilir. Ancak, belirli bir pod grubunda döngüsel etkileşimler oluşabilir. Bu nedenle, Pod Affinity kullanırken döngüsel etkileşimleri önlemek önemlidir.
- **CPU ve hafıza kullanımı:** Pod Affinity, pod'ların belirli bir node üzerinde çalışmasını sağlayabilir. Ancak, bu node'ların CPU ve hafıza kullanımı gibi özelliklerinin pod'ların gereksinimlerini karşılayıp karşılamadığı kontrol edilmelidir. Aksi takdirde, node üzerindeki pod'lar performans sorunlarına neden olabilir.
- **Verimlilik:** Pod Affinity, pod'ların doğru şekilde planlanmasına yardımcı olabilir. Ancak, Pod Affinity kullanımının verimliliği artırmak için doğru şekilde yapılandırılması önemlidir. Yetersiz yapılandırılmış Pod Affinity kısıtlamaları, uygulamanın performansını olumsuz etkileyebilir.
- **Pod sayısı:** Pod Affinity, belirli bir pod grubuyla aynı node üzerinde çalışan pod sayısını kontrol edebilir. Ancak, node üzerindeki pod sayısı kontrol edilmelidir. Aksi takdirde, node üzerindeki pod'ların performans sorunlarına neden olabilir.
- **Topoloji anahtarları:** Pod Affinity, pod'ların belirli bir topoloji anahtarı kullanarak node'lar arasında planlanmasını sağlar. Ancak, doğru topoloji anahtarlarının seçilmesi önemlidir. Yanlış topoloji anahtarları kullanıldığında, pod'ların yanlış node üzerinde çalışmasına neden olabilir.
