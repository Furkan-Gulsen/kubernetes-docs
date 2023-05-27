# Minikube

Minikube, Kubernetes'in yerel bir sürümünü çalıştırmak ve geliştirmek için kullanılan bir araçtır. Minikube, kullanıcıların tek bir bilgisayarda Kubernetes'i keşfetmelerine, uygulamalarını test etmelerine ve yerel olarak geliştirmelerine olanak tanır. Ayrıca, birden fazla Kubernetes versiyonunu destekleyerek farklı versiyonlar arasında geçiş yapmayı da kolaylaştırır.

Minikube, tek bir düğüm (node) üzerinde çalışan basit bir Kubernetes cluster'ı oluşturur. Bu düğüm, yerel bir sanal makine (Virtual Machine - VM) veya konteyner çalıştırma ortamı (container runtime) üzerinde çalışabilir. Minikube, tüm Kubernetes bileşenlerini (master, etcd, kubelet, kube-proxy vb.) otomatik olarak yapılandırır ve başlatır.

## Özellikler

- Hızlı Kurulum ve Başlatma: Minikube, hızlı bir şekilde kurulabilen ve başlatılabilen basit bir araçtır. Kullanıcılar, tek bir komutla Minikube'ı çalıştırabilir ve hızlıca yerel bir Kubernetes cluster'ını kullanıma hazır hale getirebilir.

- Esnek Konfigürasyon Seçenekleri: Minikube, çeşitli konfigürasyon seçenekleri sunar. Bellek boyutu, CPU sayısı, Kubernetes versiyonu gibi özellikleri isteğe bağlı olarak ayarlayabilirsiniz. Bu, farklı uygulama senaryolarına uygun cluster'lar oluşturmanızı sağlar.

- Multi-Node Desteği: Minikube, tek düğüm (node) üzerinde çalışmasının yanı sıra birden fazla düğümü destekleyebilir. Böylece, birden fazla Minikube düğümünü birleştirerek daha karmaşık ve ölçeklenebilir senaryoları simüle edebilirsiniz.
