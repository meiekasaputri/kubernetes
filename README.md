# KUBERNETES

# Komponen-Komponen Kubernetes

1. **Komponen Master**

     Komponen master menyediakan control plane bagi klaster. Komponen ini berperan dalam proses pengambilan secara global pada klaster, serta berperan dalam proses deteksi serta pemberian respons terhadap events yang berlangsung di dalam klaster.

     Komponen master dapat dijalankan di mesin manapun yang ada di klaster. Meski begitu, untuk memudahkan proses yang ada, script inisiasi awal yang dijalankan biasanya memulai komponen master pada mesin yang sama, serta tidak menjalankan kontainer bagi pengguna di mesin ini.

     **kube-apiserver**

     Komponen di master yang mengekspos API Kubernetes. Merupakan front-end dari kontrol plane Kubernetes.
     Komponen ini didesain agar dapat di-scale secara horizontal. Lihat Membangun Klaster HA.

     **etcd**

     Penyimpanan key value konsisten yang digunakan sebagai penyimpanan data klaster Kubernetes.

     **kube-scheduler**

     Komponen di master yang bertugas mengamati pod yang baru dibuat dan belum di-assign ke suatu node dan kemudian akan memilih sebuah node dimana pod baru tersebut akan dijalankan.
     Faktor-faktor yang diperhatikan dalam proses ini adalah kebutuhan resource secara individual dan kolektif, konstrain perangkat keras/perangkat lunak/peraturan, spesifikasi afinitas dan non-afinitas, lokalisasi data, interferensi inter-workload dan deadlines.

     **kube-controller-manager**

     Komponen di master yang menjalankan kontroler.
     Secara logis, setiap kontroler adalah sebuah proses yang berbeda, tetapi untuk mengurangi kompleksitas, kontroler-kontroler ini dikompilasi menjadi sebuah binary yang dijalankan sebagai satu proses. Kontroler-kontroler ini meliputi:

   - Kontroler Node : Bertanggung jawab untuk mengamati dan memberikan respons apabila jumlah node berkurang.
   - Kontroler Replikasi : Bertanggung jawab untuk menjaga jumlah pod agar jumlahnya sesuai dengan kebutuhan setiap
      objek kontroler replikasi yang ada di sistem.
   - Kontroler Endpoints : Menginisiasi objek Endpoints (yang merupakan gabungan Pods dan Services).
   - Kontroler Service Account & Token: Membuat akun dan akses token API standar untuk setiap namespaces yang dibuat.

     **cloud-controller-manager**

     Cloud-controller-manager merupakan kontroler yang berinteraksi dengan penyedia layanan cloud. Kontroler ini merupakan fitur alfa yang diperkenalkan pada Kubernetes versi 1.6. Cloud-controller-manager hanya menjalankan iterasi kontroler cloud-provider-specific.

     Adanya cloud-controller-manager memungkinkan kode yang dimiliki oleh penyedia layanan cloud dan kode yang ada pada Kubernetes saling tidak bergantung selama masa development. Pada versi sebelumnya, Kubernetes bergantung pada fungsionalitas spesifik yang disediakan oleh penyedia layanan cloud. Di masa mendatang, kode yang secara spesifik dimiliki oleh penyedia layanan cloud akan dipelihara oleh penyedia layanan cloud itu sendiri, kode ini selanjutnya akan dihubungkan dengan cloud-controller-manager ketika Kubernetes dijalankan.

     Kontroler berikut ini memiliki keterkaitan dengan penyedia layanan cloud:

     - Kontroler Node : Melakukan pengecekan pada penyedia layanan cloud ketika menentukan apakah sebuah node telah
       dihapus pada cloud apabila node tersebut berhenti memberikan respons.

     - Kontroler Route : Melakukan pengaturan awal route yang ada pada penyedia layanan cloud.

     - Kontroler Service : Untuk membuat, memperbaharui, menghapus load balancer yang disediakan oleh penyedia layanan
       cloud.

     - Kontroler Volume : Untuk membuat, meng-attach, dan melakukan mount volume serta melakukan inetraksi dengan
       penyedia layanan cloud untuk melakukan orkestrasi volume.

2. **Komponen Node**

   Komponen ini ada pada setiap node, fungsinya adalah melakukan pemeliharaan terhadap pod serta menyediakan environment runtime bagi Kubernetes.

   **kubelet**

   Agen yang dijalankan pada setiap node di klaster dan bertugas memastikan kontainer dijalankan di dalam pod.

   **kube-proxy**

   kube-proxy membantu abstraksi service Kubernetes melakukan tugasnya. Hal ini terjadi dengan cara memelihara aturan-aturan jaringan (network rules) serta meneruskan koneksi yang ditujukan pada suatu host.

   **Container Runtime**

   Container runtime adalah perangkat lunak yang bertanggung jawab dalam menjalankan kontainer. Kubernetes mendukung beberapa runtime, diantaranya adalah: Docker, containerd, cri-o, rktlet dan semua implementasi Kubernetes CRI (Container Runtime Interface).

