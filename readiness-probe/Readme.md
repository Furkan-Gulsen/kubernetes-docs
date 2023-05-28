# Readiness Probe

Readiness Probe, bir Kubernetes Pod'unun çalışmaya hazır olup olmadığını belirlemek için kullanılan bir mekanizmadır. Bir Pod'un Ready olabilmesi için tüm containerların çalışması gerektiği durumlarda, tüm containerların Ready olması gerekmektedir. Ancak bazı durumlarda bir container'ın çalışmaya hazır olması yeterlidir. Bu noktada Readiness Probe devreye girer ve ilgili container'ın belirtilen koşulları karşılayıp karşılamadığını belirleyerek, Kubernetes'e bu bilgiyi bildirir.

Bir Readiness Probe, bir HTTP isteği göndererek, belirli bir portun açık olup olmadığını kontrol ederek veya bir işlemi kontrol ederek gerçekleştirilebilir. Eğer bir container'in belirtilen koşulları karşılamadığı tespit edilirse, Kubernetes bu container'a yeni istekler göndermeyi durdurabilir. Bu sayede, hatalı container'lardan kaynaklanan servis kesintileri önlenebilir.

## Uygulama

Aşağıdaki örnekte, bir Pod'un içinde çalışan bir konteynırın hazır olup olmadığını belirlemek için bir Readiness Probe tanımlanmıştır. Konteynır, /healthz yoluna GET istekleri alacak ve yanıt olarak HTTP durum kodu 200 verecek.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
    - name: mycontainer
      image: myimage
      readinessProbe:
        httpGet:
          path: /healthz
          port: 80
        initialDelaySeconds: 5
        periodSeconds: 10
```

Bu tanım, Pod'un oluşturulmasından 5 saniye sonra ve her 10 saniyede bir /healthz yoluna istek göndererek konteynırın hazır olup olmadığını kontrol edecek. Konteynır yanıt veremezse, Kubernetes tarafından yeniden başlatılacaktır.

## Readiness ve Liveness Probe Farkı

Kubernetes'te Liveness Probe ve Readiness Probe, bir konteynerin durumunu izlemek için kullanılan araçlardır. Ancak ikisi arasında birkaç farklılık vardır:

- **Liveness Probe:** Bir konteynerin çalışıp çalışmadığını belirler. Eğer konteyner liveness probe tarafından sağlanan testi geçemezse, Kubernetes tarafından yeniden başlatılır. Liveness probe genellikle bir web sunucusunun servis verip vermediğini kontrol eder.

- **Readiness Probe:** Bir konteynerin isteklere yanıt verebilecek durumda olup olmadığını belirler. Eğer bir konteyner hala başlatılırken olduğu gibi ayarlanıyorsa, ancak isteklere yanıt veremiyorsa, readiness probe tarafından sağlanan testi geçemez ve bu nedenle servis trafiğinden çıkarılır. Readiness probe genellikle bir web sunucusunun isteklere yanıt verip vermediğini kontrol eder.

Bu nedenle, Liveness Probe bir konteynerin "canlı" olup olmadığını belirlerken, Readiness Probe bir konteynerin "hazır" olup olmadığını belirler.

## Dikkat Edilmesi Gerekenler

- Probenin başarısız olması, pod'un doğrudan silinmesine neden olabilir ve bağlı olduğu hizmetlerin kullanılamaz hale gelmesine sebep olabilir. Bu nedenle, özellikle kritik hizmetlerde, uygun bir bekleme süresi ve yeniden deneme sayısı ayarlamak önemlidir.
- Probenin sıklığı da önemlidir. Sık sık yapılan istekler, gereksiz yere kaynak tüketebilir ve sistemi yavaşlatabilir. Aksine, nadiren yapılan istekler hataların tespit edilmesini geciktirebilir.
- Probenin yapıldığı yol, pod'un durumunu etkileyebilir. Örneğin, bir dosya yoksa veya bir bağlantı sağlanamazsa, pod başarısız olarak işaretlenebilir ve hizmetlerde kesintiye neden olabilir. Bu nedenle, doğru yol belirtilmelidir.
