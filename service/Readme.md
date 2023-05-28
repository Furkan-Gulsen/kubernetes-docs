# Service

## Service Nedir?

Kubernetes, containerized uygulamaları yönetmek ve dağıtmak için kullanılan bir açık kaynaklı sistemdir. Service, Kubernetes ortamında çalışan uygulamalar arasında ağ trafiği yönlendirmek için kullanılan bir objedir. Service, belirli bir IP adresi ve port numarasına sahiptir ve podlar gibi diğer Kubernetes objeleriyle birlikte çalışır.

Bir örnek senaryo ile açıklamak gerekirse, diyelim ki bir e-ticaret uygulaması çalıştırıyorsunuz ve birden fazla podda çalışan bir web uygulamasına sahipsiniz. Bu podlar, uygulamanın farklı özellikleri için hizmet verirler, örneğin kullanıcı hesap sayfaları, ürün sayfaları ve ödeme işlemleri gibi. Her podun kendine ait IP adresi vardır ve podlar arasındaki trafiği yönlendirmek için Service objesi kullanılır.

Service objesi, podların IP adreslerini dinler ve gelen istekleri doğru podlara yönlendirir. Örneğin, bir kullanıcının ürün sayfasını açmak istediğinde, tarayıcısı Service objesinin IP adresine istek gönderir ve Service objesi isteği doğru pod'a yönlendirir.

Bu senaryoda Service objesi, web uygulamasının farklı özelliklerini tek bir IP adresi altında birleştirerek, uygulama kullanıcılarına kolay bir erişim sağlar. Ayrıca, yeni bir pod eklenirse veya bir pod kaldırılırsa, Service objesi bu değişiklikleri otomatik olarak algılayacak ve trafiği doğru şekilde yönlendirecektir.

## Uygulama

Bir Kubernetes kümenizde üç adet Apache web sunucusu pod'u çalıştırıyorsunuz. Her pod farklı bir web sitesi için hizmet veriyor ve bir Service objesi kullanarak bu podların trafiğini yönlendirmek istiyorsunuz.

İlk olarak, Service objesi için bir YAML dosyası oluşturmanız gerekiyor. Aşağıdaki örnek YAML dosyasını kullanabilirsiniz:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-service
spec:
  selector:
    app: apache
  ports:
    - name: http
      protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP
```

Bu YAML dosyası, web-service adında bir Service objesi tanımlar. selector özelliği, Service objesinin yönlendireceği podları belirler. Bu örnekte, app: apache etiketi olan podlar seçilir. ports özelliği, Service objesinin dinleyeceği portu belirler. Bu örnekte, HTTP trafiği için 80 numaralı port kullanılır. targetPort özelliği, Service objesinin istekleri yönlendireceği podların hedef portunu belirler. Bu örnekte, podların da 80 numaralı portu dinlediği varsayılmıştır. type özelliği, Service objesinin türünü belirler. Bu örnekte, ClusterIP kullanılır.

YAML dosyanızı kaydettikten sonra, kubectl apply komutunu kullanarak Service objesini Kubernetes kümenize yükleyebilirsiniz:

```bash
kubectl apply -f web-service.yaml
```

Bu komut Service objesini yükler ve Kubernetes kümenizdeki tüm podlarla eşleşen app: apache etiketini arar.

Artık Service objeniz hazır. Bir web tarayıcısı açarak Kubernetes kümenizin IP adresini veya DNS adını kullanarak Service objenize istek gönderebilirsiniz. Service objesi isteği doğru pod'a yönlendirecektir.

Örneğin, http://<kubernetes-cluster-ip>/ adresine istek göndererek, Service objesi tarafından yönlendirilen bir Apache pod'unuzda çalışan bir web sitesi görebilirsiniz. Aynı şekilde, http://<kubernetes-cluster-ip>/site2 adresine istek göndererek, Service objesi tarafından yönlendirilen başka bir Apache pod'unuzda çalışan bir başka web sitesi görebilirsiniz.

Bu örnekte Service objesi, farklı podların trafiğini tek bir IP adresi altında birleştirerek, uygulama kullanıcılarına kolay bir erişim sağlıyor. Ayrıca, yeni bir pod eklediğinizde veya bir pod kaldırdığınızda, Service objesi bu değişiklikleri otomatik olarak algılayacak ve trafiği doğru şekilde yönlendirecektir.

## Service Obje Türleri

### ClusterIP

<img src="https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FIWxgpIZWYV2RbSJFQUb6%2Fuploads%2FoeJikcOSmT9QyPK8Lggy%2Fimage.png?alt=media&token=4c107f8f-0cd6-49fa-84cf-dd2edad0a3aa">

ClusterIP, Kubernetes Service objelerinin varsayılan türüdür ve yalnızca Kubernetes cluster'ı içinde erişilebilir. Bu Service türü, pod'ları tek bir IP adresi altında gruplandırır ve belirli bir port numarası üzerinden erişilebilir hale getirir.

Örneğin, bir web uygulaması çalıştırdığınızı ve web uygulamasının arkasında üç farklı pod olduğunu varsayalım. Bu pod'lar, uygulama sunucusu olarak hizmet vermektedir. ClusterIP Service objesi, bu pod'ları tek bir IP adresi altında gruplandırır ve belirli bir port numarası üzerinden erişilebilir hale getirir.

Aşağıdaki YAML belgesi, bir ClusterIP Service objesi oluşturmak için kullanılabilir:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-webapp-service
spec:
  selector:
    app: my-webapp
  ports:
    - name: http
      protocol: TCP
      port: 80
      targetPort: 8080
```

