# Tugas 7

## Apa perbedaan utama antara stateless dan stateful widget dalam konteks pengembangan aplikasi Flutter?
+ Stateless Widget
    + Bersifat statis, artinya tidak akan pernah dirubah setelah dibuat.
    + Perubahan pada Stateless Widget dipengaruhi oleh peristiwa eksternal pada widget induk dalam pohon widget.
    + Stateless Widget mengendalikan cara mereka dibangun (render) oleh widget induk yang mereka masuki.
    + Widget anak Stateless Widget menerima deskripsinya dari widget induk dan tidak mengubahnya sendiri.
    + Stateless Widget hanya memiliki properti akhir (final) yang didefinisikan selama konstruksi, dan itulah satu-satunya yang perlu dibangun di layar perangkat.
+ Stateful Widget
    + Stateful Widget dapat mengubah deskripsi (state) secara dinamis selama hidupnya.
    + Meskipun widget Stateful itu sendiri bersifat tidak berubah (immutable), mereka memiliki kelas State yang terkait yang mewakili keadaan saat ini dari widget tersebut. State inilah yang dapat berubah dan mempengaruhi tampilan widget.
    + Stateful Widget digunakan ketika perlu melacak dan mengubah data atau keadaan selama siklus hidup widget, seperti mengganti teks dalam sebuah teks input atau mengubah tampilan widget berdasarkan tindakan pengguna.

## Sebutkan seluruh widget yang kamu gunakan untuk menyelesaikan tugas ini dan jelaskan fungsinya masing-masing.

+ `MyHomePage` (StatelessWidget): Ini adalah widget utama yang mewakili halaman beranda aplikasi. Ini adalah tampilan utama yang berisi daftar item toko.
+ `Scaffold`: Ini adalah widget yang menyediakan kerangka kerja dasar untuk halaman. Ini berisi AppBar dan body.
+ `AppBar`: Widget ini digunakan untuk menampilkan bilah aplikasi di bagian atas halaman dengan judul "Minechesty".
+ `Text`: Digunakan untuk menampilkan teks "Minechesty" dalam judul AppBar.
+ `ScaffoldMessenger`: Digunakan untuk menampilkan pesan snack bar saat salah satu item di toko ditekan.
+ `SnackBar`: Ini adalah pesan pop-up sementara yang muncul ketika salah satu item di toko ditekan.
+ `SingleChildScrollView`: Widget ini digunakan untuk membungkus seluruh konten halaman sehingga dapat discroll jika kontennya lebih panjang dari layar.
+ `Padding`: Digunakan untuk memberikan padding ke dalam konten yang berisi judul dan daftar item toko.
+ `Column`: Ini adalah widget yang digunakan untuk menampilkan anak-anak (children) secara vertikal.
+ `GridView.count`: Ini adalah widget yang digunakan untuk membuat tata letak grid dengan jumlah kolom yang diberikan.
+ `ShopItem`: Ini adalah kelas yang digunakan untuk mewakili item toko. Masing-masing item memiliki nama, ikon, dan warna.
+ `ShopCard`: Ini adalah widget yang digunakan untuk menampilkan setiap item toko dalam bentuk kartu. Widget ini berisi ikon dan teks, serta merespons ketika ditekan untuk menampilkan snack bar.
+ `Material`: Ini adalah widget yang digunakan untuk mengatur latar belakang warna kartu.
+ `InkWell`: Digunakan untuk memberikan area responsif terhadap sentuhan sehingga item toko bisa ditekan.
+ `Container`: Ini adalah widget yang digunakan untuk mengelompokkan ikon dan teks dalam kartu.
+ `Icon`: Digunakan untuk menampilkan ikon yang terkait dengan item toko.
+ `Text`: Digunakan untuk menampilkan nama item toko dalam kartu.

