# PrintPlugin

[![npm version](https://badge.fury.io/js/bluetooth-print-js.svg)](https://badge.fury.io/js/bluetooth-print-js)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

PrintPlugin adalah plugin JavaScript untuk mencetak teks menggunakan printer thermal Bluetooth. Plugin ini mendukung dua ukuran kertas: "58mm" dan "80mm".

## Fitur

1. Mencetak teks dengan berbagai opsi seperti huruf tebal, garis bawah, perataan, dan ukuran teks.
2. Mencetak teks dengan dua kolom.
3. Mencetak garis putus-putus.
4. Mencetak baris baru.
5. Mendukung dua ukuran kertas: "58mm" dan "80mm".
6. Mendukung koneksi ke printer thermal Bluetooth.
7. Mendukung browser modern seperti Chrome, Firefox, dan Edge.
8. Mendukung Node.js.
9. Mendukung penggunaan CDN.
10. Mendukung penggunaan NPM.
11. Mendukung penggunaan ES6.

## Instalasi

### Menggunakan NPM

```bash
npm install bluetooth-print-js
```

import atau require PrintPlugin ke dalam proyek Anda.

```javascript
import PrintPlugin from "bluetooth-print-js";
```

or

```javascript
const PrintPlugin = require("bluetooth-print-js");
```

### Menggunakan CDN

```html
<script src="https://cdn.jsdelivr.net/npm/bluetooth-print-js@1.0/index.min.js"></script>
```

## Penggunaan

### Membuat Instance dari PrintPlugin

Anda dapat membuat instance dari PrintPlugin dengan atau tanpa menentukan ukuran kertas yang diinginkan. Ukuran kertas yang didukung adalah "58mm" dan "80mm". Jika ukuran kertas tidak ditentukan, ukuran kertas default adalah "58mm".

```javascript
// Create instance with paper size "80mm"
let printer = new PrintPlugin("80mm");
```

or

```javascript
// Create instance with default paper size "58mm"
let printer = new PrintPlugin();
```

### Menghubungkan ke Printer dan Mencetak Teks

Gunakan metode `connectToPrint` untuk menghubungkan ke printer Bluetooth dan mencetak teks. Anda perlu menyediakan dua fungsi callback: `onReady` dan `onFailed`.

- `onReady`: Dipanggil ketika koneksi ke printer berhasil. Anda dapat menggunakan objek print yang diteruskan ke callback ini untuk mencetak teks.
- `onFailed`: Dipanggil ketika koneksi ke printer gagal. Anda dapat menggunakan parameter message untuk mendapatkan pesan error.

### Cara Menggunakan PrintPlugin

1. Hubungkan ke printer dan cetak teks.

   ```javascript
   printer.connectToPrint({
     onReady: async (print) => {
       await print.writeText("Hello, World!");
     },
     onFailed: (message) => {
       console.log(message);
     },
   });
   ```

2. Contoh penggunaan lengkap (HTML):

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Bluetooth Print</title>
  </head>
  <body>
    <h1>Bluetooth Print</h1>
    <button id="connect">Connect and Print</button>
    <p id="status"></p>

    <script src="https://cdn.jsdelivr.net/npm/bluetooth-print-js@1.0/index.min.js"></script>
    <script>
      document.getElementById("connect").addEventListener("click", () => {
        let printer = new PrintPlugin("80mm");
        printer.connectToPrint({
          onReady: async (print) => {
            // Print Header
            await print.writeText("SADIGIT", {
              align: "center",
              bold: true,
              size: "double",
            });
            await print.writeText(
              "Jl. Kutamaya No.Ruko A, Kotakulon, Kec. Sumedang Sel., Kabupaten Sumedang, Jawa Barat 45311",
              { align: "center" }
            );
            await print.writeText("0852-2299-9699", { align: "center" });
            await print.writeLineBreak();
            await print.writeText("No.Transaksi: SDGT-ONL-0001", {
              align: "center",
            });
            await print.writeText("Kasir: Otongsuke", { align: "center" });
            await print.writeText("2024-10-23 10:20:18", { align: "center" });

            // Print Items
            await print.writeDashLine();
            for (let i = 0; i < 5; i++) {
              await print.writeText("Item Sample-" + i, { align: "left" });
              await print.writeTextWith2Column("1 pcs x 10.000", "10.000");
            }
            await print.writeDashLine();

            // Print Total
            await print.writeTextWith2Column("Total :", "50.000");
            await print.writeTextWith2Column("Bayar :", "100.000");
            await print.writeTextWith2Column("Kembali :", "50.000");
            await print.writeTextWith2Column("Metode :", "Tunai");

            // Print Footer
            await print.writeLineBreak();
            await print.writeText(
              "Terimakasih sudah mencoba Follow IG @sadigit.id",
              { align: "center" }
            );
            await print.writeLineBreak(3);
          },
          onFailed: (message) => {
            console.log(message);
          },
        });
      });
    </script>
  </body>
</html>
```

### Metode yang Tersedia

- `writeLineBreak({ count = 1 })`: Menulis baris baru.
- `writeDashLine()`: Menulis garis putus-putus.
- `writeTextWith2Column(text1, text2, options)`: Menulis teks dengan dua kolom.
- `writeText(text, options)`: Menulis teks.
- `connectToPrint({ onReady, onFailed })`: Menghubungkan ke printer dan memanggil callback onReady atau onFailed.

### Opsi untuk Metode `writeText` dan `writeTextWith2Column`

- `bold`: Menentukan apakah teks dicetak dengan huruf tebal. (default: false)
- `underline`: Menentukan apakah teks dicetak dengan garis bawah. (default: false)
- `align`: Menentukan perataan teks. Nilai yang didukung: "left", "center", "right". (default: "left")
- `size`: Menentukan ukuran teks. Nilai yang didukung: "normal", "double". (default: "normal")
