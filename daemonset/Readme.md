# DaemonSet

Kubernetes DaemonSet, Kubernetes cluster'ında tüm node'lar üzerinde çalışan pod'ların oluşturulmasını ve yönetilmesini sağlayan bir obje tipidir. DaemonSet, özellikle tüm node'lar üzerinde çalışması gereken bir uygulamanın Kubernetes cluster'ında dağıtılması için kullanılır.

DaemonSet, belirli bir node'a bir pod oluşturmak yerine, tüm node'lar üzerinde pod'ların çalıştırılmasını sağlar. Bu, özellikle bir pod'un her node üzerinde çalışması gerektiği durumlarda kullanışlıdır. Örneğin, log toplama, izleme veya ağ politikası uygulama gibi sistem düzeyi görevlerde DaemonSet kullanılabilir.

DaemonSet, pod'ların tüm node'lar üzerinde çalışmasını sağladığından, yeni bir node eklediğimizde DaemonSet otomatik olarak yeni bir pod oluşturur. Bunun yanı sıra, DaemonSet, pod'ların tüm node'lar üzerinde çalışmasını sağlamak için yüksek kullanılabilirlik (high availability) sağlar.

DaemonSet, Kubernetes cluster'ında DaemonSet objesi oluşturmak için YAML dosyaları kullanılarak oluşturulur. Örneğin, aşağıdaki YAML dosyası ile bir DaemonSet objesi oluşturulabilir:

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: my-daemonset
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
        - name: my-container
          image: my-image
```

Bu YAML dosyasında, "my-daemonset" adında bir DaemonSet oluşturuluyor. DaemonSet, "my-image" adlı bir konteyneri çalıştırıyor. Pod'lar, "my-app" adlı bir etiket ile işaretlenir. Bu sayede, DaemonSet, pod'ların tüm node'lar üzerinde çalışmasını sağlar.

**Özetle**, DaemonSet, Kubernetes cluster'ında tüm node'lar üzerinde çalışan pod'ların oluşturulması ve yönetilmesini sağlar. DaemonSet, tüm node'lar üzerinde çalışması gereken bir uygulamanın dağıtımı için idealdir. DaemonSet, Kubernetes cluster'ında DaemonSet objesi oluşturmak için YAML dosyaları kullanılarak oluşturulabilir.
