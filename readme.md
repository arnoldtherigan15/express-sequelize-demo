
<div style="background-image:url('assets/jumbotron.jpg'); padding:10px; background-position:center;background-size:cover;background-repeat:no-repeat;display:flex;justify-content:center;align-items:center;width:100%; height:350px;background-attachment:fixed">
<div style="padding:20px; background-color:rgba(255, 255, 255,.8);border-radius:20px">
<span style="font-size:30px;">Express Sequelize</span>
</div>
</div>


## Learning Objectives
    1. Recap Express
    2. Recap Sequelize
    3. Make an App using Express & Sequelize

<h3 style="background-color:#f3993e;color:#fff;padding:15px; border-radius:10px;margin-bottom:0">Recap Express</h3>
<div style="display:flex;align-items:center">
<img src="assets/express.png" style="height:200px; object-fit:contain" />
<p style="text-align:justify; margin-left:10px">Express merupakan framework minimal untuk pembuatan web pada nodejs yang sangat populer. Dengan menggunakan express ini kita bisa membuat REST API dengan sangat cepat ataupun kita bisa membuat aplikasi web Server Side Rendering dengan menggunakan template engine tertentu, seperti pug, ejs, dan lain lain.</p>
</div>

<h3 style="background-color:#f3993e;color:#fff;padding:15px; border-radius:10px;margin-bottom:0">Recap Sequelize</h3>
<div style="display:flex;align-items:center">
<p style="text-align:justify; margin-right:10px">Sequelize merupakan ORM (Object Relational Mapper) berbasis promise untuk PostgreSQL, MariaDB, MySQL, SQLite, dan MSSQL. dengan menggunakan Sequelize ini, kita nantinya tidak perlu lagi menuliskan query sederhana lagi pada SQL kita, karena akan dihandle dengan berbagai method yang sudah disediakan oleh Sequelize.</p>
<img src="assets/sequelize.png" style="height:200px; object-fit:contain" />
</div>

<p style="font-style:italic">Pada pembelajaran kali ini kita akan membuat sebuah aplikasi yang akan menggunakan:</p>

<div style="background-color:#f7f8fb;padding:10px">
<ul>
<li>express sebagai web framework kita.</li>
<li>ejs sebagai templating engine tampilan.</li>
<li>postgres sebagai database SQL penampung data.</li>
<li>sequelize sebagai ORM untuk mempermudah dalam penulisan query.</li>
</ul>
</div>

<h3 style="background-color:#f3993e;color:#fff;padding:15px;">Lets Make The App</h3>
<p style="font-style:italic">Requirement dari aplikasi ini adalah:</p>
<div style="background-color:#f7f8fb;padding:10px">
<ul>
<li>Diberikan sebuah database pada postgresql dengan nama pokemon</li>
<li>Dengan diberikan sebuah data data/pokemon.json, buatlah sebuah tabel dengan nama Pokemons pada database yang dibuat.</li>
<li>Tabel Pokemons akan memiliki kolom yang dapat dilihat pada Tabel 1</li>
<li>Buatlah sebuah aplikasi web sederhana yang akan memiliki endpoint yang dapat dilihat pada Tabel 2</li>
</ul>
</div>
<h4 style="margin-top:20px">Tabel 1</h4>
<table style="margin-top:20px">
  <tr>
    <th>Column</th>
    <th>Type</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>id</td>
    <td>SERIAL</td>
    <td>PRIMARY KEY</td>
  </tr>
  <tr>
    <td>name</td>
    <td>VARCHAR(255)</td>
    <td>NOT NULL</td>
  </tr>
  <tr>
    <td>type</td>
    <td>VARCHAR(255)</td>
    <td>NOT NULL</td>
  </tr>
  <tr>
    <td>imgURL</td>
    <td>TEXT</td>
    <td>NOT NULL</td>
  </tr>
</table>

