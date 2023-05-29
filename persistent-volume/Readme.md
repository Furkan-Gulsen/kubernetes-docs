# Persistent Volume

Kubernetes'te, verilerin kalıcı bir şekilde depolanması için Persistent Volume (PV) ve Persistent Volume Claim (PVC) adı verilen iki önemli kavram kullanılır.

Persistent Volume (PV):

- PV, Kubernetes kümesinde uygulamaların kullanabileceği kalıcı bir depolama alanıdır.
- PV'ler, altta yatan fiziksel veya sanal depolama kaynaklarına bağlanarak sağlanır.
- PV'ler, depolama sınıfları (storage class) aracılığıyla belirli özelliklere (hız, boyut, dayanıklılık vb.) sahip olabilir.
- PV'ler, bağımsız olarak oluşturulur ve yönetilir. İhtiyaç duyulduğunda bir PVC ile eşleştirilerek kullanılır.

Persistent Volume Claim (PVC):

- PVC, uygulamaların bir PV talep etmesi ve kullanması için oluşturulan bir istektir.
- PVC'ler, uygulamaların depolama ihtiyaçlarını belirtmek için kullanılır.
- PVC'ler, PV'leri talep eder ve uygun olan bir PV ile eşleşirse, PVC'ye bağlanır ve kullanılabilir hale gelir.
- PVC'ler, PV'lerden bağımsız olarak oluşturulur ve yönetilir.

Bu iki kavramın bir araya gelmesiyle, uygulamaların depolama talepleri dinamik olarak karşılanabilir ve uygulamalar PV'ler üzerinden kalıcı verilere erişebilir.

Aşağıda, bir örnek YAML dosyası üzerinden PV ve PVC nasıl tanımlanır, oluşturulur ve kullanılır gösterilmektedir:

PV Tanım Dosyası (pv.yaml):

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /data/my-pv
```

PVC Tanım Dosyası (pvc.yaml):

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 2Gi
```

Bu örnekte, "my-pv" adında 5GB boyutunda bir PV tanımlanır ve "/data/my-pv" dizinine bağlanır. Ardından, "my-pvc" adında 2GB boyutunda bir PVC tanımlanır.

PVC'nin kullanılması için bir Pod'a bağlanması gerekmektedir. Bu Pod tanımı aşağıdaki gibi olabilir:

Pod Tanım Dosyası (pod.yaml):

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-app
      image: my-app-image:latest
      volumeMounts:
        - name: my-volume
          mountPath: /data
  volumes:
    - name: my-volume
      persistentVolumeClaim:
        claimName: my-pvc
