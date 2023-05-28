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
