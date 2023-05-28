# Volume

## Kubernetes Volume nedir?

Kubernetes Volume, Kubernetes podları içinde kullanılan veri depolama birimleridir. Bir Kubernetes podu, bir veya daha fazla konteyneri içerebilir ve bu konteynerler arasında veri paylaşımı gerektiğinde, bir pod seviyesinde bir depolama birimi kullanılması gerekmektedir.

Kubernetes Volume, bir pod içindeki konteynerler arasında veri paylaşımını sağlamak için kullanılan bir Kubernetes kaynağıdır. Bu depolama birimi, podun yaşam döngüsü boyunca kullanılabilir ve verilerin silinmesi, depolanması ve geri yüklenmesi işlemlerini otomatik olarak yönetir.

Kubernetes Volume'lerinin birçok türü vardır, bunlar arasında Azure Disk, Amazon EBS, NFS, iSCSI, ConfigMap, Secret ve EmptyDir gibi seçenekler bulunur. Podlarda kullanılan Volume'ler, podlar oluşturulurken veya güncellenirken tanımlanabilir ve kullanılan depolama kaynağına göre yapılandırılabilir.

Kubernetes Volume'lerinin avantajları arasında, veri paylaşımını kolaylaştırması, veri kaybını önlemesi ve podların bağımsızlığını artırması yer alır. Dezavantajları ise, disk alanı tüketebilmesi, veri depolama maliyetlerini artırması ve hata ayıklama işlemlerini zorlaştırması olabilir.

## Volume Tipleri

- EmptyDir
- HostPath
- GitRepo
- ConfigMap
- Secret

## EmptyDir

emptyDir, Kubernetes'te kullanılan bir volume tipidir ve geçici verilerin depolanması için kullanılır. Bu volume türü, bir pod çalıştırıldığında oluşturulur ve bu pod silindiğinde de otomatik olarak temizlenir.

Bir emptyDir volume'ü oluşturduktan sonra, pod'ların içinde çalışan tüm container'lar bu volume'e erişebilirler. Bu, bir pod'daki container'ların birbirleriyle geçici bir şekilde veri paylaşmasına izin verir. emptyDir volume'ü, özellikle bir pod'da birden fazla container kullanıldığında, container'lar arasında geçici bir veri paylaşımı gerektiğinde veya pod'un bir sonraki aşamasında kullanmak üzere geçici bir depolama alanına ihtiyaç duyulduğunda kullanışlı olabilir.

Ancak, emptyDir volume'leri pod silindiğinde tamamen silinir, bu nedenle verilerin kalıcı olması gerekiyorsa başka bir volume türü kullanmak gerekmektedir.

Aşağıdaki YAML konfigürasyon örneğinde, emptyDir adında bir Volume oluşturuluyor ve web adındaki Pod'a bağlanıyor:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: web
spec:
  containers:
    - name: nginx
      image: nginx
      volumeMounts:
        - name: cache-volume
          mountPath: /cache
  volumes:
    - name: cache-volume
      emptyDir: {}
```

Yukarıdaki örnekte, emptyDir Volume'unun ismi cache-volume olarak belirtiliyor. Pod'un içindeki nginx konteynerinde /cache dizinine bağlanıyor. Bu sayede Pod içindeki konteynerler arasında geçici veri saklamak için kullanılabilir.

## HostPath

hostPath, Kubernetes cluster'ındaki bir node'un dosya sistemi yolunu bir volume olarak kullanmamızı sağlayan bir volume tipidir. Böylece pod'lar doğrudan host makinanın dosya sistemi ile iletişim kurabilir.

Bu volume tipi, genellikle test, geliştirme veya single-node cluster'ların oluşturulmasında kullanılır. Ancak, multi-node cluster'larında hostPath kullanmak, yüksek kullanılabilirlik ve ölçeklenebilirlik açısından önerilmez.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
    - name: mycontainer
      image: myimage
      volumeMounts:
        - name: myvolume
          mountPath: /mnt/mydata
  volumes:
    - name: myvolume
      hostPath:
        path: /var/data
        type: Directory
```

Yukarıdaki örnekte, mypod adlı pod, /var/data dizinini mycontainer adlı container'da /mnt/mydata mount point'inde kullanır. Bu, host makinanın /var/data dizinindeki tüm dosyaların, mypod pod'unda /mnt/mydata altında kullanılabileceği anlamına gelir.

## GitRepo

GitRepo, bir Git deposundan alınan bir kaynak kodunu Kubernetes pod'una bir volume olarak eklemek için kullanılan bir tipdir. GitRepo volume tipi, bir depo adı, bir branch veya tag adı, bir volume adı ve kullanılacak Git depo kimlik bilgileri gibi birkaç parametre gerektirir. Bu volume tipi, özellikle bir uygulamanın bir Git deposundan alınan kaynak kodunu kullanarak çalıştırılması gerektiğinde yararlıdır.

