# StatefulSet

StatefulSet, Kubernetes kümelerinde durumsal (stateful) uygulamaların yönetimi için kullanılan bir kaynak türüdür. Durumsal uygulamalar, her bir örneğin benzersiz bir kimliği veya durumu olan uygulamalardır.

StatefulSet, ölçeklenebilir ve sıralı dağıtım gerektiren uygulamaların yönetimi için tasarlanmıştır. Örneğin, veritabanı sunucuları veya mesaj sıraları gibi uygulamalar, durumsal uygulama örnekleri olarak kabul edilebilir.

**StatefulSet, aşağıdaki avantajları sağlar:**

- **Sabit Kimlik:** Her bir durumsal uygulama örneği (Pod), benzersiz bir kimlik veya isimle tanımlanır. Bu, uygulamanın verileri ve durumu için tutarlılık sağlar.
- **Sıralı Dağıtım:** StatefulSet, uygulama örneklerini belirli bir sıra veya düzen içinde dağıtmak için kullanılabilir. Bu, uygulamanın başlatma, yeniden başlatma veya ölçeklendirme gibi işlemlerinde sıralı ve öngörülebilir davranış sağlar.
- **Stable Network Identity:** Her bir durumsal uygulama örneği, istikrarlı bir ağ kimliğine sahiptir. Bu, diğer uygulamaların ve servislerin uygulamaya güvenli bir şekilde bağlanabilmesini sağlar.
- **Stable Storage:** StatefulSet, her bir durumsal uygulama örneği için kalıcı depolama alanının korunmasını sağlar. Bu, verilerin güvenli bir şekilde saklanmasını ve uygulama örneklerinin verilere erişimini sürdürebilmesini sağlar.

**StatefulSet, birkaç ana bileşenden oluşur:**

- **Pod:** Durumsal uygulama örneklerinin çalıştığı temel birimdir.
- **Headless Service:** Durumsal uygulama örneklerine istikrarlı bir ağ kimliği sağlar.
- **Volume Claim Templates:** Her bir durumsal uygulama örneği için kalıcı depolama taleplerini tanımlar.
- **StatefulSet Controller:** Durumsal uygulama örneklerini yönetir ve istenen duruma ulaşmak için gerektiğinde yeni örnekler oluşturur veya var olanları yeniden başlatır.

**StatefulSet kullanırken dikkate almanız gereken bazı önemli noktalar vardır:**

- **Podların Benzersiz Kimlikleri:** Her bir durumsal uygulama örneği benzersiz bir kimliğe sahiptir. Bu, uygulama örneklerinin farklı düğümlere veya dağıtım zamanlarında yeniden başlatıldığında aynı veriler ve durumla devam edebilmesini sağlar.
- **Stable Network Identity ve Servisler:** StatefulSet, uygulama örneklerine istikrarlı bir ağ kimliği sağlar. Diğer Kubernetes servisleri veya uygulamaları, bu istikrarlı kimlikleri kullanarak durumsal uygulama örneklerine güvenli bir şekilde erişebilir.
- **Kalıcı Depolama:** Durumsal uygulama örnekleri genellikle kalıcı verileri saklar. Bu nedenle, StatefulSet kullanırken kalıcı depolama taleplerini doğru şekilde yapılandırmalı ve uygun depolama sağlayıcısını seçmelisiniz.
- **Yeniden Başlatma ve Ölçeklendirme:** Durumsal uygulama örneklerini yeniden başlatırken veya ölçeklendirirken, sıralı ve koordineli bir şekilde yapmanız önemlidir. Bu, uygulamanın tutarlılığını ve verilerin bütünlüğünü korumaya yardımcı olur.

## Deployment vs StatefulSet

**Uygulama Yönetimi:**

- **Deployment**: Deployment, tipik olarak durumsuz (stateless) uygulamaların yönetimi için kullanılır. Bu tür uygulamalar genellikle paylaşılan verilere veya sabit duruma ihtiyaç duymazlar.
- **StatefulSet:** StatefulSet, durumsal (stateful) uygulamaların yönetimi için kullanılır. Bu tür uygulamalar, her bir örneğin benzersiz bir kimliğe, istikrarlı bir ağ kimliğine ve kalıcı depolamaya ihtiyaç duyarlar.

**Örnekleme ve Kimliklendirme:**

