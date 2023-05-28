# Environment Variables

Kubernetes ortam değişkenleri, bir Kubernetes pod veya container'ına ilişkin değişkenlerdir. Pod veya container'ın çalıştığı zaman çevresel değişkenler olarak kullanılabilirler.

Bu değişkenler, genellikle önceden tanımlanmış ve sabit değerlerden oluşur ve pod veya container'ın hangi durumda çalıştırılacağına bağlı olarak belirlenir. Kubernetes, podlar ve konteynerler için önceden tanımlanmış bazı ortam değişkenlerini sağlar, ancak kullanıcıların da kendi değişkenlerini tanımlayabileceği esnek bir yapı sunar.

Örneğin, bir uygulamanın veritabanı bağlantı dizesi genellikle bir ortam değişkeni olarak kullanılır. Böylece uygulamanın farklı veritabanlarına bağlanması gerektiğinde sadece ortam değişkeni değiştirilir ve uygulama yeniden başlatılır.

Ortam değişkenleri, Kubernetes pod ve container konfigürasyon dosyalarında belirtilir. Bunlar genellikle YAML formatında tanımlanır. Örneğin:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-container
      image: my-image
      env:
        - name: MY_ENV_VAR
          value: my-env-value
```

Yukarıdaki örnekte, MY_ENV_VAR adlı bir ortam değişkeni tanımlanmıştır ve my-env-value değeri atanmıştır. Bu pod başlatıldığında, my-container içindeki herhangi bir işlemin MY_ENV_VAR değişkenini kullanabilmesi için hazır olacaktır.
