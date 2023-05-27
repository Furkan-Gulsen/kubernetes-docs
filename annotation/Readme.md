# Annotation

Annotation'lar, Kubernetes ortamında kaynaklara (örneğin Pod'lar, Servisler, Deployment'lar vb.) ilişkili olmayan metaveriler eklemek için kullanılan key-value (anahtar-değer) çiftleridir. Annotation'lar, Label'lar gibi kaynaklara metaveri eklemek için kullanılır, ancak Label'ların aksine kaynakların gruplandırılması veya seçilmesinde kullanılmazlar. Annotation'lar, kaynaklara ilgili ek bilgileri eklemek için kullanılır ve genellikle açıklama, sürüm numarası, oluşturma tarihi gibi bilgileri içerir.

## Annotation'ların Avantajları

- **Ek Bilgi Saklama**: Annotation'lar, kaynaklarla ilgili ek bilgileri saklamak için kullanılır. Bu, kaynakların anlaşılmasını ve yönetilmesini kolaylaştırır. Örneğin, bir Pod'un neden oluşturulduğu veya ne işe yaradığı gibi açıklamaları Annotation'lar aracılığıyla ekleyebilirsiniz.

- **Entegrasyon ve İş Akışı**: Annotation'lar, farklı araçlar, hizmetler veya otomasyon iş akışlarıyla entegrasyon sağlamak için kullanılabilir. Örneğin, CI/CD araçları, Annotation'ları okuyarak ve onlara göre işlem yaparak süreçlerinizi otomatikleştirebilir.

- **Üçüncü Taraf Araçlar ve Uzantılar**: Annotation'lar, Kubernetes ekosistemindeki üçüncü taraf araçlar veya uzantılar tarafından kullanılmak için kullanılabilir. Bu, farklı hizmetleri veya özellikleri etkinleştirmek veya yapılandırmak için kullanılabilir.

## Annotation'ların Kullanımı ve Yönetimi

Annotation'lar, kaynak tanımlama dosyalarında veya Kubernetes API'si aracılığıyla tanımlanır. Annotation'lar, kaynak oluşturulurken veya daha sonra kaynağa eklenebilir. Ayrıca, Annotation'lar güncellenebilir ve silinebilir.

Kubernetes, Annotation'ları doğrudan kullanmasa da, üçüncü taraf araçlar veya uzantılar tarafından kullanılarak çeşitli işlemler gerçekleştirilebilir. Örneğin, bir izleme aracı, belirli Annotation'lara sahip olan kaynakları izleyebilir veya bir otomasyon aracı, Annotation'ları kullanarak belirli işlemler gerçekleştirebilir.