- **Deployment:** Deployment, örnekleme için otomatik olarak benzersiz bir kimlik atar ve her örnek için farklı bir IP adresi alabilir. Örneğin, yük dengeleyici önünde çalışan bir örnekleme grubu oluşturulabilir.
- **StatefulSet:** StatefulSet, her bir örneğe benzersiz bir kimlik atar ve istikrarlı bir ağ kimliği sağlar. Örneğin, veritabanı sunucuları gibi durumsal uygulama örnekleri, her biri kendi kimliğiyle erişilebilir ve özel ağ bağlantılarına sahip olabilir.

**Depolama ve Veri Saklama:**

- **Deployment:** Deployment'lar genellikle geçici (ephemeral) depolama kullanır. Herhangi bir örnek yeniden oluşturulduğunda veya ölçeklendirildiğinde veriler kaybedilebilir.
- **StatefulSet:** StatefulSet'ler, kalıcı depolama talepleriyle kullanılır. Her bir örnek, benzersiz bir veritabanı veya dosya sistemi gibi kalıcı verileri korumak için ayrı bir depolama alanına sahip olabilir.

**Ölçeklendirme ve Güncelleme:**

- **Deployment:** Deployment'lar, yatay ölçeklendirme (scale-out) için uygundur. Uygulama örnekleri, yük dengeleyici ile dağıtılarak isteğe bağlı olarak artırılabilir veya azaltılabilir. Güncelleme sırasında eski örnekler kaldırılır ve yeni örnekler eklenir.
- **StatefulSet:** StatefulSet'ler, dikey ölçeklendirme (scale-up) için uygundur. Her bir örneğin ayrı bir kimliği olduğundan, ölçeklendirme işlemleri genellikle el ile yapılır. Güncellemeler sırasında, eski örneklerin sırayla kaldırılması ve yeni örneklerin eklenmesi gerekebilir.

## Uygulama

### Adım 1: Headless Service Oluşturma

Öncelikle, StatefulSet için bir Headless Service (istikrarlı hizmet) oluşturmanız gerekmektedir. Bu hizmet, her bir durumsal uygulama örneğine istikrarlı bir ağ kimliği sağlayacaktır:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-app-service
  labels:
    app: my-app
spec:
  clusterIP: None
  selector:
    app: my-app
```

Bu YAML dosyası, "my-app-service" adında bir Headless Service oluşturur. "my-app" etiketine sahip durumsal uygulama örneklerine istikrarlı bir ağ kimliği sağlar.

### Adım 2: StatefulSet Oluşturma

Sonraki adımda, StatefulSet'i oluşturmanız gerekmektedir:

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: my-app-statefulset
spec:
  selector:
    matchLabels:
      app: my-app
  serviceName: my-app-service
  replicas: 3
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: my-app-container
          image: my-app-image
          ports:
            - containerPort: 8080
  volumeClaimTemplates:
    - metadata:
        name: data
      spec:
        accessModes: ['ReadWriteOnce']
        resources:
          requests:
            storage: 1Gi
```

Bu YAML dosyası, "my-app-statefulset" adında bir StatefulSet oluşturur. "my-app" etiketine sahip durumsal uygulama örneklerini seçer. "my-app-service" adlı Headless Service'i kullanır. Replica sayısını 3 olarak belirtir. Her bir durumsal uygulama örneği için "my-app-container" adında bir konteyner oluşturur. Ayrıca, her bir uygulama örneği için "data" adında bir volume talebi tanımlar.

### Adım 3: StatefulSet'i Kubernetes Kümelerine Uygulama

StatefulSet YAML dosyasını kullanarak StatefulSet'i Kubernetes kümelerine uygulamak için aşağıdaki komutu kullanabilirsiniz:

```shell
kubectl apply -f statefulset.yaml
```

Bu komut, statefulset.yaml dosyasını kullanarak StatefulSet'i Kubernetes kümelerine uygular.

### Adım 4: StatefulSet Durumunu Kontrol Etme

StatefulSet'in başarıyla oluşturulduğunu ve çalıştığını kontrol etmek için aşağıdaki komutu kullanabilirsiniz:

```yaml
kubectl get statefulset
```

Bu komut, mevcut StatefulSet'leri listeler ve durumunu gösterir. "my-app-statefulset" adında bir StatefulSet listelenmeli ve durumu "Running" olarak görünmelidir.

### Adım 5: Durumsal Uygulama Örneklerini Kontrol Etme

Durumsal uygulama örneklerinin başarıyla oluşturulduğunu ve çalıştığını kontrol etmek için aşağıdaki komutu kullanabilirsiniz:

```yaml
kubectl get pods
```

Bu komut, mevcut Pod'ları listeler ve durumlarını gösterir. "my-app-statefulset-" ile başlayan adlara sahip 3 adet Pod listelenmeli ve durumları "Running" olarak görünmelidir.
