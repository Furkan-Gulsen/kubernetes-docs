# Rollout ve Rollback

## Rollout Nedir?

- Bir uygulamanın yeni bir sürümünün dağıtımını başlatmak için kullanılır.
- Bir önceki sürümden farklı olarak yeni bir sürümde yer alan güncellemeleri sağlar.
- Yeni bir sürümün başarılı bir şekilde dağıtımı için bir dizi adımı otomatikleştirir.
- Yeni bir sürümün yavaş yavaş tüm pod'lara dağıtılmasını ve eski sürümün aşamalı olarak kaldırılmasını sağlar.
- Uygulamanın yeni sürümüne geçiş sırasında yaşanabilecek olası kesintileri en aza indirir.

## Rollback Nedir?

- Uygulamanın bir önceki sürümüne geri dönmek için kullanılır.
- Bir uygulama sürümünün yanlışlıkla veya hatalı bir şekilde dağıtıldığı durumlarda kullanılır.
- Uygulama sürümünün hızlı bir şekilde geri alınmasını ve bir önceki çalışan sürüme geçiş yapılmasını sağlar.
- Geri alma işlemini otomatikleştirir ve hızlı bir şekilde tamamlanmasını sağlar.

## Uygulama

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
  labels:
    app: myapp
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
        - name: myapp
          image: myapp:v1
          ports:
            - containerPort: 80
```

Bu deployment nesnesinde image alanı, myapp:v1 olarak tanımlanmıştır. Eğer bu image güncellenmek istenirse, örneğin myapp:v2 sürümüne geçmek istenirse, aşağıdaki komut kullanılabilir:

```shell
kubectl set image deployment/myapp-deployment myapp=myapp:v2
```

Bu komut, deployment nesnesindeki myapp isimli container için kullanılan image'ı myapp:v2 olarak güncelleyecektir. Bu güncelleme, varsayılan olarak, tüm replica setlerinde aynı anda gerçekleştirilir. Rollout işlemi sırasında, deployment nesnesinin status alanı güncellenir. Aşağıdaki komut kullanılarak deployment nesnesinin rollout durumu takip edilebilir:

```shell
kubectl rollout status deployment/myapp-deployment
```

Bu komut, deployment nesnesinin rollout işleminin tamamlanıp tamamlanmadığını kontrol eder. Eğer güncelleme işlemi tamamlanmışsa, aşağıdaki komut kullanılarak yeni image sürümüne uygun pod'ların listesi alınabilir:

```shell
kubectl get pods -l app=myapp
```

Bu komut, app=myapp label'ına sahip pod'ların listesini verir. Artık pod'ların yeni image sürümüne sahip olması beklenir. Eğer güncelleme işlemi hatalı bir şekilde sonuçlanırsa, rollback işlemi kullanılarak önceki sürüme geri dönülebilir.

## Strateji Belirleme

> Deployment'ın yeniden oluşturulma stratejisini strategy adında bir ifade belirler. Bu ifadenin default değeri RollingUpdate

RollingUpdate stratejisi, yeni bir versiyonu yavaş yavaş deployment'a dahil ederek eski versiyonun yerini değiştirirken, Recreate stratejisi mevcut tüm pod'ları öldürür ve yeni versiyon için tamamen yeni bir set oluşturur. RollingUpdate stratejisi, kullanıcılara sıfır downtime sağlamak için daha çok tercih edilirken, Recreate stratejisi daha basit bir yaklaşım sunar ve eski versiyondan tamamen kurtulmak istenildiğinde kullanılabilir.

### Recreate Kullanımı

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: rcdeployment
  labels:
    team: development
spec:
  replicas: 3
  selector:
    matchLabels:
      app: recreate
  strategy:
    type: Recreate # Recreate stratejisi burada tanımlanıyor
  template:
    metadata:
      labels:
        app: recreate
    spec:
      containers:
        - name: nginx
          image: nginx:1.20 # yeni bir sürüm tanımlanıyor
          ports:
            - containerPort: 80
```

Bu YAML dosyasında strategy bölümünde Recreate stratejisi tanımlandı. Bu durumda güncelleme işlemi sırasında, önceki versiyon silinir ve yeni versiyon oluşturulur. Bu, tüm pod'ların yeniden oluşturulması anlamına gelir ve bu nedenle geçici bir çalışmazlık süresine yol açabilir.

### RollingUpdate Kullanımı

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:1.19.3
          ports:
            - containerPort: 80
      terminationGracePeriodSeconds: 10
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1
      maxSurge: 1