<h4 style="margin-top:20px">Tabel 2</h4>
<table style="margin-top:20px">
  <tr>
    <th>Endpoint</th>
    <th>Description</th>
  </tr>
  <tr>
    <td><span style="background-color:#f3993e;color:#fff">GET</span> /pokemons</td>
    <td>Menampilkan semua data <span style="background-color:#f3993e;color:#fff">Pokemons</span></td>
  </tr>
  <tr>
    <td><span style="background-color:#f3993e;color:#fff">GET</span> /pokemons/add</td>
    <td>Menampilkan form penambah <span style="background-color:#f3993e;color:#fff">Pokemons</span></td>
  </tr>
  <tr>
    <td><span style="background-color:#f3993e;color:#fff">GET</span> /pokemons/edit/:id</td>
    <td>Menampilkan edit form <span style="background-color:#f3993e;color:#fff">Pokemons</span></td>
  </tr>
  <tr>
    <td><span style="background-color:#f3993e;color:#fff">POST</span> /pokemons/edit/:id</td>
    <td>Menghandle edit form <span style="background-color:#f3993e;color:#fff">Pokemons</span></td>
  </tr>
  <tr>
    <td><span style="background-color:#f3993e;color:#fff">GET</span> /pokemons/delete/:id</td>
    <td>Menghandle delete <span style="background-color:#f3993e;color:#fff">Pokemons</span></td>
  </tr>
</table>

<p style="font-style:italic">Jadi setelah melihat requirement seperti, apa sajakah yang harus kita lakukan?</p>

<h3 style="background-color:#f3993e;color:#fff;padding:15px;">
Langkah 1 - Inisialisasi & Install module yang dibutuhkan terlebih dahulu</h3>
<p style="font-style:italic">
Pertama-tama kita harus menginisialisasi folder dan module yang dibutuhkan dengan cara:</p>

<div style="background-color:#f7f8fb;padding:10px">
<ul>
<li>Melakukan <span style="background-color:#f3993e;color:#fff">npm init -y</span></li>
<li>Menginstall module yang dibutuhkan express, ejs, pg, dan sequelize dengan <span style="background-color:#f3993e;color:#fff">npm install express ejs pg sequelize</span></li>
<li>Menginstall module yang dibutuhkan pada saat development yaitu nodemon dan sequelize-cli dengan 
<span style="background-color:#f3993e;color:#fff">npm install -D nodemon sequelize-cli</span></li>
</ul>
</div>

<p style="font-style:italic; margin-top:20px">Maka setelah ini bentuk foldernya akan menjadi seperti ini:</p>

```
.
├── data
│   └── pokemon.json
├── node_modules
│   └── ...
├── package.json
└── package-lock.json
```
<h3 style="background-color:#f3993e;color:#fff;padding:15px;">
Langkah 2 - Inisialisasi dan Konfigurasi sequelize</h3>
<p style="font-style:italic">
Selanjutnya setelah kita melakukan inisialisasi dan konfigurasi `sequelize` 
dengan cara:</p>
<div style="background-color:#f7f8fb;padding:10px">
<ul>
<li>Inisialisasi sequelize dengan <span style="background-color:#f3993e;color:#fff">npx sequelize-cli init</span></li>
<li>Setelah perintah di atas diketik, maka akan terbentuk folder:
<ul>
<li><span style="background-color:#f3993e;color:#fff">config</span>, yang digunakan untuk menyimpan konfigurasi database</li>
<li><span style="background-color:#f3993e;color:#fff">migrations</span>, yang digunakan untuk membuat atau mengubah tabel pada database</li>
<li><span style="background-color:#f3993e;color:#fff">models</span>, yang digunakan untuk membuat Model representasi tabel data</li>
<li><span style="background-color:#f3993e;color:#fff">seeders</span>, yang digunakan untuk memasukkan data awal.</li>
</ul>
</li>
<li>Mengedit file konfigurasi pada <span style="background-color:#f3993e;color:#fff">config/config.json</span> dengan memasukkan 
  credential yang tepat, dan <span style="background-color:#f3993e;color:#fff">dialect</span> yang diubah ke <span style="background-color:#f3993e;color:#fff">postgres</span></li>
