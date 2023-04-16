# Ağ Kuralları

## Ağ Kuralları Nedir?

Kubernetes ağ kuralları, Kubernetes kümesindeki pod'ların hangi ağ kaynaklarına erişebileceğini kontrol etmek için kullanılır. Ağ kuralları, pod'lar arasındaki ağ trafiğini kontrol etmek, pod'ların dış dünya ile iletişim kurmasını sağlamak ve kümedeki kaynakların güvenliğini sağlamak için kullanılabilir.

Kubernetes ağ kuralları aşağıdaki şekillerde tanımlanabilir:

- **Pod seviyesinde:** Pod'a doğrudan erişimi kontrol etmek için pod özelliklerini kullanarak ağ kısıtlamaları tanımlanabilir.
- **Namespace seviyesinde:** Namespace seviyesinde ağ politikaları oluşturularak tüm pod'ların ağ trafiğini kontrol etmek mümkündür.

- **Küme seviyesinde:** Tüm pod'ların ağ trafiğini kontrol etmek için küme seviyesinde ağ politikaları oluşturulabilir.

Ağ kısıtlamaları, ağ politikaları aracılığıyla uygulanabilir. Ağ politikaları, belirli bir pod veya pod seti tarafından üretilen trafiği kontrol etmek için kullanılan bir Kubernetes öğesidir. Ağ politikaları, gelen ve/veya giden trafiği filtrelemek ve düzenlemek için belirli kriterlerle tanımlanabilir.

Örneğin, bir ağ politikası, belirli bir etikete sahip pod'ların sadece belirli bir başka etikete sahip pod'larla iletişim kurmasına izin verebilir. Bu, pod'lar arasındaki trafiği sınırlandırarak ağ güvenliğini artırabilir.

<br>

## Senaryolar

Konun daha iyi anlaşılması için aşağıdaki yapı üzerinden 3 farklı senaryo oluşturacağız.

<img src="https://965066212-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FIWxgpIZWYV2RbSJFQUb6%2Fuploads%2FAKmptqFNgG0KxYsAtqgz%2Fimage.png?alt=media&token=b043e314-0944-4e43-80eb-1f5067f2615d">

- **pod(sarı):** Kubernetes içindeki en küçük ve en temel uygulama öğesi olan, bir veya birden fazla konteynırdan oluşan ve aynı ağ ve depolama kaynaklarını paylaşan uygulama gruplarıdır.
- **Router:** cluster içerisindeki pod'lara gelen ağ trafiğini doğru pod'a yönlendirmek için kullanılan bir bileşendir.
- **Virtual Bridge (mavi):** Ağdaki sanal makinelerin birbirleriyle iletişim kurabilmesi ve ağ trafiğini yönetebilmek için kullanılan bir sanal ağ bileşenidir.
- **--pord-network-cidr:** Kubernetes cluster'ında Podlar için kullanılacak olan IP adreslerini belirlemek için kullanılır. Bu CIDR notasyonu, Podların IP adreslerinin atanacağı ağ adres aralığını tanımlar.
- **NAT (natlanmak):** Ağ bağlantısı yapılandırması sırasında kullanılan bir terimdir. "NAT" (Network Address Translation) olarak kısaltılan "Ağ Adresi Çevirisi", bir ağ üzerindeki bir bilgisayarın veya cihazın, diğer bir ağa erişmesini sağlar. NAT, özel IP adreslerini internete yönelik genel IP adresleriyle değiştirir ve bu sayede özel ağdaki cihazlar internete bağlanabilir. "Natlanmak" terimi, NAT kullanılarak bir ağdaki cihazların internete erişimini ifade eder.

<br>

### Senaryo - 1

<img src="https://965066212-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FIWxgpIZWYV2RbSJFQUb6%2Fuploads%2F5SKDPNS5QzImKyli9D8R%2Fimage.png?alt=media&token=a13381e9-c1a2-40c2-ae89-8c4bdfb35ea0">

**Senorya 1 için:** Burada podA'nın dış dünyada bulunan bir sunucu ile iletişimi hedefleniyor.

Bu durumda podA iletişim paketlerini virtual bridge'e teslim edecek, paket buradan worker node'un eth0 interface'ine ulaşıp natlanacak. Daha sonra paket en son olarak Router'a ulaşıp burada da natlandıktan sonra 1.1.1.1 sunucu ile iletişim sağlanacak. Bu sunucudan geri gelen pakette aynı şekilde natlanıp podA'a ulaşacak. Bu senaryoda bir problem yok.

