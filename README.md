# [Tugas 8 - Pertemuan 9] 

**Nama** : Yacobus Daeli

**NIM** : H1D022024

**Shift** : E


Menjelaskan proses CRUD untuk Data Mahasiswa.

# Tambah Mahasiswa

![Tambah Mahasiswa](https://github.com/user-attachments/assets/94ac05e7-81ea-4780-8b5a-b4ac5d0cc05c)


![Tambah Mahasiswa - 2](https://github.com/user-attachments/assets/b6653147-4d82-481f-8240-3d68d3d95089)


![Tambah Mahasiswa - 3](https://github.com/user-attachments/assets/908a8071-9e4a-4cdf-a9c1-9e047fd0fcbc)




Berikut adalah source code dari tambah data : 

```javascript
tambahMahasiswa() {
    if (this.nama != '' && this.jurusan != '') {
      let data = {
        nama: this.nama,
        jurusan: this.jurusan,
      };
      this.api.tambah(data, 'tambah.php').subscribe({
        next: async (hasil: any) => {
          this.resetModal();
          console.log('berhasil tambah mahasiswa');
          this.getMahasiswa();
          this.modalTambah = false;
          this.modal.dismiss();
          await this.showAlert('Data mahasiswa berhasil ditambahkan!');
        },
        error: (err: any) => {
          console.log('gagal tambah mahasiswa');
        }
      });
    } else {
      console.log('gagal tambah mahasiswa karena masih ada data yg kosong');
    }
  }
```

Penjelasan Create data
```
if (this.nama != '' && this.jurusan != '') {
      let data = {
        nama: this.nama,
        jurusan: this.jurusan,
      };

```

Untuk baris code di atas memeriksa apakah atribut nama dan jurusan telah diisi (tidak kosong). Jika salah satu kosong, fungsi akan berhenti tanpa melanjutkan ke proses penambahan data dan hanya menampilkan pesan kesalahan.

Jika kondisi ini tidak terpenuhi, fungsi akan keluar, dan kode berikutnya akan dieksekusi:

```javascript

error: (err: any) => {
          console.log('gagal tambah mahasiswa');
        }

```

Diatas adalah kondisi ketika salah satu kosong maka akan keluar dari pengkondisian diatas.

```javascript

if (this.nama != '' && this.jurusan != '') {
      let data = {
        nama: this.nama,
        jurusan: this.jurusan,
      };

```

Jika input tidak kosong, fungsi akan membuat objek data yang berisi nama dan jurusan untuk dikirim ke server.

```javascript
this.api.tambah(data, 'tambah.php').subscribe({
        next: async (hasil: any) => {
          this.resetModal();
          console.log('berhasil tambah mahasiswa');
          this.getMahasiswa();
          this.modalTambah = false;
          this.modal.dismiss();
          await this.showAlert('Data mahasiswa berhasil ditambahkan!');
        },
        error: (err: any) => {
          console.log('gagal tambah mahasiswa');
        }
      });
```


Setelah itu, data yang sudah kita input akan dikirim ke API dengan memanggil this.api.tambah(), yang menerima data dan endpoint 'tambah.php'. Fungsi ini mengembalikan sebuah Observable, memungkinkan respons dari server untuk ditangani menggunakan metode subscribe.

Setelah pengkondisian, jika pengiriman data berhasil maka fungsi next akan dijalankan : 
## if(request==true) -> Data berhasil dikirim :

Fungsi `next` akan dijalankan, yang meliputi langkah-langkah berikut:

### Penjelasan Kode (Langkah-Langkah Eksekusi)

```typescript
 next: async (hasil: any) => {
          this.resetModal();
          console.log('berhasil tambah mahasiswa');
          this.getMahasiswa();
          this.modalTambah = false;
          this.modal.dismiss();
          await this.showAlert('Data mahasiswa berhasil ditambahkan!');
        },
```

- `this.resetModal();` – Mereset input di modal agar siap digunakan kembali.
- `console.log('berhasil tambah mahasiswa');` – Menampilkan pesan sukses di konsol.
- `this.getMahasiswa();` – Memuat ulang daftar mahasiswa untuk memperbarui tampilan.
- `this.modalTambah = false;` – Menyembunyikan modal.
- `this.modal.dismiss();` – Menutup modal.
- `await this.showAlert('Data mahasiswa berhasil ditambahkan!');` – Menampilkan alert berhasil

## Menampilkan Alert data berhasil ditambahkan


![Alert Tambah Mahasiswa](https://github.com/user-attachments/assets/3bac07d1-5437-420f-bf87-d4142f386c09)


## if(request==false) -> Jika data tidak berhasil dikirim : 
maka akan menjalankan kode error exception, yaitu keluar dari pengkondisian sebelumnya dan mengeluarkan pesan error : 
```typescript
error: (err: any) => {  
          console.log('gagal tambah mahasiswa');
        }
      });
    } else {
      console.log('gagal tambah mahasiswa karena masih ada data yg kosong');
    }
  }
```
Di atas mengeluarkan 2 kondisi error, jika ketika menambahkan data mahasiswa masih ada kolom yang kosong maka akan mengirim pesan `gagal tambah mahasiswa karena masih ada data yg kosong`


# Read (View)

![Read Mahasiswa - 3](https://github.com/user-attachments/assets/4032bf43-fa07-4653-8d94-42cc6209752a)

Source code dari read data mahasiswa : 
```typescript
getMahasiswa() {
    this.api.tampil('tampil.php').subscribe({
      next: (res: any) => {
        console.log('sukses', res); // Menampilkan pesan sukses dan data yang diterima di konsol.
        this.dataMahasiswa = res; // Menyimpan data yang diterima ke dalam variabel 'dataMahasiswa'.
      },
      error: (err: any) => {
        console.log(err); // Menampilkan pesan error di konsol jika terjadi kegagalan.
      },
    });
}
```

## Penjelasan Read
Fungsi `getMahasiswa()` digunakan untuk mengambil data mahasiswa dari server dengan memanggil metode `tampil()` dari `this.api`. Data diambil dari endpoint `tampil.php` dan hasilnya ditangani menggunakan metode `subscribe()` yang mencakup dua bagian: `next` untuk respons sukses dan `error` untuk penanganan kesalahan.

Setelah API mengirimkan respons, `subscribe()` digunakan untuk memproses hasil yang diterima. Fungsi ini menangani dua skenario utama: respons sukses (`next`) dan kesalahan (`error`).

## If(Response == true) :
```typescript
next: (res: any) => {
        console.log('sukses', res);
        this.dataMahasiswa = res;
      },
```
Blok `next` akan dijalankan ketika server mengirimkan respons yang sukses. Dalam kasus ini, respons dari server (`res`) akan berisi data mahasiswa yang diambil.
Data tersebut kemudian disimpan ke dalam variabel `this.dataMahasiswa`.

## If(Response == false) :
```typescript
error: (err: any) => {
    console.log(err); // Menampilkan pesan kesalahan di konsol.
},
```
Blok `error` akan dijalankan jika terjadi kesalahan dalam mengambil data, misalnya kegagalan koneksi atau masalah pada API.


# Update (Edit)


![Edit Mahasiswa](https://github.com/user-attachments/assets/0d1abd01-013f-4008-9309-9b1df00fa9ef)


![Edit Mahasiswa - 2](https://github.com/user-attachments/assets/5e6cd948-4d45-40ca-84a0-3eb1420ccf07)


![Edit Mahasiswa - 3 ](https://github.com/user-attachments/assets/2d3fbd6b-e236-4360-8313-2edafd286590)

## Alert untuk update (edit) data

![Alert Edit Mahasiswa](https://github.com/user-attachments/assets/e629d1f0-1079-42b4-be32-cdaa9a52913d)


Source code dari fungsi update (edit) diatas : 

```typescript
 editMahasiswa() {
    let data = {
      id: this.id,
      nama: this.nama,
      jurusan: this.jurusan,
    };
    this.api.edit(data, 'edit.php').subscribe({
      next: async (hasil: any) => {
        console.log(hasil);
        this.resetModal();
        this.getMahasiswa();
        console.log('Berhasil Edit Mahasiswa');
        this.modalEdit = false;
        this.modal.dismiss();
        await this.showAlert('Data mahasiswa berhasil diedit');
      },
      error: (err: any) => {
        console.log('Gagal Edit Mahasiswa');
      }
    });
  }
}
```

# Penjelasan Fungsi `editMahasiswa()`

Fungsi `editMahasiswa()` digunakan untuk mengedit data mahasiswa melalui API. Berikut adalah penjelasan rinci tentang cara kerja fungsi ini.

```typescript
let data = {
  id: this.id,
  nama: this.nama,
  jurusan: this.jurusan,
};
```
Fungsi ini dimulai dengan mempersiapkan objek data yang berisi informasi mahasiswa yang akan diedit. Nilai-nilai this.id, this.nama, dan this.jurusan diambil dari form input atau komponen yang terkait di dalam aplikasi.

## Mengirim Permintaan Edit ke API
Setelah objek data siap, fungsi this.api.edit(data, 'edit.php') dipanggil untuk mengirim data ini ke server melalui endpoint edit.php
`this.api.edit(data, 'edit.php')`

Permintaan untuk mengedit data mahasiswa dikirim ke server menggunakan fungsi this.api.edit(). Data yang telah disiapkan (data) akan dikirim ke endpoint edit.php di server untuk diproses.

## Menangani Respons dari API
Fungsi subscribe() digunakan untuk menangani respons dari server. Respons ini dapat berupa dua kondisi: berhasil atau gagal.

## If(request == true) -> Edit Berhasil :
```typescript
next: async (hasil: any) => {
  console.log(hasil);
  this.resetModal();
  this.getMahasiswa();
  console.log('Berhasil Edit Mahasiswa');
  this.modalEdit = false;
  this.modal.dismiss();
  await this.showAlert('Data mahasiswa berhasil diedit');
}
```

- **`console.log(hasil);`**  
  Menampilkan hasil respons dari server di konsol.

- **`this.resetModal();`**  
  Mereset modal atau form input untuk persiapan data baru.

- **`this.getMahasiswa();`**  
  Memanggil fungsi untuk memperbarui daftar mahasiswa setelah data berhasil diedit.

- **`console.log('Berhasil Edit Mahasiswa');`**  
  Menampilkan pesan di konsol yang menandakan bahwa pengeditan berhasil.

- **`this.modalEdit = false;`**  
  Menutup modal edit setelah pengeditan selesai.

- **`this.modal.dismiss();`**  
  Menutup modal aktif yang sedang ditampilkan.

- **`await this.showAlert('Data mahasiswa berhasil diedit');`**  
  Menampilkan pesan alert untuk memberi tahu pengguna bahwa data mahasiswa berhasil diedit.

## if(request == false) -> Edit tidak berhasil : 
```error: (err: any) => {
    console.log('gagal edit Mahasiswa');
},
```

Jika permintaan edit gagal, pesan 'Gagal Edit Mahasiswa' akan dicetak ke konsol untuk memberitahukan bahwa proses pengeditan tidak berhasil.

# Delete 

![Hapus Mahasiswa - 1 ](https://github.com/user-attachments/assets/a2bbda72-4a3b-4533-84e1-1029bba36e47)

## Konfirmasi Hapus

![Konfirmasi Hapus](https://github.com/user-attachments/assets/bf5d1765-e679-4f2c-9f3d-15b8d4d01c6a)

## Alert delete data berhasil

![Alert Hapus](https://github.com/user-attachments/assets/df306152-7124-46c1-824a-f745d136f9b3)

Source code delete dari gambar diatas : 
```typescript
hapusMahasiswa(id: any) {
    this.api.hapus(id, "hapus.php?id=").subscribe({
      next: async (res: any) => {
        console.log('sukses', res);
        this.getMahasiswa();
        console.log('berhasil hapus data');
        await this.showAlert('Data mahasiswa berhasil dihapus!');
      },
      error: (error: any) => {
        console.log('gagal');
      }
    });
  }
```

## Penjelasan Delete data
Fungsi this.api.hapus() digunakan untuk mengirim permintaan HTTP ke server dengan menyertakan id mahasiswa yang ingin dihapus. URL endpoint hapus.php?id= diikuti dengan ID mahasiswa yang ingin dihapus.

Kemudian, metode this.api.hapus(id, 'hapus.php?id=') dipanggil untuk mengirim permintaan penghapusan data mahasiswa ke server. Endpoint 'hapus.php?id=' digunakan bersama dengan ID mahasiswa yang diteruskan untuk menentukan mahasiswa yang akan dihapus.

```typescript
this.api.hapus(id, 'hapus.php?id=').subscribe({
      next: (res: any) => {
        console.log('sukses', res);
        this.getMahasiswa();
        console.log('berhasil hapus data');
      },
      error: (error: any) => {
        console.log('gagal');
      },
    });
```

Setelah permintaan penghapusan dikirim, fungsi subscribe() digunakan untuk menangani respons dari server. Dua blok utama dalam subscribe adalah next untuk respons yang berhasil, dan error untuk menangani kegagalan.

## if(request == true) -> Penghapusan Berhasil:
```typescript
next: async (res: any) => {
        console.log('sukses', res);
        this.getMahasiswa();
        console.log('berhasil hapus data');
        await this.showAlert('Data mahasiswa berhasil dihapus!');
      },
```
- **`console.log('sukses', res);`**  
  Menampilkan pesan "sukses" beserta respons dari server di konsol, yang menunjukkan bahwa data berhasil dihapus.

- **`this.getMahasiswa();`**  
  Memanggil fungsi untuk memperbarui daftar mahasiswa setelah data berhasil dihapus.

- **`console.log('berhasil hapus data');`**  
  Menampilkan pesan di konsol yang menandakan bahwa penghapusan data berhasil.

- **`await this.showAlert('Data mahasiswa berhasil dihapus!');`**  
  Menampilkan pesan alert yang memberi tahu pengguna bahwa data mahasiswa telah berhasil dihapus.

## if (request == false) -> Penghapusan tidak berhasil : 
```
 error: (error: any) => {
        console.log('gagal');
      }
    });
  }
```
pesan "gagal" yang ditampilkan di konsol untuk memberi tahu bahwa penghapusan data tidak berhasil.
