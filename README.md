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
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
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
1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough? <br>
    Pada kasus BambangShop, Observer atau Subscriber hanya memiliki 1 tipe atau jenis. Hal ini memungkinkan kita untuk tidak perlu menggunakan interface atau trait pada rust untuk menerapkan Observer design patterns.

2. id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case? <br>
    Pada kasus ini, kita mungkin saja menggunakan Vec/list untuk menyimpan Product atau Subscriber. Namun, dikarenakan id dari Product dan url dari Subscriber unik, kita dapat memanfaatkan map lebih efisien dimana kita dapat mengambil data secara constant, dimana jika dibandingkan Vec/list yang linear, akan jauh lebih cepat.

3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead? <br>
    Pada kasus ini, kita menggunakan pattern Singleton untuk memastikan hanya terdapat 1 instance database yang ada untuk tiap jenis model. Namun, masalah akan terjadi jika kita tidak menggunakan Thread Safe library seperti DashMap, dimana pada pattern Singleton, instance dari 1 objek tersebut dapat digunakan secara bersamaan oleh beberapa thread, yang dimana akan menyebabkan terjadinya race condition. Untuk itu, kita menggunakan Thread Safe library seperti DashMap.  

#### Reflection Publisher-2
1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model? <br>
    Dalam model MVC, kita perlu memisahkan Service dan Repository dari Model untuk memenuhi salah satu prinsip dari SOLID Principle yaitu SRP atau Single Responsibility Principle yang dimana Repository berfungsi untuk menyimpan data dalam basis data kedalam basis data, sementara itu service berfungsi sebagai business logic.

2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model? <br>
    Jika kita hanya menggunakan model tanpa memisahnya menjadi Service dan Repository, maka kode akan menjadi kurang maintainable. Hal ini dikarenakan kelas model akan menjadi sangat panjang dan akan ada keterikatan kuat antara beberapa model. Hal ini membuat code dari tiap model menjadi lebih complex.

3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects. <br>
    Ya. Menurut saya Postman merupakan tool yang sangat berguna untuk melakukan http request terutama request dengan tipe post, put, atau delete. Hal ini dikarenakan kita dapat set metode http request sesuai dengan yang diinginkan dimana dengan browser, kita sulit untuk melakukan http request dengan tipe-tipe tersebut. Selain itu, Postman juga memudahkan kita untuk melihat http response yang dikirimkan oleh server.

#### Reflection Publisher-3
1. Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use? <br>
    Dalam tutorial ini, kita menggunakan Push model, dimana ketika suatu product ditambahkan atau dihapus, maka product service akan memanggil menggunakan notification service untuk mengirimkan notifikasi ke tiap subscriber yang ada 

2. What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull) <br>
    Jika kita menggunakan Pull model, maka subscriber harus meminta notifikasi pada publisher. Kelebihan dari Push model adalah subscriber tidak perlu meminta request pada publisher untuk mendapatkan notifikasi product yang telah dibuat/dihapus. Kelebihan dari Pull method adalah publisher tidak perlu mengirim notifikasi ke tiap subscriber ketika ada product yang telah dibuat/dihapus sehingga akan lebih efisien.

3. Explain what will happen to the program if we decide to not use multi-threading in the notification process. <br>
    jika kita tidak menggunakan multi-threading pada proses notifikasi, maka server akan mengirimkan notifikasi satu per satu untuk tiap subscriber. Hal ini sangat tidak efisien dan dapat menghambat jalannya server.