<li>Setelah melakukan konfigurasi di atas, dengan menggunakan GUI / CLI untuk 
  <span style="background-color:#f3993e;color:#fff">postgresql</span> buatlah database dengan nama <span style="background-color:#f3993e;color:#fff">pokemon</span>. </li>
</ul>
</div>

<h3 style="background-color:#f3993e;color:#fff;padding:15px;">
Langkah 3 - Membuat tabel Identities</h3>
<p style="font-style:italic">
Selanjutnya adalah kita akan membuat model <span style="background-color:#f3993e;color:#fff">Pokemon</span> dengan tabel <span style="background-color:#f3993e;color:#fff">Pokemons</span>
sekaligus dengan menggunakan <span style="background-color:#f3993e;color:#fff">sequelize</span>. Caranya adalah dengan:</p>

<div style="background-color:#f7f8fb;padding:10px">
<ul>
<li>Melihat ulang terlebih dahulu struktur tabel yang dibutuhkan, perhatikan 
  bahwa pada struktur tabel memiliki <span style="background-color:#f3993e;color:#fff">id</span>, namun pada saat kita menggunakan
  <span style="background-color:#f3993e;color:#fff">sequelize</span> kita tidak perlu menuliskan hal tersebut, karena akan dibuat
  secara otomatis. Sehingga berdasarkan info ini, maka pada tabel <span style="background-color:#f3993e;color:#fff">Pokemons</span>,
  yang butuh dibuat adalah <span style="background-color:#f3993e;color:#fff">name</span>, <span style="background-color:#f3993e;color:#fff">type</span>, <span style="background-color:#f3993e;color:#fff">imgURL</span></li>
<li>Harus diingat juga ketika membuat model dan tables / migrations, perhatikan
  bahwa nama pada Model = <span style="background-color:#f3993e;color:#fff">Singular</span> dan nama table = <span style="background-color:#f3993e;color:#fff">Plurals</span>.
  <span style="background-color:#f3993e;color:#fff">Jangan sampai terbalik yah !</span></li>
<li>Sehingga, berdasarkan data ini, maka yang harus diketik adalah 
  <span style="background-color:#f3993e;color:#fff">npx sequelize-cli model:generate --name Pokemon --attributes "name:String,type:String,imgURL:String",phone:String,address:String</span></li>
<li>Setelah perintah di atas diketik, maka akan terbentuk sebuah file pada 
  folder <span style="background-color:#f3993e;color:#fff">models</span> dan sebuah file pada folder <span style="background-color:#f3993e;color:#fff">migrations</span>, coba lihat file
  pada <span style="background-color:#f3993e;color:#fff">models</span> dan <span style="background-color:#f3993e;color:#fff">migrations</span>  untuk mengetahui lebih lanjut bagaimaan kode
  dibuat.</li>
<li>Dikarenakan kita di sini masih ingin menggunakan <span style="background-color:#f3993e;color:#fff">promise</span> instead of
  async / await, maka kita akan melakukan edit pada file <span style="background-color:#f3993e;color:#fff">migrations</span> yang
  baru dibuat oleh <span style="background-color:#f3993e;color:#fff">sequelize</span>, contoh perubahan untuk mengubah async / await
  menjadi <span style="background-color:#f3993e;color:#fff">promise</span> dapat dilihat pada Code 00</li>
<li>Selanjutnya, kita akan menjalankan perintah untuk membuat tabel ini dengan 
  menjalankan perintah <span style="background-color:#f3993e;color:#fff">npx sequelize-cli db:migrate</span></li>
<li>Setelah menjalankan perintah ini, maka dapat dilihat pada postgresql bahwa 
  tabel <span style="background-color:#f3993e;color:#fff;">Pokemons</span> sudah terbentuk.</li>

