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
