# Label

Label'lar, Kubernetes ortamında kaynaklara (örneğin Pod'lar, Servisler, ReplicaSet'ler vb.) metaveriler eklemek için kullanılan anahtar-değer çiftleridir. Bir kaynağa bir veya daha fazla Label atanabilir ve bu Label'lar, kaynakların gruplandırılması, sorgulanması ve seçilmesi için kullanılabilir. Label'lar, birçok farklı amaç için kullanılabilir, örneğin çevresel ayrımlar, uygulama bileşenleri, versiyon kontrolü vb.

Label'lar, kaynak tanımlama dosyalarında veya Kubernetes API'si aracılığıyla tanımlanır. Label'lar, genellikle key-value (anahtar-değer) çifti olarak ifade edilir. Örneğin, bir Pod'a "app=backend" veya "env=production" gibi Label'lar atanabilir.

```yaml
metadata:
  labels:
    key1: value1
    key2: value2
```

## Label'ların Avantajları

- Gruplama ve Kategorizasyon: Label'lar, kaynakların belirli kriterlere göre gruplandırılmasını sağlar. Örneğin, bir uygulama içindeki tüm frontend Pod'larını bir Label aracılığıyla gruplayabilirsiniz.

- Seçme ve Filtreleme: Label'lar, belirli özelliklere veya koşullara sahip kaynakları seçmek veya filtrelemek için kullanılabilir. Örneğin, "env=production" Label'ına sahip olan tüm kaynakları seçmek istediğinizde bu Label'ı kullanabilirsiniz.

- Versiyon Kontrolü: Label'lar, kaynakların versiyon kontrolü için kullanılabilir. Örneğin, bir uygulama için farklı sürümleri etiketleyerek, kaynaklarınızı kolayca yönetebilirsiniz.

## Uygulama

Örnek Senaryo: Bir e-ticaret uygulaması için frontend ve backend olmak üzere iki ayrı uygulama bileşeni bulunmaktadır. Bunları Kubernetes ortamında dağıtmak ve yönetmek istiyoruz.

1. Öncelikle, frontend ve backend uygulamaları için ayrı ayrı Pod tanımları oluşturuyoruz. Frontend uygulaması için "app=frontend" ve backend uygulaması için "app=backend" şeklinde Label'ları tanımlıyoruz.

```yaml
# frontend-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: frontend-pod
  labels:
    app: frontend
spec:
  containers:
    - name: frontend-container
      image: frontend-image

# backend-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: backend-pod
  labels:
    app: backend
spec:
  containers:
    - name: backend-container
      image: backend-image
```

2. Kaynak tanımlarını Kubernetes'e uyguluyoruz:

```bash
kubectl apply -f frontend-pod.yaml
kubectl apply -f backend-pod.yaml
```

3. Oluşturulan Pod'ları kontrol ederken, Label'ları kullanarak belirli bileşenleri seçebiliriz:

```bash
kubectl get pods -l app=frontend
kubectl get pods -l app=backend
```

4. Eğer frontend uygulamasında bir güncelleme yapmak istiyorsak, Pod tanımında değişiklik yaparız ve güncellemeyi uygularız:

```bash
kubectl apply -f frontend-pod.yaml
```

Bu şekilde, Label'lar sayesinde frontend ve backend uygulamalarını kolayca gruplayabilir, seçebilir ve güncelleyebiliriz. Ayrıca, farklı sürümleri veya çevreleri temsil etmek için ek Label'lar ekleyebilir ve bu şekilde kaynakları daha iyi yönetebiliriz.