> eth0 => Ethernet ağ teknolojisi üzerinden ağ bağlantısı sağlar ve ağa bağlı cihazlarla iletişim kurmak için kullanılır.

<br>

### Senaryo - 2

<img src="https://965066212-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FIWxgpIZWYV2RbSJFQUb6%2Fuploads%2FzJlRNeUBgIdEjB2JezRA%2Fimage.png?alt=media&token=6bec05d8-d9aa-4132-9393-a89f15a33ac7">

**Senaryo 2 için:** Burada podA aynı worker-1 içerisinde bulunan podB ile iletişime geçmesi hedefleniyor.

podA paketi virtual bridge'e teslim edecek. Virtual Bridge podA'dan gelen paketi sorunsuz bir şekilde podB'e iletecek. podB'den gelen cevapta aynı şekilde; podB cevabı virtual bridge'e iletecek, bridge gelen cevap paketini podA'a iletecek. Bu senaryoda da sıkıntı yok.

<br>

### Senaryo - 3

<img src="https://965066212-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FIWxgpIZWYV2RbSJFQUb6%2Fuploads%2FR4fMzf7lZdIExyL63vd2%2Fimage.png?alt=media&token=1d72dab5-6b26-413d-8096-191ed252de61">

Senaryo 3 için: worker-1'de bulunan PodA, worker-2'de bulunan PodC ile iletişime geçmesi hedefleniyor.

> Podlar birbiri ile NAT'a ihtiyaç duymadan iletişim kurabilmeliydi. Senaryo2'de bunu kanıtladık. Fakat burada, podA'nın Virtual Bridge'e ilettiği paket podC ile aynı worker üzerinde olmadığı için NAT'sız bir iletişim nasıl söz konusu olabiliyor?

Burada bunu çözecek birkaç düzenlemede olsa bunlar her cluster yapısı için düzgün çalışmayacaktır. Farklı clusterlar arası iletişimde kullanılabilecek Kubernetes içerisinde olan "Container Network Interface (CLI)" bu sorunu çözebilecek en uygun yapıdır.

CNI, Kubernetes'te kullanılan bir arayüzdür ve ağların podlar arasında iletişim kurmasını sağlar. Podlar arasındaki iletişim, CNI tarafından kurulan ağ arabirimleri sayesinde gerçekleştirilir.

CNI kullanarak podA ve podC arasında iletişim kurmak için şu adımlar takip edilebilir:

- **CNI eklentisi kurulur:** Öncelikle, Kubernetes cluster'ında CNI eklentisi yüklenmelidir.
- **Ağ yapılandırması oluşturulur:** Ardından, podA ve podC'nin ağ yapılandırmaları oluşturulmalıdır. Bunun için, CNI tarafından sağlanan konfigürasyon dosyası kullanılabilir.
- **Podlar oluşturulur:** Son olarak, podA ve podC oluşturulmalıdır. Bu podlar, CNI tarafından oluşturulan ağ yapılandırmaları üzerinde çalışacaklardır.

Böylece podA ve podC arasındaki iletişim, CNI tarafından sağlanan ağ arabirimleri sayesinde gerçekleştirilebilir.

<br>

### Çözüm

CNI, Kubernetes üzerinde network pluginleri kullanarak network bağlantısını yapılandırır. Podlar arasındaki iletişimi sağlamak için kullanılan CNI pluginlerinden biri olan Multus ile birden fazla ağ arabirimi tanımlanabilir ve podlar bu arabirimler aracılığıyla iletişim kurabilir.

Multus, Kubernetes CNI arabirimini kullanan bir ağ pluginidir. Multus'un konfigürasyon dosyası Multus CNI CRD (Custom Resource Definition) olarak tanımlanır ve Kubernetes API'sinde saklanır. Örnek bir Multus CNI konfigürasyon dosyası şöyle görünebilir:

```yaml
apiVersion: 'k8s.cni.cncf.io/v1'
kind: NetworkAttachmentDefinition
metadata:
  name: example-multus-network
  annotations:
    k8s.v1.cni.cncf.io/resourceName: my-net
spec:
  config: '{
    "cniVersion": "0.3.1",
    "type": "bridge",
    "bridge": "mybridge",
    "ipam": {
    "type": "host-local",
    "subnet": "192.168.0.0/16",
    "routes": [
    { "dst": "0.0.0.0/0" }
    ]
    }
    }'
```
