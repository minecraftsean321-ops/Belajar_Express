# Belajar_Express
Disini aku ingin belajar express dengan membuat API sederhana dengan pemisahan file route dan controller.

## First Step
Menginisialisasi project dengan perintah berikut:

```bash
npm init -y
npm install express
```
`npm init -y` : Perintah ini digunakan untuk membuat file package.json secara otomatis yang nantinya berisi identitas dari project yang kita miliki seperti nama project, versi, script untuk running, serta daftar library/paket yang kita install.
`npm install express` : Perintah ini digunakan untuk menginstall library express di project yang akan kita gunakan. Akan muncul folder `node_modules` (tempat menyimpan kode library express).

<img width="447" height="203" alt="image" src="https://github.com/user-attachments/assets/4cbab4c2-ba95-4e13-a32e-0e01a139b9b1" />

## Second steps
Membuat struktur folder:

```text
tugas-api/
├── controllers/
│   └── productController.js
├── routes/
│   └── productRoutes.js
├── index.js
└── package.json
```

<img width="448" height="384" alt="image" src="https://github.com/user-attachments/assets/d429e2f6-8392-4868-86bb-2e9b50345e49" />

## Third steps
Membuat kode untuk array dan logika bisnisnya di `./controllers/productController.ts`:

```
//Membuat array untuk product 
const products = [
    {id: 1, name:'Buku', price: 15000},
    {id: 2, name: 'Pulpen', price: 5000}

];

//Fungsi getall
const getAllProducts = (req, res) => {
    res.status(200).json(products);
};

//Fungsi get by id
const getProductById = (req, res) => {
    const id = parseInt(req.params.id);
    const product = products.find((p) => p.id === id);

    if(!product) {
        return res.status(404).json({message:  'produk tidak ditemukan'});
    }

    res.status(200).json(product);
}

module.exports = {
    getAllProducts,
    getProductById,
};
```
