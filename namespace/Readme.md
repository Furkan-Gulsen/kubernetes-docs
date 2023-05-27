# Namespace

Kubernetes'de Namespaces, bir Kubernetes kümesi içinde kaynakların mantıksal olarak izole edilmesini sağlayan bir kavramdır. Bir Namespace, birçok Kubernetes nesnesini (örneğin Pod'ları, Service'leri, Deployment'ları) gruplandırmak için kullanılır ve bu nesnelerin benzersiz bir ad alanında çalışmasını sağlar.

Namespaces, farklı takımlar, projeler veya ortamlar arasında kaynak çakışmasını önlemek için kullanılabilir. Örneğin, bir firma içindeki farklı ekipler kendi Namespaces'lerinde çalışabilir ve birbirlerinin kaynaklarını etkilemeden kendi uygulamalarını yönetebilirler. Aynı şekilde, farklı ortamlar (örneğin, geliştirme, üretim) için farklı Namespaces'ler oluşturarak bu ortamlar arasında izolasyon sağlanabilir.

Namespaces, varsayılan olarak "default" adında bir Namespace içerir. Ancak, ihtiyaçlarınıza göre kendi özel Namespaces'lerinizi oluşturabilirsiniz. Bir Namespace oluşturduğunuzda, o Namespace içindeki kaynaklar, diğer Namespaces'lerin kaynaklarından izole edilir. Bu, kaynakların birbirleriyle çakışmadan çalışmasını sağlar.

Namespaces aynı zamanda erişim kontrolünü kolaylaştırır. Örneğin, farklı ekipler arasında roller ve izinler atanabilir ve bu ekipler sadece kendi Namespaces'leri üzerinde çalışabilir. Bu şekilde, güvenlik artırılır ve bir ekip diğer ekiplerin kaynaklarına erişemez.

## Uygulama

Bir e-ticaret platformu üzerinde çalışan bir Kubernetes kümesini ele alalım. Bu platformda, hem kullanıcı arayüzü hem de arka plandaki hizmetler bulunmaktadır. Projenizde, farklı ekiplerin bu platform üzerinde çalışacağını ve her bir ekibin kendi kaynaklarını izole etmek istediğini varsayalım.

- Öncelikle, farklı ekiplerin projelerini izole edebilmeleri için her bir ekip için ayrı bir Namespace oluşturun. Örneğin, "frontend-team" ve "backend-team" gibi isimler kullanabilirsiniz.

- "frontend-team" Namespace'i içinde, kullanıcı arayüzüne ait olan Pod'ları, Service'leri ve Deployment'ları gruplandırın. Bu, frontend ekibinin kendi kaynaklarını yönetebilmesini ve diğer ekiplerin kaynaklarından etkilenmemesini sağlar.

- Benzer şekilde, "backend-team" Namespace'i içinde, arka plandaki hizmetlere ait olan Pod'ları, Service'leri ve Deployment'ları gruplandırın. Bu, backend ekibinin kendi kaynaklarını yönetebilmesini ve diğer ekiplerin kaynaklarından etkilenmemesini sağlar.

- Her bir Namespace için uygun rol ve izinler tanımlayın. Örneğin, "frontend-team" için sadece frontend ekibine ait kullanıcıların erişebileceği bir rol tanımlayabilirsiniz. Benzer şekilde, "backend-team" için sadece backend ekibine ait kullanıcıların erişebileceği bir rol tanımlayabilirsiniz. Bu şekilde, her bir ekip kendi Namespace'i içinde çalışabilir ve diğer ekiplerin kaynaklarına erişimi kısıtlanmış olur. İzolasyon ve güvenlik sağlanmış olur.

- Her bir Namespace içindeki kaynaklar, kendi Namespace'i içindeki diğer kaynaklarla ilişkilidir. Örneğin, "frontend-team" Namespace'i içindeki Pod'lar, sadece aynı Namespace içindeki Service'lere yönlendirilebilir. Bu sayede, bir ekibin kaynakları diğer ekibin kaynaklarından etkilenmez ve her bir ekip kendi Namespace'i içinde kendi kaynaklarını yönetebilir.

## Dikkat Edilmesi Gerekenler

- Mantıklı Adlandırma: Namespaces'i oluştururken, anlaşılır ve tanımlayıcı isimler kullanmaya özen gösterin. İsimlendirme, Namespaces içindeki kaynakların amacını veya sahibini anlatmalıdır. Böylece, başkaları Namespace'in içeriğini daha kolay anlayabilir.

- İzolasyonu Sağlamak: Namespaces, kaynakları birbirinden izole etmek için kullanılır. Farklı Namespaces'ler arasında kaynak çakışmasını önlemek için her Namespace'e yalnızca ihtiyaç duyduğu kaynakları yerleştirin. Aşırı kaynak yüklemesi veya gereksiz bağımlılıklar, küme performansını etkileyebilir.

- Güvenlik ve Erişim Kontrolü: Namespaces, güvenlik ve erişim kontrolü sağlamak için kullanılabilir. Farklı ekipler veya kullanıcılar arasında roller ve izinler atanabilir. Her bir Namespace, kaynaklara erişim yetkilerini kısıtlayarak izolasyonu güçlendirir.

- Kaynakların Gruplandırılması: Kaynakları Namespaces içinde mantıklı bir şekilde gruplandırmak önemlidir.