Örnek olarak, aşağıdaki YAML, bir Git deposundan bir kaynak kodu alarak bir volume oluşturur ve bu volume'u bir Pod'a bağlar:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-container
      image: my-image
      volumeMounts:
        - name: git-volume
          mountPath: /my-app
  volumes:
    - name: git-volume
      gitRepo:
        repository: https://github.com/my-user/my-repo
        revision: master
        directory: my-app
```

Yukarıdaki örnekte, gitRepo volume tipi kullanılarak my-user/my-repo deposundan master dalındaki my-app dizinindeki kaynak kodu alınır. Bu kaynak kodu, my-pod adlı Pod'da /my-app dizinine bağlanan git-volume adlı bir volume'a eklenir. Bu sayede, my-container adlı konteyner, /my-app dizinindeki kaynak kodunu kullanabilir.

## ConfigMap

Kubernetes'te configMap bir yapılandırma ayarlarını depolamak için kullanılan bir objedir. Bir configMap nesnesi, podların kullanabileceği birden çok yapılandırma ayarını depolayabilir. Her bir ayar, bir anahtar-değer çifti olarak depolanır. configMap ile depolanan ayarlar, env, arg veya bir dosya olarak kullanılabilir.

Örneğin, bir uygulamanız var ve farklı ortamlarda (geliştirme, test ve prod) kullanmak üzere ayarlarınız var. Bu ayarları her ortam için farklı bir şekilde saklamanız gerekiyor. configMap'leri kullanarak, bu ayarları her ortam için farklı bir configMap objesi olarak depolayabilirsiniz.

Örneğin, aşağıdaki gibi bir configMap objesi tanımlayabilirsiniz:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  DB_HOST: db.example.com
  DB_PORT: '5432'
  API_URL: http://api.example.com
```

Bu örnekte, app-config adlı bir configMap objesi oluşturulur. DB_HOST, DB_PORT ve API_URL gibi anahtar-değer çiftleri, farklı ortamlarda kullanılmak üzere ayarlar olarak tanımlanır.

Daha sonra, bu configMap objesi podlarda kullanılabilir. Örneğin, aşağıdaki gibi bir pod tanımı kullanarak configMap objesindeki ayarları env olarak kullanabilirsiniz:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-app
      image: my-image
      env:
        - name: DB_HOST
          valueFrom:
            configMapKeyRef:
              name: app-config
              key: DB_HOST
        - name: DB_PORT
          valueFrom:
            configMapKeyRef:
              name: app-config
              key: DB_PORT
        - name: API_URL
          valueFrom:
            configMapKeyRef:
              name: app-config
              key: API_URL
```

Bu örnekte, my-pod adlı bir pod tanımlanır ve app-config adlı configMap objesi kullanılarak pod içindeki konteynerlerin env değişkenleri ayarlanır. configMapKeyRef özelliği, configMap objesindeki ayarların nasıl alınacağını belirtir.

## Secret

Kubernetes'de Secret Volume, verileri şifrelenmiş bir şekilde depolamak için kullanılan bir depolama türüdür. Bu tür bir volume ile veriler, Kubernetes Secret nesnelerinde tutulur ve pod'larda bu secret volume'una bağlanarak kullanılabilir.

Örneğin, bir uygulama veritabanı şifreleri gibi hassas bilgiler içeriyorsa, bu bilgilerin açık şekilde kaydedilmesi uygun olmaz. Bu tür hassas bilgileri, Kubernetes Secret nesnelerinde saklayabiliriz. Daha sonra bu Secret nesnelerini pod'lara Volume olarak bağlayarak, bu hassas bilgilere pod'larda erişebiliriz.

Örneğin bir deployment'ta bir container çalıştırdığımızı ve bu container'ın bir secret dosyasına ihtiyacı olduğunu varsayalım. Öncelikle bir secret oluşturmalıyız:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: mysecret
type: Opaque
data:
  username: dXNlcg==
  password: cGFzc3dvcmQ=
```

> Kubernetes Secret objesi içerisinde data ve stringData alanlarına sahip olabilir. Her iki alan da Secret objesi içerisinde değer saklamak için kullanılır ancak aralarında bazı farklılıklar vardır.

> data alanı, Base64 kodlu verileri saklamak için kullanılır. Bu nedenle, verileri kaydetmek için genellikle bir Base64 kodlayıcı kullanmak gerekir. Veriler, anahtar-değer çiftleri şeklinde saklanır. Bu alan, gizli anahtarları ve sertifikaları gibi hassas verileri saklamak için kullanılır.