## Jelaskan bagaimana cara kamu mengimplementasikan checklist di atas secara step-by-step (bukan hanya sekadar mengikuti tutorial)
- [x] Membuat sebuah program Flutter baru dengan tema inventory seperti tugas-tugas sebelumnya. <br>

  + Buka Command Prompt atau terminal pada folder tempat proyek flutter akan dibuat.
  + Buatlah proyek minechesty dengan menggunakan command berikut
  ```
  flutter create minechesty
  cd minechesty
  ```

  + Membuat file `menu.dart` lalu menambahkan import:
  ```
  import 'package:flutter/material.dart';
  ```

  + Pada file `main.dart` pindahkan kode baris ke-39 hingga akhir dan ubahlah menjadi seperti berikut ke file `menu.dart` yang baru saja dibuat serta ubah `({super.key, required this.title})` menjadi `({Key? key}) : super(key: key);` serta tambahkan juga Widget build seperti dibawah ini.
  ```
  class MyHomePage extends StatelessWidget {
      MyHomePage({Key? key}) : super(key: key);

      @override
      Widget build(BuildContext context) {
          return Scaffold(
              ...
          );
      }

  }
  ```
  + Pada file `main.dart` ubah `home: const MyHomePage(title: 'Flutter Demo Home Page'),` menjadi `home: MyHomePage(),`

  + Tambahkan juga kode berikut pada file `main.dart` untuk mengenali class MyHomePage pada file `menu.dart`:
  ```
  import 'package:shopping_list/menu.dart';
  ```

  + Pertama-tama membuat define tipe dengan dibawah ini:

  ```
  class ShopItem {
    final String name;
    final IconData icon;

    ShopItem(this.name, this.icon);
  }
  ```

  + Selanjutnya tambahkan kode berikut didalam Widget build:
  ```
        return Scaffold(
              appBar: AppBar(
                title: const Text(
                  'Minechesty',
                ),
                backgroundColor: Theme.of(context).colorScheme.primary,
              ),
              body: SingleChildScrollView(
                // Widget wrapper yang dapat discroll
                child: Padding(
                  padding: const EdgeInsets.all(10.0), // Set padding dari halaman
                  child: Column(
                    // Widget untuk menampilkan children secara vertikal
                    children: <Widget>[
                      const Padding(
                        padding: EdgeInsets.only(top: 10.0, bottom: 10.0),
                        // Widget Text untuk menampilkan tulisan dengan alignment center dan style yang sesuai
                        child: Text(
                          'Minechesty', // Text yang menandakan toko
                          textAlign: TextAlign.center,
                          style: TextStyle(
                            fontSize: 30,
                            fontWeight: FontWeight.bold,
                          ),
                        ),
                      ),
                      // Grid layout
                      GridView.count(
                        // Container pada card kita.
                        primary: true,
                        padding: const EdgeInsets.all(20),
                        crossAxisSpacing: 10,
                        mainAxisSpacing: 10,
                        crossAxisCount: 3,
                        shrinkWrap: true,
                        children: items.map((ShopItem item) {
                          // Iterasi untuk setiap item
                          return ShopCard(item);
                        }).toList(),
                      ),
                    ],
                  ),
                ),
              ),
            );
  ```

  + Tambahkan juga widget stateless baru untuk menampilkan card:
  ```
  class ShopCard extends StatelessWidget {
    final ShopItem item;

    const ShopCard(this.item, {super.key}); // Constructor

    @override
    Widget build(BuildContext context) {
      return Material(
        color: item.color,
        child: InkWell(
          // Area responsive terhadap sentuhan
          onTap: () {
            // Memunculkan SnackBar ketika diklik
            ScaffoldMessenger.of(context)
              ..hideCurrentSnackBar()
              ..showSnackBar(SnackBar(
                  content: Text("Kamu telah menekan tombol ${item.name}!")));
          },
          child: Container(
            // Container untuk menyimpan Icon dan Text
            padding: const EdgeInsets.all(8),
            child: Center(
              child: Column(
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  Icon(
                    item.icon,
                    color: Colors.white,
                    size: 30.0,
                  ),
                  const Padding(padding: EdgeInsets.all(3)),
                  Text(
                    item.name,
                    textAlign: TextAlign.center,
                    style: const TextStyle(color: Colors.white),
                  ),
                ],
              ),
            ),
          ),
        ),
      );
    }
  }
  ```

- [x] Membuat tiga tombol sederhana dengan ikon dan teks untuk: <br>
  - [x] Melihat daftar item (`Lihat Item`) <br>
  - [x] Menambah item (`Tambah Item`) <br>
  - [x] Logout (`Logout`) <br>
    
      + Tambahkan kode berikut dibawah kode `MyHomePage({Key? key}) : super(key: key);`:
      ```
          final List<ShopItem> items = [
          ShopItem("Lihat Item", Icons.checklist, Colors.red),
          ShopItem("Tambah Item", Icons.add_shopping_cart, Colors.blue),
          ShopItem("Logout", Icons.logout, Colors.green),
          ];
      ```

- [x] Memunculkan `Snackbar` dengan tulisan: <br>
  - [x] "Kamu telah menekan tombol Lihat Item" ketika tombol `Lihat Item` ditekan. <br>
  - [x] "Kamu telah menekan tombol Tambah Item" ketika tombol `Tambah Item` ditekan. <br>
  - [x] "Kamu telah menekan tombol Logout" ketika tombol `Logout` ditekan. <br>

      + Pada bagian widget build terdapat kode dibawah ini yang dimana pada saat button di tap maka akan showSnackBar dengan pesan "Kamu telah menekan tombol sesuatu" dimana sesuatu sudah disesuaikan dengan context yang diinginkan:

      ```
              onTap: () {
            // Memunculkan SnackBar ketika diklik
            ScaffoldMessenger.of(context)
              ..hideCurrentSnackBar()
              ..showSnackBar(SnackBar(
                  content: Text("Kamu telah menekan tombol ${item.name}!")));
          },
      ```