```

Bu örnekte, Deployment nesnesi, spec bölümünde belirtilen özelliklere sahip bir ReplicaSet oluşturur. ReplicaSet daha sonra bir veya daha fazla Pod'u barındıracak şekilde scale edilir. strategy bölümünde, type: RollingUpdate olarak ayarlandığı için, yeni bir güncelleme yayınlandığında, Kubernetes önce yeni Pod'ları oluşturur ve eski Pod'ları siler, bu sayede sıfır kesinti süresi (zero-downtime) elde edilir. rollingUpdate bölümü, aynı anda kaç tane Pod'un güncellenebileceğini ve kaç tane yeni Pod oluşturulabileceğini belirtir. Bu örnekte, maxUnavailable değeri 1 ve maxSurge değeri de 1 olarak ayarlandı, bu da Kubernetes'in aynı anda sadece bir Pod'u devre dışı bırakacağı ve bir Pod daha oluşturacağı anlamına gelir.

### Record Kullanımı

**--record** parametresi, kubectl komutlarındaki değişikliklerin kaydedilmesine ve bu değişikliklerin sonrasında kullanılabilecek bir kayıt oluşturulmasına olanak tanır. Bu parametre, kubectl kullanarak bir nesne oluşturulduğunda, güncellendiğinde veya silindiğinde, bir kayıt oluşturulmasını sağlar.

Bu kayıt, sonrasında kubectl rollout history komutu kullanılarak geçmiş değişiklikleri görüntülemek veya bir önceki sürüme geri dönmek gibi işlemlerde kullanılabilir. Özellikle güncelleme işlemleri için kayıt oluşturmak, geri alma işlemlerinde oldukça faydalıdır.

Örnek olarak, kubectl create veya kubectl apply komutlarının sonuna --record parametresi eklenerek kayıt oluşturulabilir:

```yaml
kubectl apply -f deployment.yaml --record
```

### Geçmişe Dönme

Kubernetes'te Deployment'lar üzerinden kaydedilen kayıtların arasında geçiş yapmak için kubectl rollout undo komutu kullanılabilir. Aşağıdaki örnek, my-deployment adlı bir Deployment'ın son yapılandırmasına geri dönmek için bir kayıt geçişini gerçekleştirir:

```shell
kubectl rollout undo deployment/my-deployment
```

Bu komut, my-deployment Deployment'ının son yapılandırmasına geri döner. --record seçeneği kullanılarak, geri alma işleminin kaydı tutulabilir:

```shell
kubectl rollout undo deployment/my-deployment --to-revision=2 --record
```

Bu komut, my-deployment Deployment'ının 2. yapılandırmasına geri döner ve geri alma işleminin kaydını tutar. Daha sonra kubectl rollout history komutu kullanılarak kaydedilen kayıtların listesi görüntülenebilir:

```shell
kubectl rollout history deployment/my-deployment
```

Bu komut, my-deployment Deployment'ının tarihçesini görüntüler. --revision seçeneği kullanılarak belirli bir kaydın detaylarına bakılabilir:

```shell
kubectl rollout history deployment/my-deployment --revision=3
```

Bu komut, my-deployment Deployment'ının 3. kaydının detaylarını görüntüler.

## Status Kullanımı

Bu komutlar kullanıldığında, deployment üzerindeki güncelleme işleminin durumu kubectl rollout status komutu ile görüntülenebilir. Bu komut, deployment'in güncelleme işleminin ne durumda olduğunu gösterir ve güncelleme işlemi tamamlanana kadar bekleyebilirsiniz.

Örnek olarak, aşağıdaki komut ile myapp-deployment deployment'inin güncelleme işlemi durdurulabilir:

```shell
kubectl rollout pause deployment/myapp-deployment
```

Daha sonra, güncelleme işlemi devam ettirilebilir:

```shell
kubectl rollout resume deployment/myapp-deployment
```

## Pause ve Resume Kullanımı

Kubernetes'te deployment üzerinde yapılan bir güncelleme işlemi sırasında, belirli bir noktada işlemleri durdurmak ve sonra devam ettirmek isteyebilirsiniz. Bu durumda rollout pause ve rollout resume komutları kullanılır.

- **rollout pause:** Güncelleme işlemini durdurmak için kullanılır.

Örneğin, aşağıdaki komut ile deployment üzerindeki güncelleme işlemini durdurabilirsiniz:

```shell
kubectl rollout pause deployment/myapp-deployment
```

- **rollout resume:** Güncelleme işlemini devam ettirmek için kullanılır.

Örneğin, aşağıdaki komut ile deployment üzerindeki güncelleme işlemini devam ettirebilirsiniz:

```shell
kubectl rollout resume deployment/myapp-deployment
```

## Dikkat Edilmesi Gerekenler

1. Deployment'ın yaml dosyasındaki strateji kısmı doğru belirlenmelidir. Bu, deployment'ın nasıl yayınlanacağını, kaç pod'un çalışacağını, eski pod'ların nasıl ele alınacağını vb. belirler.
2. Rollout işlemi sırasında, kubernetes objelerinde yapılan değişikliklerin doğru bir şekilde kontrol edilmesi önemlidir.
3. Rollout işlemi sırasında, yeni deployment versiyonuna geçerken kaynak tüketimi ve performans konuları göz önünde bulundurulmalıdır.
4. Rollout sırasında takip edilebilir bir strateji uygulamak için, kubectl gibi araçlar kullanarak rollout sırasında pod'ların ve deployment'ın durumunu takip etmek önemlidir.
5. Rollout sırasında, hata durumunda geri alma (rollback) işlemi için hazırlıklı olmak gerekir. Bu nedenle, deployment'ın eski versiyonlarına kolayca geri dönülebilmesi için geçmiş versiyonların saklanması önemlidir.