</ul>
</div>



### Code 00
```javascript
'use strict';
module.exports = {
  // Awalnya di sini ada async bukan ?
  // kita akan mengubah nya dengan cara 
  // membuang keyword async tersebut
  up: (queryInterface, Sequelize) => {
    // Kemudian di sini harusnya ada await bukan?
    // kita akan mengganti keyword await tersebut
    // menjadi return.
    // Hal ini dapat terjadi karena 
    // queryInterface.createTable sudah bersifat Promise.
    return queryInterface.createTable('Pokemons', {
      ...
    });
  },

  // sama dengan yang ada di up tadi, 
  // keyword asyncnya dibuang
  // dan await nya diganti return.
  down: (queryInterface, Sequelize) => {
    return queryInterface.dropTable('Pokemons');
  }
};
```

<h3 style="background-color:#f3993e;color:#fff;padding:15px;">
Langkah 4 - Membuat seeder</h3>
<p style="font-style:italic">Selanjutnya, setelah tabel terbentuk, kita akan memasukkan data yang kita
miliki dalam <span style="background-color:#f3993e;color:#fff">data/pokemon.json</span> menjadi data dalam tabel kita, oleh karena itu
langkah-langkahnya adalah:</p>
<div style="background-color:#f7f8fb;padding:10px">
<ul>
<li>Membuat file seed dengan cara mengetik perintah 
  <span style="background-color:#f3993e;color:#fff">npx sequelize-cli seed:generate --name seed-pokemons</span></li>
<li>Setelah mengetik perintah di atas, maka akan terbentuk file baru dengan nama
  <span style="background-color:#f3993e;color:#fff">seeders/[timestamp]-seed-pokemons.js</span>, buka file tersebut dan kita akan 
  mengedit file tersebut. Kode yang akan ditulis dapat dilihat pada 
  Code 01</li>
<li>Setelah menuliskan kode tersebut, kita akan melakukan *seeding* dengan cara 
  mengetik <span style="background-color:#f3993e;color:#fff">npx sequelize db:seed:all</span></li>
</ul>
</div>


#### Code 01
```javascript
'use strict';
// 01.
// Jangan lupa fs karena kita mau baca json
const fs = require('fs');

module.exports = {
  up: (queryInterface, Sequelize) => {
    // 02.
    // Di sini kita akan membaca filenya terlebih dahulu
    // Ingat bahwa pada sequelize semua tabel akan memiliki 2 kolom tambahan
    // createdAt dan updatedAt
    // sehingga kita harus memasukkan data tersebut.

    let pokemons = JSON.parse(fs.readFileSync('./data/pokemon.json', 'utf8'));

    pokemons = pokemons.map(elem => {
      // Jangan lupa dipetakan karena dalam tabel Pokemons dibutuhkan 
      // 2 tambahan kolom ini
      elem.createdAt = new Date();
      elem.updatedAt = new Date();

      return elem;
    })

    // 03. 
    // Masukkan data ke dalam tabel Pokemons
    // Kita gunakan 
    // return queryInterface.bulkInsert('NamaTabel', arrayObj, opt)
    return queryInterface.bulkInsert(
      'Pokemons', 
      pokemons, 
      {}
    );
  },

  down: (queryInterface, Sequelize) => {
    // Ceritanya, kalau ada up (kita melakukan)
    // down (kita mereverse apa yang kita lakukan)
    // Kita gunakan 
    // return queryInterface.bulkDelete('NamaTabel', arrayObj, opt)
    return queryInterface.bulkDelete('Pokemons', null, {});
  }
};
```


<h3 style="background-color:#f3993e;color:#fff;padding:15px;">
Langkah 5 - Membuat app.js, routes, dan Controller</h3>
<p style="font-style:italic">Sebelumnya, kita akan membuat sebuah folder bernama <span style="background-color:#f3993e;color:#fff">controllers</span> terlebih
dahulu dan membuat sebuah file <span style="background-color:#f3993e;color:#fff">controllers/controller.js</span> yang masih kosong</p>