> stringData alanı ise Base64 kodlaması yapmadan doğrudan açık metin olarak verileri saklamak için kullanılır. Bu nedenle, verileri saklamak için herhangi bir kodlayıcı kullanmak gerekmez. Veriler yine anahtar-değer çiftleri şeklinde saklanır. Bu alan, genellikle basit metin verileri gibi hassas olmayan verileri saklamak için kullanılır.

> Özetle, data alanı hassas verileri saklamak için kullanılırken, stringData alanı hassas olmayan verileri saklamak için kullanılır.

Bu secret dosyasını oluşturduktan sonra, pod'da kullanmak için aşağıdaki şekilde kullanabiliriz:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
    - name: mycontainer
      image: myimage
      volumeMounts:
        - name: secret-volume
          mountPath: /etc/mysecret
          readOnly: true
  volumes:
    - name: secret-volume
      secret:
        secretName: mysecret
```

Bu YAML dosyası, mypod adlı bir pod yaratır ve mycontainer adlı bir container içerir. Bu container, /etc/mysecret yoluna read-only bir volume bağlar. Volume'un adı secret-volume olarak belirtilir ve bu volume, mysecret adlı secret dosyasını kullanarak oluşturulur.

Artık bu pod çalıştırıldığında, mycontainer adlı container /etc/mysecret yolunda mysecret dosyasına erişebilir ve içerdiği username ve password bilgilerini kullanabilir. Bu şekilde, hassas bilgileri container'ımızın içine gömmek yerine secret dosyaları kullanarak daha güvenli bir yöntemle saklayabiliriz.

### ConfigMap vs Secret Volume

Kubernetes'te ConfigMap ve Secret, uygulama yapılandırması ve gizli bilgilerin depolanması için kullanılan iki kaynak türüdür. ConfigMap, anahtar-değer çiftleri aracılığıyla yapılandırma bilgilerini depolamak için kullanılırken, Secret, duyarlı bilgileri (örneğin, şifreler, API anahtarları) depolamak için kullanılır.

ConfigMap'lar, bir uygulamanın çeşitli yapılandırma ayarlarını depolamak için kullanışlıdır. Örneğin, bir web uygulamasında kullanılan farklı yapılandırma dosyalarının tümünü tek bir ConfigMap içinde depolayabilirsiniz. Bu, yapılandırma değişikliklerini daha kolay yönetmenizi sağlar. ConfigMap'lar, uygulama yapılandırmasını değiştirmek için manuel olarak veya bir konfigürasyon yönetimi aracı (örneğin, Helm) kullanarak güncellenebilir.

Secret'lar, gizli verilerin depolanması için kullanılır. Örneğin, bir veritabanı şifresi veya bir API anahtarı gibi duyarlı bilgileri Secret içinde depolayabilirsiniz. Secret'lar, ConfigMap'lar gibi anahtar-değer çiftleri aracılığıyla depolanır ve kullanım sırasında otomatik olarak deşifre edilir. Secret'lar, genellikle bir uygulamanın Pod'larında bulunan konteynerlerin gizli bilgilere erişmesi gerektiğinde kullanılır.

Özetle, ConfigMap'lar uygulama yapılandırması için, Secret'lar ise duyarlı bilgilerin depolanması için kullanılır.

## Dikkat Edilmesi Gerekenler

- Verilerinizin kaybolmasını önlemek için doğru depolama sınıfını seçin. Örneğin, Ephemeral Volume'ler önbellek için kullanılabilirken, Persistant Volume'ler daha kalıcı veri saklama için tercih edilir.
- Volume'lerin hangi pod'lar tarafından kullanılacağını belirleyin. Pod'larda birden fazla konteyner olduğunda, tüm konteynerlerin aynı volume'ü kullanması gerekiyorsa bunu belirtmelisiniz.
- Farklı sınıflardaki Volume'leri farklı pod'lar kullanıyorsa, birden fazla pod üzerindeki değişiklikleri takip etmek için kaynak kısıtlamalarını tanımlayın.
- Volume'lerin veri bütünlüğünü korumak için doğru şekilde yapılandırıldığından emin olun. Örneğin, veri bütünlüğü sağlamak için hazırda bekletilmiş ve ayrıcalıklı bir konumda depolanması gereken verilerin kullanımı için HostPath Volume kullanmak uygun değildir
- Yapılandırmanızda Volume'lerin temizlenmesi veya silinmesi için gerekli prosedürleri belirleyin. Örneğin, bir Pod veya Deployment silindiğinde Volume'ün otomatik olarak silinmesini sağlayabilirsiniz.
