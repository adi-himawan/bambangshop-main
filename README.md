# BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [V] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [V] Commit: `Create Subscriber model struct.`
    -   [V] Commit: `Create Notification model struct.`
    -   [V] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [V] Commit: `Implement add function in Subscriber repository.`
    -   [V] Commit: `Implement list_all function in Subscriber repository.`
    -   [V] Commit: `Implement delete function in Subscriber repository.`
    -   [V] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [V] Commit: `Create Notification service struct skeleton.`
    -   [V] Commit: `Implement subscribe function in Notification service.`
    -   [V] Commit: `Implement subscribe function in Notification controller.`
    -   [V] Commit: `Implement unsubscribe function in Notification service.`
    -   [V] Commit: `Implement unsubscribe function in Notification controller.`
    -   [V] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [V] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [V] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [V] Commit: `Implement publish function in Program service and Program controller.`
    -   [V] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [V] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
#### 1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?
Umumnya, penggunaan trait diperlukan jika kita memiliki beberapa jenis Observer yang mungkin memiliki behavior yang berbeda-beda. Dalam aplikasi BambangShop, kita hanya memiliki satu jenis Observer, yakni Subscriber. Oleh karena itu, dalam kasus ini, struktur single Model sudah sangat cukup untuk merepresentasikan Subscriber.

#### 2. id in Product and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?
Dalam kasus ini, penggunaan DashMap jauh lebih sesuai dibanding penggunaan Vec. DashMap memungkinkan penggunaan id yang unik sebagai key, yang dapat membantu mempercepat proses searching, insertion, dan deletion. Jika kita mengganti DashMap dengan Vec, kita akan perlu lebih dari satu Vec, yang bisa saja mempersulit proses pengolahan data.

#### 3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?
Dalam aplikasi berbasis multithreaded, pemilihan struktur data yang thread-safe sangat penting untuk menghindari race condition. Dalam kasus BambangShop, DashMap merupakan pilihan yang lebih tepat dibanding HashMap karena telah dirancang khusus sebagai 'HashMap' yang mampu me-handle concurrency. 

Selanjutnya, secara definisi, penerapan Singleton pattern melalui lazy_static memastikan hanya ada satu instance untuk menyimpan daftar Subscriber. Hal ini dilakukan untuk memastikan setiap Subscriber diakses dari satu sumber yang sama. Oleh karena itu, sebagai kesimpulan, penggunaan DashMap yang dibarengi dengan penerapan Singleton pattern dapat menjadi salah satu solusi untuk diterapkan pada aplikasi berbasis multithreaded.

#### Reflection Publisher-2
#### 1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?
Dalam aplikasi BambangShop, Service berfungsi untuk mengelola logika bisnis dan Repository berfungsi untuk mengakses serta mengolah data di database. Berdasarkan Single Responsibility Principle (SRP), Model perlu dipisah menjadi Service dan Repository untuk membantu meningkatkan code maintainability secara keseluruhan.

#### 2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?
Hanya menggunakan Model dan tidak membaginya menjadi Service dan Repository dapat membuat kode program kita menjadi tightly coupled. Akibatnya, perubahan pada salah satu model dapat berpengaruh terhadap model-model lainnya. Hal ini membuat kode kita menjadi lebih sulit untuk di-maintain.

#### 3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.
Postman adalah alat pengujian API yang memungkinkan pengguna untuk melakukan berbagai jenis HTTP request. Menurut saya, hal ini sangat membantu proses pengembangan aplikasi karena memungkinkan developer untuk memeriksa apakah HTTP response yang diterima sudah sesuai dengan rancangan di awal. Tak hanya itu, Postman juga memungkinkan developer untuk memeriksa apakah data yang diterima setelah operasi CRUD sudah benar atau belum.  

#### Reflection Publisher-3
#### 1. Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?
Pada aplikasi BambangShop, kita menerapkan Push model untuk sistem notifikasi. Hal ini dapat terlihat pada mekanisme yang diterapkan, di mana NotificationService secara otomatis mengirimkan notifikasi kepada semua Subscriber setiap kali ada perubahan pada Product. Mekanisme ini membuat Subscriber dapat selalu menerima notifikasi tanpa harus membuat request secara terpisah.

#### 2. What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull).
Jika dibandingkan dengan Push model, penggunaan Pull model memang memberi keleluasaan bagi Subscriber dalam mengakses informasi. Namun, di sisi lain, penggunaan Pull model membuat Subscriber terus-menerus memeriksa update dari Publisher. Akibatnya, penggunaan model ini dapat menyebabkan potensi terjadinya peningkatan network traffic.

#### 3. Explain what will happen to the program if we decide to not use multithreading in the notification process.
Jika kita tidak menggunakan multithreading dalam sistem notifikasi, seluruh pengiriman notifikasi akan terjadi secara synchronous dan berurutan. Akibatnya, pengiriman notifikasi ke setiap Subscriber akan dilakukan satu persatu. Jika Subscriber berada dalam jumlah yang besar, hal ini dapat menurunkan kinerja aplikasi secara keseluruhan. Oleh karena itu, dalam kasus ini, penggunaan multithreading sangat diperlukan untuk mengatasi potensi bottleneck dalam pengiriman notifikasi.