<p style="font-style:italic">Selanjutnya kita akan membuat app.js beserta routesnya terlebih dahulu.</p>

<p style="font-style:italic">Pada file <span style="background-color:#f3993e;color:#fff">app.js</span> kita akan menuliskan kode untuk:</p>

<div style="background-color:#f7f8fb;padding:10px">
<ul>
<li>Menjalankan express</li>
<li>Menggunakan view engine ejs</li>
<li>Menggunakan body-parser / express.urlencoded</li>
<li>Membuat router express mengarah ke <span style="background-color:#f3993e;color:#fff">routes/index.js</span></li>
</ul>
</div>

<p style="margin-top:20px">Sehingga pada file <span style="background-color:#f3993e;color:#fff">app.js</span>, akan terbentuk kode sebagai berikut</p>


```javascript
const express = require('express');
const app = express();

const PORT = 3000;

// Jangan lupa import routes/index.js
// Abaikan bila error pada saat pembuatan pertama
const indexRoutes = require('./routes/index.js');

// set view engine
app.set('view engine', 'ejs');
// gunakan middleware bodyParser
app.use(express.urlencoded({ extended: true }));

// Menggunakan routes dari routes/index.js
// Abaikan bila error pada saat pembuatan pertama karena file dan
// folder belum terbentuk
app.use('/', indexRoutes);

// Jalankan express
app.listen(PORT, () => {
  console.log(`Aplikasi jalan di PORT ${PORT}`);
})
```


Selanjutnya kita akan mendefinisikan routes dan seluruh endpoint yang ada, 
hal ini akan kita definisikan dalam 2 file, yaitu:
* `routes/pokemon.js` yang berisi semua yang berhubungan dengan resource
  `pokemons/` dan
* `routes/index.js` yang berisi penampung semuanya.

Berdasarkan penjelasan di atas, maka selanjutnya adalah kita akan:
* Membuat folder `routes`
* Membuat file `routes/pokemon.js`
* Membuat file `routes/index.js`

Maka pada file `routes/pokemon.js`, kita juga akan mendefinisikan beberapa
method yang dibutuhkan sebagai method `Controller` untuk setiap endpoint yang
ada.

### routes/pokemon.js
```javascript
const express = require('express');
const router = express.Router();

const Controller = require('../controllers/controller.js);

// Semua router endpoint yang ada hub dengan /ide
// ditaruh di sini
router.get('/', Controller.findAllPokemon);
router.get('/add', Controller.showAddPokemon);
router.post('/add', Controller.createPokemon);
router.get('/edit/:id', Controller.showEditPokemon);
router.post('/edit/:id', Controller.updatePokemon);
router.get('/del/:id', Controller.destroyPokemon);

module.exports = router;
```

Selanjutnya kita akan berpindah pada file `routes/index.js` dan mendefinisikan
seluruh rute utama yang harus ada.

Kode untuk `routes/index.js` adalah sebagai berikut:

### routes/index.js
```javascript
const express = require('express');
const router = express.Router();

const Controller = require('../controllers/controller.js');
const pokemonRouter = require('./pokemon.js');

// Semua route akan dihandle oleh si index ini
router.use('/pokemons', pokemonRouter);

module.exports = router;
```

Setelah semua rute didefinisikan dan `app.js` selesai dibuat, maka saatnya 
kita berpindah ke bagian `otak` nya, yaitu `controllers.controller.js` dan
mendefinisikan method yang dibuat.

Diingat bahwa karena kita sudah menggunakan `sequelize`, maka untuk bagian
`models` sudah di-generate, kita tinggal menggunakannya saja !

WARNING:  
Untuk mempercepat proses pembelajaran, maka untuk `views` nya sudah 
disediakan template dan sudah disebutkan variabel apa yang dibutuhkan.
Diingat pada dunia nyata `views` ini harus dibuat sendiri yah !

