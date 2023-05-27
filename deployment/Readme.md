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
