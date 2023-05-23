# Kubernetes Dokümanı

<img src="https://miro.medium.com/v2/resize:fit:1400/1*5-LJw8RG96qMMxPZeKcV5w.png">

Bu GitHub Reposunda amacım Kubernetes'e dair öğrendiğim bilgileri (makalelerden, kurslardan, notlarımdan) paylaşarak bu alana daha hızlı ve bilgilini girmeni sağlamaktır. Zamanında araştırırken zorlandığım ve vakit alan kısımlarında içerisine alan bu repo sayesinde umarım, DevOps kariyerine Kubernetes teknolojisini hızla katabileceksin 🚀

🙏 Bu repoyu paylaştığım için bana teşekkür etmek istersen repoya yıldız vermen yeterli olacaktır. İyi öğrenmeler.

⚡️ Ayıca bu reponun bir websitesi var. Oraya giderek burada yer alan sayfaları çok daha kolay inceleyebilirsin: [Kubernetes Dokümanı](https://docs.furkangulsen.com/kubernetes)

## Bölümler:

- [ReplicaSet](#replicaset)
- [Rollout ve Rollback](#rollout-ve-rollback)
- [Ag Kuralları](#ağ-kuralları)

---

<br>

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
      "routes": [ { "dst": "0.0.0.0/0" }]
    }
  }'
```
