# Belajar_Express
Disini aku ingin belajar express dengan membuat API sederhana dengan pemisahan file route dan controller.

## First Step
Menginisialisasi project dengan perintah berikut:

```bash
npm init -y
npm install express
npm install -D typescript ts-node @types/node @types/express
npx tsc --init
```
`npm init -y` : Perintah ini digunakan untuk membuat file package.json secara otomatis yang nantinya berisi identitas dari project yang kita miliki seperti nama project, versi, script untuk running, serta daftar library/paket yang kita install.
`npm install express` : Perintah ini digunakan untuk menginstall library express di project yang akan kita gunakan. Akan muncul folder `node_modules` (tempat menyimpan kode library express).
`npm install -D typescript ts-node @types/node @types/express` : Perintah ini untuk install typescript dan tools pendukungnya.
`npx tsc --init` : Untuk membuat file konfigurasi tsconfig.json.

## Second steps
Membuat struktur folder:

```text
tugas-api/
├── src/
│   ├── controllers/
│   │   └── productController.ts
│   ├── routes/
│   │   └── productRoutes.ts
│   └── index.ts
├── package.json
└── tsconfig.json
```

<img width="450" height="391" alt="image" src="https://github.com/user-attachments/assets/0f17f65d-4084-42f8-98f3-ae83cf55774f" />

## Third steps

1. Membuat kode untuk array dan logika bisnisnya di `src/controllers/productController.ts`:

```
import { Request, Response } from 'express';

//Interface untuk struktur data produk
interface Product{
    id: number;
    name: string;
    price: number;
}

const products: Product[] = [
    {id: 1, name: 'Buku', price: 15000},
    { id: 2, name: 'Pulpen', price: 5000}
];


//Fungsi GET All products
export const getAllProducts = (req: Request, res: Response): void => {
    res.status(200).json(products);
};

//Fungsi GET Products by ID
export const getProductByID = (req: Request, res: Response): void => {
    const { id } = req.params;
    const productId = parseInt(String(id), 10);
    const product = products.find((p) => p.id === productId);

    if (!product) {
        res.status(404).json({message: 'produk tidak ditemukan'});
        return;
    }

    res.status(200).json(products);
}

```

Interface berfungsi untuk menentukan struktur (kontrak) dari suatu objek, dimana jika terjadi typo si typescript bisa langsung mengingatkan kita. Tidak seperti di javacript yang jika kita typo akan dibiarin aja. Selain itu interface ini juga bisa mengaktifkan Auto-completion dari Vs Code untuk merekomendasikan struktur data dari products misalnya.

Void di typescript sendiri itu berarti fungsi tersebut tidak mengembalikan nilai apapun. 

2. Membuat kode untuk routes API nya di `src/routes/productRoutes.ts`:

```
import { Router } from 'express';
import { getAllProducts, getProductById } from '../controllers/productController.js';

const router = Router();

// Route /products
router.get('/', getAllProducts);

// Route /products/:id
router.get('/:id', getProductById);

export default router;
```

Pastikan untuk menambahkan ekstensi js mespkipun kita menggunakan Typescript sesuai dengan standar ESM.

3. Membuat kode di `src/index.ts` untuk mengkonfigurasi portnya:

```
import express, { Application } from 'express';
import productRoutes from './routes/productRoutes.js';

const app: Application = express();
const PORT = 3000;

app.use(express.json());

// Main route
app.use('/products', productRoutes);

app.listen(PORT, () => {
  console.log(`Server running at http://localhost:${PORT}`);
});
```

## Fourth Step 

Tambahkan kode ini di package.json agar tidak perlu compile manual tiap mau nyoba codenya. Tinggal ketik perintah `npm start` di terminal.

```
{
  "name": "tugas-api",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "ts-node src/index.ts"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "type": "module",
  "dependencies": {
    "express": "^5.3.0"
  },
  "devDependencies": {
    "@types/express": "^5.0.6",
    "@types/node": "^26.6.5",
    "ts-node": "^10.9.2",
    "typescript": "^7.0.2"
  }

}
```

file tsconfig.json:

```
{
  // Visit https://aka.ms/tsconfig to read more about this file
  "compilerOptions": {
    // File Layout
    // "rootDir": "./src",
    // "outDir": "./dist",

    // Environment Settings
    // See also https://aka.ms/tsconfig/module
    "module": "nodenext",
    "target": "esnext",
    "types": [],
    // For nodejs:
    // "lib": ["esnext"],
    // "types": ["node"],
    // and npm install -D @types/node

    // Other Outputs
    "sourceMap": true,
    "declaration": true,
    "declarationMap": true,

    // Stricter Typechecking Options
    "noUncheckedIndexedAccess": true,
    "exactOptionalPropertyTypes": true,

    // Style Options
    // "noImplicitReturns": true,
    // "noImplicitOverride": true,
    // "noUnusedLocals": true,
    // "noUnusedParameters": true,
    // "noFallthroughCasesInSwitch": true,
    // "noPropertyAccessFromIndexSignature": true,

    // Recommended Options
    "strict": true,
    "jsx": "react-jsx",
    "verbatimModuleSyntax": false,
    "isolatedModules": true,
    "noUncheckedSideEffectImports": true,
    "moduleDetection": "force",
    "skipLibCheck": true,
  }
}
```