3. **Addons**

   Addons merupakan pod dan service yang mengimplementasikan fitur-fitur yang diperlukan klaster.

   **DNS**

   Meskipun tidak semua addons dibutuhkan, semua klaster Kubernetes hendaknya memiliki DNS klaster. Komponen ini penting karena banyak dibutuhkan oleh komponen lainnya.

   Klaster DNS adalah server DNS, selain beberapa server DNS lain yang sudah ada di environment kamu, yang berfungsi sebagai catatan DNS bagi Kubernetes services

   Kontainer yang dimulai oleh kubernetes secara otomatis akan memasukkan server DNS ini ke dalam mekanisme pencarian DNS yang dimilikinya.

   **Web UI (Dasbor)**

   Dasbor adalah antar muka berbasis web multifungsi yang ada pada klaster Kubernetes. Dasbor ini memungkinkan user melakukan manajemen dan troubleshooting klaster maupun aplikasi yang ada pada klaster itu sendiri.

   **Container Resource Monitoring**

   Container Resource Monitoring mencatat metrik time-series yang diperoleh dari kontainer ke dalam basis data serta menyediakan antar muka yang dapat digunakan untuk melakukan pencarian data yang dibutuhkan.

   **Cluster-level Logging**

   Cluster-level logging bertanggung jawab mencatat log kontainer pada penyimpanan log terpusat dengan antar muka yang dapat digunakan untuk melakukan pencarian.


# Membuat Deployment dengan Python+Flask

Kubernetes Pod adalah grup yang terdiri dari satu atau lebih Container, yang saling terhubung untuk keperluan administrasi dan jaringan. Pod dalam praktik ini hanya memiliki satu Container. Deployment Kubernetes memeriksa kesehatan Pod dan memulai kembali (restart) Container Pod jika sudah berakhir atau mati. Deployment adalah cara yang disarankan untuk mengelola pembuatan dan replikasi Pod.

Dalam praktik ini saya menggunakan terminal yang memiliki fitur minikube di URL berikut :
https://kubernetes.io/docs/tutorials/hello-minikube/

1. `kubectl create` perintah ini digunakan untuk membuat sebuah Deployment untuk memanage atau mengatur sebuah Pod.
    Dimana Pod menjalankan Container berdasarkan pada images Docker yang disediakan.
    Yang mana `python-flask:v2` adalah images Docker dan `050612` adalah akun DockerHub saya.
    Sebelumnya images Docker `python-flask:v2` ini dibuat dan dipush ke DockerHub pada saat praktikum tcc minggu-08.

    ![](uas/1.png)

2. `kubectl get deployments` perintah ini digunakan untuk melihat Deployment yang sudah berhasil dibuat,
   dan hasilnya seperti berikut :

   ![](uas/2.png)

3. `kubectl get pods` perintah ini digunakan untuk melihat Pod yang sudah berhasil dibuat, dan hasilnya seperti berikut:

   ![](uas/3.png)

4. `kubectl get events` perintah ini digunakan untuk melihat event yang terjadi pada cluster,
   dan hasilnya seperti berikut :

   ![](uas/4.png)

5. `kubectl config view` perintah ini digunakan untuk melihat konfigurasi kubectl, dan hasilnya seperti berikut :

   ![](uas/5.png)

# Membuat Service

Mengekspos Pod sebagai Layanan Kubernetes (Service Kubernetes) digunakan untuk membuat python-flask Container dapat diakses dari luar jaringan virtual Kubernetes. Karena secara default, Pod hanya dapat diakses oleh alamat IP (IP Address) internal di dalam cluster Kubernetes.

1. `kubectl expose` perintah ini digunakan untuk mengekspos Pod ke internet public,
    sedangkan flags `--type=LoadBalancer` menunjukkan bahwa saya ingin mengekspos Layanan (Service) saya di luar cluster.

    ![](uas/6.png)

2. `kubectl get services` perintah ini digunakan untuk melihat Layanan (Service) yang baru saja saya buat.

   ![](uas/7.png)

3. Pada penyedia cloud yang mendukung penyeimbang beban (LoadBalancer),
   alamat IP (IP Address) eksternal akan disediakan untuk mengakses Layanan (Service). Di Minikube, tipe LoadBalancer membuat Layanan (Service) dapat diakses melalui perintah `minikube service`.

   ![](uas/8.png)

4. `minikube service python-flask` pada perintah ini terlihat bahwa untuk mengakses pada browser menggunakan port 31868.
    Maka ubah port pada Katacoda yang sebelumnya 30000 menjadi port 31868.

    ![](uas/9.png)

5.  Setalah mengubah port yang sesuai kemudian klik Display Port pada Katacoda,
    maka akan muncul isi dari images docker `python-flask:v2`

    ![](uas/10.png)