Bu Service objesi, my-webapp adlı pod'ları hedef alır ve 80 numaralı bir porttan erişilebilir hale getirir. Service objesi, hedef pod'ların 8080 numaralı portuna yönlendirir.

Bu ClusterIP Service objesi, diğer pod'lar veya servisler tarafından kolayca erişilebilir hale getirir. Örneğin, bir başka pod, bu Service objesi üzerinden web uygulamasına erişmek istediğinde, my-webapp-service adını kullanarak erişebilir. Bu, web uygulamasının, Service objesi aracılığıyla diğer pod'lar veya hizmetlerle haberleşebileceği anlamına gelir.

### NodePort

<img src="https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FIWxgpIZWYV2RbSJFQUb6%2Fuploads%2FA2Op4aHsPfm2u4vfUYUi%2Fimage.png?alt=media&token=300e21ae-167f-47f5-b637-72e14a9aac10">

NodePort, Kubernetes'teki Service objelerinden biridir ve cluster içindeki herhangi bir node üzerinde özel bir port numarası atar ve bu port numarası üzerinden Service objesine gelen tüm trafiği bu node'a yönlendirir. Bu, cluster dışındaki kaynakların, Service objesi tarafından yönlendirilen trafiğe erişmesini sağlar.

Örneğin, bir web uygulaması çalıştırdığınızı ve web uygulamasının arkasında üç farklı pod olduğunu varsayalım. Bu pod'lar, uygulama sunucusu olarak hizmet vermektedir. NodePort Service objesi, bu pod'ları tek bir IP adresi altında gruplandırır ve belirli bir port numarası üzerinden erişilebilir hale getirir. Ayrıca, bu Service objesi, her node'a özel bir port numarası atayarak, cluster dışındaki kaynakların bu node'a erişmesine izin verir.

Aşağıdaki YAML belgesi, bir NodePort Service objesi oluşturmak için kullanılabilir:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-webapp-service
spec:
  selector:
    app: my-webapp
  type: NodePort
  ports:
    - name: http
      protocol: TCP
      port: 80
      targetPort: 8080
```

Bu YAML belgesi, my-webapp-service adlı bir NodePort Service objesi oluşturur. Bu Service objesi, my-webapp adlı pod'ları hedef alır ve 80 numaralı bir porttan erişilebilir hale getirir. Service objesi, hedef pod'ların 8080 numaralı portuna yönlendirir ve her bir node üzerinde rastgele bir port numarası atar.

Bu NodePort Service objesi, her node üzerinde atanan port numarası üzerinden, cluster dışındaki kaynaklar tarafından da erişilebilir hale getirilir. Örneğin, bir kullanıcı, node_ip:node_port şeklindeki bir URL ile web uygulamasına erişebilir. Bu, web uygulamasının, NodePort Service objesi aracılığıyla cluster dışındaki kaynaklarla haberleşebileceği anlamına gelir.

> ClusterIP Service objesi, pod'lar arasındaki iletişimi kolaylaştırmak için kullanılırken, NodePort Service objesi, uygulama servislerinin cluster dışından erişilebilir hale getirilmesi için kullanılır.

### LoadBalancer

<img src="https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FIWxgpIZWYV2RbSJFQUb6%2Fuploads%2FgRd7w3HFf51qUhNXYTwf%2Fimage.png?alt=media&token=e220e40b-effd-44c3-87bb-21ae2911cb0f">

LoadBalancer, cloud ortamındaki sağlayıcının yönettiği bir yük dengeleyici oluşturur ve uygulama servislerini yük dengeleyici üzerinden dış dünyaya açar.

Örneğin, bir web uygulaması çalıştırdığınızı ve bu web uygulamasının arkasında üç farklı pod olduğunu varsayalım. Bu pod'lar, uygulama sunucusu olarak hizmet vermektedir. LoadBalancer Service objesi, bu pod'ları tek bir IP adresi altında gruplandırır ve belirli bir port numarası üzerinden erişilebilir hale getirir. Ayrıca, cloud sağlayıcısı tarafından yönetilen bir yük dengeleyici oluşturur ve uygulama servislerini yük dengeleyici üzerinden dış dünyaya açar.

Aşağıdaki YAML belgesi, bir LoadBalancer Service objesi oluşturmak için kullanılabilir:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-webapp-service
spec:
  selector:
    app: my-webapp
  type: LoadBalancer
  ports:
    - name: http
      protocol: TCP
      port: 80
      targetPort: 8080
```

