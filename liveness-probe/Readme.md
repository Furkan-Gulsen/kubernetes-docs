# Liveness Probe

Kubernetes, bir konteyner orkestrasyon platformudur ve birçok uygulama çalıştırmak için kullanılır. Kubernetes Liveness Probes, bir uygulamanın canlı kalıp kalmadığını denetlemek için kullanılan bir özelliktir.
Liveness Probes, bir uygulamanın çalıştığını doğrulamak için belirli bir aralıkta düzenli olarak kontrol eder. Eğer uygulama yanıt vermezse, Kubernetes uygulamayı otomatik olarak yeniden başlatır veya bir yedek uygulamayı devreye alır. Bu, uygulama çökmelerinin önüne geçerek uygulamanın sürekli çalışmasını sağlar.

## Uygulama

Aşağıdaki örnek, bir Deployment objesi için Liveness Probe özelliğini kullanır. Bu Deployment objesi, bir Nginx web sunucusunu çalıştırmak için kullanılır ve her 10 saniyede bir "/healthz" endpoint'ine HTTP GET isteği göndererek web sunucusunun sağlıklı olup olmadığını denetler. Eğer web sunucusu yanıt vermezse, Kubernetes web sunucusunu yeniden başlatır:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-web-server
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx-web-server
  template:
    metadata:
      labels:
        app: nginx-web-server
    spec:
      containers:
        - name: nginx
          image: nginx:latest
          ports:
            - containerPort: 80
          livenessProbe:
            httpGet:
              path: /healthz
              port: 80
            initialDelaySeconds: 30
            periodSeconds: 10
```

Bu örnekte, "livenessProbe" alanında belirtilen HTTP GET isteği "/healthz" endpoint'ine gönderilir. "initialDelaySeconds" alanı, Deployment objesinin başlatıldıktan sonra kaç saniye bekleyeceğini belirtir. "periodSeconds" alanı, her bir Liveness Probe kontrolü arasındaki zaman aralığını belirtir. Bu örnekte, web sunucusu her 10 saniyede bir kontrol edilir ve eğer yanıt vermezse, Kubernetes otomatik olarak yeniden başlatır.

## Dikkat Edilmesi Gerekenler

- Liveness Probe'un yanlış yapılandırılması, uygulamanın gereksiz yere yeniden başlatılmasına veya hizmet kesintilerine neden olabilir. Bu nedenle, HTTP GET isteği gönderilen endpoint ve istek sıklığı gibi ayarların doğru belirlenmesi gereklidir.
- Liveness Probe'un uygulamanın sağlığını doğru bir şekilde ölçmediği durumlarda, uygulamanın yanlış zamanda yeniden başlatılması veya hatalı olarak çalışması riski vardır. Bu nedenle, Liveness Probe'un, uygulamanın gerçek sağlık durumunu doğru bir şekilde yansıtacak şekilde yapılandırılması önemlidir.
- Liveness Probe, sadece uygulamanın sağlıklı olup olmadığını denetler ve uygulamanın performansı veya kaynak kullanımı hakkında bilgi sağlamaz. Bu nedenle, Liveness Probe, uygulamanın sağlığı hakkında bilgi sağlayan bir araç olmakla birlikte, performans ve kaynak kullanımı için farklı araçlar kullanılmalıdır.
- Liveness Probe'un uygulamanın çalışma süresi ve uygulamanın yapılandırılmasına bağlı olarak farklı ayarları gerekebilir. Bu nedenle, Liveness Probe'un ayarları, uygulamanın ihtiyaçlarına göre özelleştirilmelidir.
