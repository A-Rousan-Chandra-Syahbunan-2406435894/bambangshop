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
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Subscriber model struct.`
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [x] Commit: `Implement add function in Subscriber repository.`
    -   [x] Commit: `Implement list_all function in Subscriber repository.`
    -   [x] Commit: `Implement delete function in Subscriber repository.`
    -   [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [X] Commit: `Create Notification service struct skeleton.`
    -   [X] Commit: `Implement subscribe function in Notification service.`
    -   [X] Commit: `Implement subscribe function in Notification controller.`
    -   [X] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. Dalam kasus ini, sebuah Model Struct sudah cukup karena semua subscriber kita saat ini memiliki perilaku yang sama, yaitu menerima notifikasi via URL (webhook). Kecuali jika kita ingin subscriber yang bermacam-macam (misal ada yang lewat Email, SMS, dsb), barulah kita butuh Trait.

2. Menggunakan DashMap jauh lebih baik karena kita butuh keunikan (berdasarkan URL). DashMap mempermudah proses pencarian dan penghapusan subscriber secara instan (O(1)), sedangkan Vec akan butuh waktu lebih lama (O(n)) karena harus mengecek satu-satu untuk memastikan tidak ada duplikasi.

3. Kita sebenarnya sudah menggunakan pola Singleton (melalui lazy_static!). Namun, Rust sangat ketat soal keamanan data antar thread. DashMap tetap dibutuhkan karena ia memberikan keamanan (thread-safe) saat beberapa proses mencoba membaca/menulis ke database secara bersamaan tanpa menyebabkan error atau data corrupt.

#### Reflection Publisher-2

1. Memisahkan Service dan Repository dari Model itu penting buat diterapin biar kode kita ngikutin prinsip Single Responsibility. Jadi pembagian tugasnya jelas: Model cuma ngurusin bentuk data, Repository fokus ke urusan database (simpan/ambil data), dan Service yang jalanin logika bisnisnya. Kalau dipisah begini, kodenya jauh lebih rapi, gampang di-test, dan kalau misal nanti mau ganti jenis database, kita cuma perlu ngubah kode di bagian Repository aja tanpa harus ngerombak logika bisnis di Service.

2. Kalau semuanya cuma digabung di dalam Model (biasa disebut Fat Model), kodenya bakal jadi sangat bengkak dan berantakan karena harus ngurusin struktur data, koneksi database, sampai logika bisnis sekaligus. Ketergantungan (coupling) antar komponen juga jadi terlalu tinggi. Efek buruknya, kalau kita mau ngubah cara kerja Notifikasi, bisa-bisa malah bikin error di bagian penyimpanan Subscriber karena semuanya numpuk di satu file. Intinya kodenya bakal susah banget dibaca dan di-maintain seiring berjalannya waktu.

3. Postman ngebantu banget buat ngetes API dengan cepet karena kita nggak perlu repot-repot bikin front-end dulu buat ngecek fungsi yang dibuat. Fitur yang paling kerasa gunanya itu Collections buat ngerapiin request per folder biar gampang dicari, sama fitur Body Request (JSON) yang bikin praktis banget pas mau ngirim data kompleks buat ngetes method POST. Selain itu, ngetes juga jadi lebih cepet karena ada History buat ngecek ulang request yang udah pernah dicanlankan sebelumnya.

#### Reflection Publisher-3
