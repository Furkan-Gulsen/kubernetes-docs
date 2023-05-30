# Storage Class

Storage Class, Kubernetes kümelerinde veri depolama kaynaklarını yönetmek için kullanılan bir mekanizmadır. Bu mekanizma, farklı depolama gereksinimlerine sahip uygulamaları desteklemek için esneklik sağlar.

Storage Class, depolama sağlayıcılarına (örneğin, bulut tabanlı depolama servisleri veya yerel depolama sistemleri gibi) özgü depolama seçeneklerini Kubernetes üzerinde tanımlamak için kullanılır. Depolama kaynaklarının türünü, erişim modunu, boyutunu ve diğer özelliklerini belirlemek için kullanılan bir YAML dosyasıyla yapılandırılır.

Bu yapılandırma, Kubernetes kullanıcılarına depolama taleplerini kolayca tanımlama ve kullanma imkanı sağlar. Bir kullanıcı bir PVC (Persistent Volume Claim) talebinde bulunduğunda, bu talep, ilgili Storage Class tarafından belirtilen depolama kaynağına eşlenir ve bir PVC oluşturulur. PVC, uygulamanın kullanabileceği kalıcı depolama alanını temsil eder.

Storage Class'lar ayrıca depolama kaynaklarının dinamik olarak oluşturulmasını da destekler. Bir PVC talep edildiğinde, Storage Class, mevcut depolama kaynaklarını kontrol eder ve uygun olan bir depolama alanını otomatik olarak oluşturur.

Bu şekilde, Storage Class kullanarak, farklı depolama gereksinimlerine sahip uygulamalar için esnek depolama çözümleri oluşturabilirsiniz. Ayrıca, depolama kaynaklarının dinamik olarak yönetilmesini sağlayarak, kullanıcıların depolama taleplerini daha kolay bir şekilde karşılayabilirsiniz.

## Uygulama

### Adım 1: Storage Class Oluşturma

İlk adımda, bir Storage Class oluşturmanız gerekmektedir. Örnek olarak aşağıdaki YAML dosyasını kullanabilirsiniz:

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: fast-storage
provisioner: example.com/fast-storage
parameters:
  type: ssd
```

Bu YAML dosyası, "fast-storage" adında bir Storage Class oluşturur. "example.com/fast-storage" adlı bir provizyoner kullanılarak depolama alanı sağlanır ve "type: ssd" parametresi, SSD tabanlı bir depolama çözümünü belirtir.

### Adım 2: Storage Class'ı Kubernetes Kümelerine Uygulama

Oluşturduğunuz storageclass.yaml dosyasını kullanarak Storage Class'ı Kubernetes kümelerine uygulayabilirsiniz. Bunun için aşağıdaki komutu kullanabilirsiniz:

```shell
kubectl apply -f storageclass.yaml
```

Bu komut, storageclass.yaml dosyasını kullanarak Storage Class'ı Kubernetes kümelerine uygular.

### Adım 3: PVC (Persistent Volume Claim) Talebi Oluşturma

Storage Class'ı kullanarak bir PVC talebi oluşturabilirsiniz. Örnek olarak aşağıdaki PVC YAML dosyasını kullanabilirsiniz:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  storageClassName: fast-storage
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
```

Bu YAML dosyası, "my-pvc" adında bir PVC talebi oluşturur. "fast-storage" adlı Storage Class'ı kullanarak depolama alanı talep eder. Ayrıca, "ReadWriteOnce" erişim modunu ve 5GB boyutunda bir depolama talebini belirtir.

### Adım 4: PVC'yi Kubernetes Kümelerine Uygulama

PVC talebini oluşturduktan sonra, aşağıdaki komutu kullanarak PVC'yi Kubernetes kümelerine uygulayabilirsiniz:

```shell
kubectl apply -f pvc.yaml
```

### Adım 5: PVC Durumunu Kontrol Etme

PVC'nin başarıyla oluşturulup oluşturulmadığını kontrol etmek için aşağıdaki komutu kullanabilirsiniz:

```shell
kubectl get pvc
```

Bu komut, mevcut PVC'leri listeler ve durumunu gösterir. "my-pvc" adında bir PVC listelenmeli ve durumu "Bound" olarak görünmelidir, bu da PVC'nin başarıyla bir depolama alanına bağlandığını gösterir.
