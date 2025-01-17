# Sequelize Hooks

## 1. [Apa itu hooks?](https://sequelize.org/v5/manual/hooks.html)

Secara mudahnya hooks adalah fungsi yang akan dijalankan sebelum atau sesudah sebuah operasi sequelize contoh sebelum create atau sesudah create. 

## 2. [Order of Operations Hook](https://sequelize.org/v5/manual/hooks.html#order-of-operations)
```
(1)
  beforeBulkCreate(instances, options)
  beforeBulkDestroy(options)
  beforeBulkUpdate(options)
(2)
  beforeValidate(instance, options)
(-)
  validate
(3)
  afterValidate(instance, options)
  - or -
  validationFailed(instance, options, error)
(4)
  beforeCreate(instance, options)
  beforeDestroy(instance, options)
  beforeUpdate(instance, options)
  beforeSave(instance, options)
  beforeUpsert(values, options)
(-)
  create
  destroy
  update
(5)
  afterCreate(instance, options)
  afterDestroy(instance, options)
  afterUpdate(instance, options)
  afterSave(instance, options)
  afterUpsert(created, options)
(6)
  afterBulkCreate(instances, options)
  afterBulkDestroy(options)
  afterBulkUpdate(options)
```

## 3. [Cara menggunakan hooks](https://sequelize.org/v5/manual/hooks.html#declaring-hooks)

Ada 3 cara untuk menggunakan hooks, kita bisa pilih sesuai dengan kebutuhan. Dalam sebuah hooks kita bisa melakukan banyak logic untuk memanipulasi data ataupun yang lainnya. Seperti misal jika setelah register kita akan mengirimkan email informasi bahwa berhasil register, maka itu dilakukan setelah create data user.
```js
// ===================================================
// cara 1 via .init() method
class User extends Model {}
User.init({
  username: DataTypes.STRING,
  mood: {
    type: DataTypes.ENUM,
    values: ['happy', 'sad', 'neutral']
  }
}, {
  hooks: {
    beforeValidate: (user, options) => {
      user.mood = 'happy';
    },
    afterValidate: (user, options) => {
      user.username = 'Toni';
    }
  },
  sequelize
});
// ===================================================

// ===================================================
// cara 2 via .addHook() method
User.addHook('beforeValidate', (user, options) => {
  user.mood = 'happy';
});

User.addHook('afterValidate', 'someCustomName', (user, options) => {
  return Promise.reject(new Error("I'm afraid I can't let you do that!"));
});
// ===================================================

// ===================================================
// cara 3 via direct method 
User.beforeCreate((user, options) => {
  return hashPassword(user.password).then(hashedPw => {
    user.password = hashedPw;
  });
});

User.afterValidate('myHookAfter', (user, options) => {
  user.username = 'Toni';
});
// ===================================================
```

## 4. [Global hooks](https://sequelize.org/v5/manual/hooks.html#global---universal-hooks)
Global hooks adalah hook yang akan dijalankan di semua model. Untuk menggunakan global hook maka kita akan menaruhnya di model/index.js
```
// codingan index yang lain

const basename = path.basename(__filename);
const env = process.env.NODE_ENV || 'development';
const config = require(__dirname + '/../config/config.json')[env];
const db = {};

// ===================================================
// Cara penggunaan global hooks
config.define: {
    hooks: {
        beforeCreate: () => {
            // Do something
        }
    }
}
// ===================================================

let sequelize;
if (config.use_env_variable) {
  sequelize = new Sequelize(process.env[config.use_env_variable], config);
} else {
  sequelize = new Sequelize(config.database, config.username, config.password, config);
}

// codingan index yang lain
```

## 5. Parameter pada hooks (instances, options)
Kebanyakan hooks memiliki 2 parameter yaitu instances dan options.
1. instance berisi object data yang telah di passing dari controller ke model
2. option berisi kolom dari table dll.

# Referensi 
- https://sequelize.org/v5/manual/hooks.html
- https://sequelize.org/v5/manual/hooks.html#order-of-operations
- https://sequelize.org/v5/manual/hooks.html#declaring-hooks
- https://sequelize.org/v5/manual/hooks.html#global---universal-hooks
