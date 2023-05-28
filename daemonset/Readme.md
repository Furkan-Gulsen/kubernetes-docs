# DaemonSet

DaemonSet, Kubernetes'in bir kaynak türüdür ve uygulamaları veya sistem bileşenlerini bir Kubernetes kümesindeki her bir düğümde çalıştırmak için kullanılır.

DaemonSet, her bir düğümde bir Pod oluşturur ve bu Pod'lar belirli bir etiketi karşılar. Her düğümde yalnızca bir Pod çalışır ve yeni bir düğüm eklediğinizde otomatik olarak yeni bir Pod oluşturulur. Aynı şekilde, bir düğüm silindiğinde, ilgili Pod da otomatik olarak kaldırılır.

DaemonSet'ler, sistem düzeyindeki görevler, log toplama araçları, ağ izleme araçları gibi uygulamaları çalıştırmak için idealdir. Örneğin, bir ağ izleme aracını DaemonSet olarak çalıştırarak her düğümde ağ trafiğini izleyebilir ve merkezi bir şekilde analiz edebilirsiniz.

DaemonSet'lerin bazı özellikleri şunlardır:

1. **Güncelleme Kontrolü:** DaemonSet'ler, Pod'ları güncellemek için Rolling Update stratejisini kullanır. Bu, yeni bir DaemonSet oluştururken mevcut Pod'ları yavaş yavaş güncelleyerek uygulamanızın kesintisiz çalışmasını sağlar.

2. **Node Tolerasyonu:** DaemonSet'ler, düğümler üzerinde çalışan Pod'ları hedefler ve belirli düğümlerde DaemonSet çalıştırmamak için tolerans (toleration) özelliklerini kullanabilir.

3. Güvenlik: DaemonSet'lerin güvenliği, Pod'larda kullanılan konteyner imajlarına ve Pod'ların düğümler üzerinde çalıştığı erişim haklarına dikkat etmek suretiyle sağlanmalıdır.

DaemonSet'ler, uygulamalarınızı her düğümde çalıştırmak ve sistem düzeyindeki görevleri yerine getirmek için güçlü bir araçtır. Bu sayede, Kubernetes kümenizdeki tüm düğümlerde tutarlı bir ortam sağlayabilir ve ölçeklenebilirlik ve işlevsellik sunabilirsiniz.

## Uygulama

### Adım 1: YAML Oluşturma

İlk adımda, DaemonSet'i tanımlayan bir Kaynak Tanım Dosyası (daemonset.yaml) oluşturmanız gerekmektedir. Örnek olarak aşağıdaki YAML dosyasını kullanabilirsiniz:

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: my-app-daemonset
spec:
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

Bu YAML dosyası, "my-app-daemonset" adında bir DaemonSet oluşturur. DaemonSet, tüm düğümlerde "my-app" etiketini karşılayan bir Pod oluşturur. Pod'lar için "my-app-container" adında bir konteyner oluşturulur ve bu konteyner, "my-app-image:latest" adlı bir imajı kullanarak 8080 portunda çalışır.

### Adım 2: DaemonSet'i Kubernetes Kümelerine Uygulama

Oluşturduğunuz daemonset.yaml dosyasını kullanarak DaemonSet'i Kubernetes kümelerine uygulayabilirsiniz. Bunun için aşağıdaki komutu kullanabilirsiniz:

```shell
kubectl apply -f daemonset.yaml
```

Bu komut, daemonset.yaml dosyasını kullanarak DaemonSet'i Kubernetes kümelerine uygular.

### Adım 3: DaemonSet Durumunu Kontrol Etme

DaemonSet'in başarıyla uygulanıp uygulanmadığını kontrol etmek için aşağıdaki komutu kullanabilirsiniz:

```shell
kubectl get daemonsets
```

Bu komut, mevcut DaemonSet'leri listeler ve bunların durumunu gösterir. "my-app-daemonset" adında bir DaemonSet listelenmeli ve tüm düğümlerde çalışan Pod'larını göstermelidir.

### Adım 4: Pod'ların Durumunu Kontrol Etme

DaemonSet ile oluşturulan Pod'ların durumunu kontrol etmek için aşağıdaki komutu kullanabilirsiniz:

```shell
kubectl get pods
```

Bu komut, mevcut Pod'ları listeler ve bunların durumunu gösterir. "my-app-daemonset-" ile başlayan adlara sahip Pod'lar listelenmeli ve durumları "Running" olmalıdır.
