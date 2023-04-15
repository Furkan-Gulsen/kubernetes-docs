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