```

Bu Pod tanımında, "my-pvc" adlı PVC'ye bağlanarak "/data" dizinine monte edilen bir konteyner tanımlanmıştır.

## Access Modlar

Persistent Volume (PV)'de üç farklı accessModes çeşidi bulunmaktadır:

1. ReadWriteOnce (RWO):

   - Bu accessModes, bir PV'nin aynı anda yalnızca bir Pod tarafından okunup yazılmasına izin verir.
   - Yani, bir PV, aynı anda yalnızca bir Pod tarafından monte edilebilir ve üzerinde okuma ve yazma işlemleri gerçekleştirilebilir.
   - RWO, genellikle tek bir düğümde veya fiziksel disk üzerinde bağımsız depolama sağlamak için kullanılır.

2. ReadOnlyMany (ROX):

   - Bu accessModes, bir PV'nin birden fazla Pod tarafından eşzamanlı olarak okunmasına, ancak yazılmasına izin vermez.
   - Yani, birden fazla Pod aynı PV'yi sadece okuma amaçlı olarak kullanabilir, yazma işlemi gerçekleştirilemez.
   - ROX, genellikle birden çok Pod'un aynı anda veri okuması gereken senaryolarda kullanılır.

3. ReadWriteMany (RWX):
   - Bu accessModes, bir PV'nin birden fazla Pod tarafından eşzamanlı olarak hem okunmasına hem de yazılmasına izin verir.
   - Yani, birden fazla Pod aynı PV'yi okuma ve yazma işlemleri için kullanabilir.
   - RWX, genellikle birden çok Pod'un aynı anda veri okuması ve yazması gereken senaryolarda kullanılır.

Bu accessModes çeşitleri, farklı uygulama ve kullanım senaryolarına göre depolama kaynaklarının paylaşılabilirliğini ve kullanımını kontrol etmek için kullanılır. PV'lerin accessModes özelliği, bir PVC'nin uygun bir PV'ye bağlanabilmesi için uyumlu accessModes'a sahip olması gerektiği anlamına gelir.

## PersistentVolumeReclaimPolicy

Persistent Volume (PV)'lerde üç farklı PersistentVolumeReclaimPolicy çeşidi bulunmaktadır:

1. Retain:

   - Bu politika, PV'nin kullanımdan kaldırılması veya serbest bırakılması durumunda verilerin korunmasını sağlar.
   - Bir PV, kullanımdan kaldırıldığında veya bağlantısı kesildiğinde bile verileri korunur ve elle temizlenene kadar kullanılabilir durumda kalır.
   - Bu politika, verilerin kalıcı olarak korunması gereken durumlarda tercih edilir. Kullanıcılar, verileri elle temizleyerek PV'yi yeniden kullanabilirler.

2. Delete:

   - Bu politika, PV kullanımdan kaldırıldığında veya serbest bırakıldığında otomatik olarak PV'yi temizler.
   - PV'ye bağlı veriler de otomatik olarak silinir ve geri dönüşü olmayan bir şekilde kaldırılır.
   - Bu politika, verilerin kullanımdan kaldırıldığında otomatik olarak temizlenmesi gereken durumlarda tercih edilir.

3. Recycle (Artık önerilmemektedir):
   - Bu politika, PV kullanımdan kaldırıldığında veya serbest bırakıldığında verileri silmeden önce temizlemek için bir geri dönüşüm süreci başlatır.
   - Geri dönüşüm süreci, PV içindeki verileri silerek veya sıfırlayarak gerçekleştirilebilir.
   - Bu politika artık önerilmemektedir ve genellikle kullanımdan kalkmış bir depolama sistemi için geri dönüşüm yapmak isteyen eski uygulamalar için kullanılır.

PersistentVolumeReclaimPolicy, bir PV'nin kullanımdan kaldırılması veya serbest bırakılması durumunda ne yapılacağını belirler. Bu politikalar, veri yönetimi ve güvenlik gereksinimlerine göre seçilmelidir.

## Uygulama

### Adım 1: PV Tanımı

PV için bir YAML dosyası oluşturun. Aşağıdaki örnekte, hostPath kullanılarak basit bir PV tanımlanmaktadır:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /data/my-pv
```

Bu örnekte, "my-pv" adında 5GB boyutunda bir PV tanımlanmıştır. "/data/my-pv" dizinine bağlanan bir hostPath kullanılmaktadır.

### Adım 2: PVC Tanımı

PVC için bir YAML dosyası oluşturun. Aşağıdaki örnekte, önceden tanımlanan PV'ye uygun bir PVC tanımlanmaktadır:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 2Gi
```

Bu örnekte, "my-pvc" adında 2GB boyutunda bir PVC tanımlanmıştır.

### Adım 3: PVC'yi Kullanacak Pod Tanımı

PVC'yi kullanacak bir Pod tanımlayın. Aşağıdaki örnekte, PVC ile birlikte bir Pod tanımlanmaktadır:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-app
      image: my-app-image:latest
      volumeMounts:
        - name: my-volume
          mountPath: /data
  volumes:
    - name: my-volume
      persistentVolumeClaim:
        claimName: my-pvc
```

Bu örnekte, "my-pvc" adlı PVC'ye bağlı olarak "/data" dizinine monte edilen bir konteyner tanımlanmaktadır.

### Adım 4: Kaynakları Oluşturma

Önce PV'yi oluşturun:

```
kubectl apply -f pv.yaml
```

Ardından, PVC'yi oluşturun:

```
kubectl apply -f pvc.yaml
```

Son olarak, PVC'yi kullanan Pod'u oluşturun:

```
kubectl apply -f pod.yaml
```

Bu adımları takip ederek PV, PVC ve Pod'u oluşturabilirsiniz. PV, PVC'ye bağlanacak ve PVC, Pod'un içerisindeki mountPath'e monte edilecektir. Böylece Pod, kalıcı verilere PVC aracılığıyla erişebilir.