```javascript
const { Pokemon } = require('../models/index.js');

class Controller {

  static findAllPokemon(req, res) {
    // Di sini kita akan me-render sebuah views bernama
    // views/home.ejs

    // view ini membutuhkan parameter
    //    title, untuk menaruh judul
    //    pokemons, untuk menaruh data yang didapat dari model
    //      dalam bentuk tabel
    Pokemon.findAll()
      .then(data => {
        res.render('home', {
          title: "Pokedex - Home",
          pokemons: data
        })
      })
      .catch(err => {
        res.send(err);
      });
  }
  
  static showAddPokemon(req, res) {
    // Di sini kita akan me-render sebuah views bernama
    // views/addPokemon.ejs

    // view ini membutuhkan parameter
    //    title, untuk menaruh judul
    res.render('addPokemon', {
      title: "Pokedex - Add"
    });
  }

  static createPokemon(req, res) {
    // Di sini kita akan menerima inputan form dari 
    // views/addPokemon.ejs

    // paramater yang diterima adalah
    // req.body.name
    // req.body.type
    // req.body.imgURL
    const { name, type, imgURL } = req.body;

    let pokemonData = { name, type, imgURL };

    Pokemon.create(pokemonData)
      .then(data => {
        console.log(`Data with id ${data.id} has been added !`);
        res.redirect('/pokemons');
      })
      .catch(err => {
        res.send(err);
      });
  }

  static showEditPokemon(req, res) {
    // Di sini kita akan menerima inputan dari 
    // parameter di endpoint dan 
    // Kemudian akan melakukan pencarian spesifik dari tabel
    // dan melemparkannya ke views/editPokemon.ejs

    // view ini membutuhkan parameter
    //    title, untuk menaruh judul
    //    pokemonData, untuk menerima data pokemon dari 
    //      hasil pencarian
    const { id } = req.params;

    Pokemon.findOne({
      where: { id }
    })
      .then(data => {
        res.render('editPokemon', {
          title: "Pokedex - Edit",
          pokemon: data
        })
      })
      .catch(err => {
        res.send(err);
      });
  }

  static updatePokemon(req, res) {
    // Di sini kita akan menerima inputan dari 
    // parameter di endpoint dan 
    // form dari views/editPokemon.ejs

    // paramater yang diterima adalah
    // req.body.name
    // req.body.type
    // req.body.imgURL
    
    const { name, type, imgURL } = req.body;

    let pokemonData = { name, type, imgURL };

    Pokemon.update(pokemonData, {
      where: {
        id: req.params.id
      }
    })
      .then(data => {
        console.log(`Data with id ${data.id} has been updated !`);
        res.redirect('/pokemons');
      })
      .catch(err => {
        res.send(err);
      });
  }

  static destroyPokemon(req, res) {
    // Di sini kita akan menerima inputan dari 
    // parameter di endpoint
    Pokemon.destroy({
      where: {
        id: req.params.id
      }
    })
      .then(data => {
        console.log(`Data with id ${data.id} has been deleted !`);
        res.redirect('/pokemons');
      })
      .catch(err => {
        res.send(err);
      });
  }
}

module.exports = Controller;
```

Sampai di tahap ini, artinya aplikasi kita sudah selesai dan siap dijalankan.

<h3 style="background-color:#f3993e;color:#fff;padding:15px;">
Langkah 6 - Jalankan Aplikasi</h3>
Mari kita jalankan aplikasi kita dengan mengetik <span style="background-color:#f3993e;color:#fff">npx nodemon app.js</span>

Selamat ! sampai di sini artinya kita sudah berhasil membuat aplikasi dengan
menggunakan express dan sequelize dan sudah berhasil melakukan CRUD sederhana !

## References
* [Wendy](https://withered-flowers.github.io/education-intermediate-express-sequelize/)

https://pokemondb.net/pokedex/national