Bu YAML belgesi, my-webapp-service adlı bir LoadBalancer Service objesi oluşturur. Bu Service objesi, my-webapp adlı pod'ları hedef alır ve 80 numaralı bir porttan erişilebilir hale getirir. Service objesi, hedef pod'ların 8080 numaralı portuna yönlendirir ve cloud sağlayıcısı tarafından yönetilen bir yük dengeleyici oluşturur.

Bu LoadBalancer Service objesi, cloud sağlayıcısı tarafından yönetilen yük dengeleyici üzerinden dış dünyaya açılabilir hale getirilir. Bu sayede, uygulama servisleri, dış dünyadan gelen isteklere karşılık verebilir hale gelir. Cloud sağlayıcısı, LoadBalancer Service objesi için özel bir IP adresi atar ve uygulama servisleri bu IP adresi üzerinden erişilebilir hale gelir.

### ExternalName

Uygulama servislerine DNS adları üzerinden erişmek için kullanılır. ExternalName Service objesi, cluster içinde bir DNS ismi olarak erişilebilen, farklı bir dış DNS adıyla eşleştirilir.

Örneğin, bir web uygulaması çalıştırdığınızı ve bu web uygulamasına erişmek için bir DNS adı kullanmak istediğinizi varsayalım. Ancak, web uygulaması bir başka cloud ortamındaki kaynakta çalışıyor ve erişilebilirliği sağlamak için bir DNS adı kullanıyor. ExternalName Service objesi, bu senaryoda kullanılabilir.

Aşağıdaki YAML belgesi, bir ExternalName Service objesi oluşturmak için kullanılabilir:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-webapp-service
spec:
  type: ExternalName
  externalName: my-webapp.example.com
```

Bu YAML belgesi, my-webapp-service adlı bir ExternalName Service objesi oluşturur. Bu Service objesi, cluster içinde my-webapp.example.com DNS adı olarak erişilebilir hale getirir. Bu DNS adı, dış dünyadaki bir web uygulamasına yönlendirilir ve uygulamaya erişmek için kullanılabilir.

Bu ExternalName Service objesi, cluster içinde DNS adı olarak kullanılabilir hale gelir. Bu sayede, diğer pod'lar, bu Service objesini hedefleyerek, my-webapp.example.com DNS adı üzerinden uygulamaya erişebilirler. Bu erişim, ExternalName Service objesi tarafından yönlendirilerek, dış dünyadaki uygulamaya yönlendirilir.

## Dikkat Edilmesi Gerekenler

- Pod ve Service objeleri arasındaki etiketleme tutarlılığına dikkat edin. Service objesi, etiketlerle belirlenen podları yönlendirecektir, bu nedenle Service ve pod etiketlerinin örtüşmesi gerekir.
- Service objesi türünü doğru şekilde belirleyin. ClusterIP, NodePort, LoadBalancer veya ExternalName türleri mevcuttur ve her birinin farklı kullanım durumları vardır. Bu nedenle, ihtiyacınıza en uygun olan Service türünü belirlemeniz önemlidir.
- Hedef podların sağlığını kontrol edin. Service objesi, yönlendirdiği podların sağlığını kontrol etmez. Bu nedenle, hedef podlarının sağlığını kontrol eden bir readinessProbe veya livenessProbe belirlemek önemlidir.
- Service objesi konfigürasyonunu güncellediğinizde, podların otomatik olarak yeniden başlatılmayacağına dikkat edin. Bu nedenle, Service objesi konfigürasyonunu değiştirdiğinizde, ilgili podları da manuel olarak yeniden başlatmanız gerekebilir.
- Service objelerinin trafik yönlendirmesi için kullanılan portları iyi bir şekilde belirleyin. Service objelerinin, diğer podlar veya hizmetler ile çakışmadığından emin olmak için benzersiz bir port numarası kullanın.
