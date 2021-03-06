Kubernetes bir konteyner orkestrasyon platformudur.
Kubernetes çalışan konteynerleri yönetir. Konteyneri tek başlarına çalıştırmanıza izin verilmez.
Bunun yerine pods adı verilen yapılarda çalıştırılırlar.
Tek bir pod içerisinde çalışan birden fazla konteyneriniz olabilir.
Bu durum pod'ları kubernetin temel yapı taşı yapar.

* Kubernetes'in temel kullanım amacı :
	* Pod yaratma ve çalıştırma
	* Eş podlardan birden fazla çalıştırma
	* Podlar arası trafik yükü dengeleme yapma
	* Eski podların yerine geçecek yeni podlar yaratma
	* Podların birbirleri ile konuşabilmesi için networking yapma

Kubernetes iki temel yapısal gruba ayrılır. Worker ve Master.
Master node'lar Kubernetes platformunu ayakla tutarken Worker node'lar podların koşmasından sorumludur.
* Master node'lar:
	* etcd: Key-value data store to store current state 
	* kube-apiserver: Component to interect with kube-ctl
	* kube-scheduler: determine which port should be deployed to which worker node.
	* kube-controller-manager: Manage controller object and keep scheduler. 
* Worker components:
	* kubelet: Sends instructions to Container runtine engine
	* kube-proxy:manages networking over pods.
	* Container Runtime: Actual running of containers.

_Kube Masters control Kube Workers by instructions._

Used packages:
	kubectl: main kubernetes command line tool
	minikube: creating single node kube-cluster
	docker: minikube needs a hypervisor which is provided by docker

> Pod'lar bizim konteynerimizi çalıştırdığımız yerlerdir.
> YAML formatındaki xxxx-xxx.yml dosyalarıyla çalıştırmak istediğimiz podları belirleyebilir ve niteleyebiliriz.
> Dockerfile veya docker-compose.yml dosyasına benzerlik gösterir.

```
apiVersion: v1
kind: Pod
metadata:
  name: pod-httpd
  labels:
    app: apache_webserver
spec:
  containers:
    -name: cntr-httpd
    image: httpd:latest
    ports:
    -containerPort: 80
```

kubectl apply -f xxxx-xxx.yml > komutuyla kubectl'e dosyayı verebiliriz
`kubectl get pods` komutuyla da çalışan podları görebilriiz
Bu örnekteki pod'un ip adresini öğrenmek için "kubectl get pods -o wide|yaml"  komutuyla detaylı listeleme yapabiliriz.
						kubectl describe pods [pod-name]
Silmek içinse `kubectl delete pods [pod-name]`

Pod içerisindeki bir konteyner üzerinde bir komut çalıştırmak isteyebiliriz. 
Yardımcımız: kubectl exec [metadata:name] -c [containers:name] -- [çalıştırmak istediğimiz komut] 
	formatındaki komutumuz olacaktır. Çift - karakterini unutmayalım.

Çalışan apache sunucusunun "curl http://IP-ADDR" komutu çıktısı olmayabilir.
Stres yapmaksızın apache pod'unun service'ini kontrol etmemiz, localhost'a bağlantısı olup olmadığına göre ise 
`curl localhost:[port]` şeklinde bir komut çalıştırmamız gerekebilir.

Peki dışarıdan nasıl ulaşacağız? Bir network servis objesi oluşturmamız gerekiyor.

