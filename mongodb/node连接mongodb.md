# nodejs 连接 mongodb

## 原生方式

```js
const { MongoClient } = require('mongodb');

const client = new MongoClient('mongodb://root:1qaz2wsx,@47.101.141.27:27017');

const main = async () => {
    await client.connect();
    const retro = client.db('retrospective');
    const userCol = retro.collection('user');
    const list = await userCol.find({ age: { $gt: 22 } });
    console.log('list: ', await list.toArray());

    const listOne = await userCol.findOne({ age: { $gt: 22 } });
    console.log('listOne: ', listOne);
};

main().finally(() => {
    client.close();
});
```

## 使用 mogonoose

```js
const mongoose = require('mongoose');

const main = async () => {
    await mongoose.connect('mongodb://zhoujiawei:1qaz2wsx,@47.101.141.27:27017/retrospective');
};

main()
    .then(res => {
        console.log('res: ', res);
    })
    .catch(err => {
        console.log('err: ', err);
    });

// 定义 user schema
const userSchema = new mongoose.Schema({
    username: {
        type: String,
        required: true,
        unique: true
    },
    nickname: {
        type: String
    },
    phone: {
        type: String
    },
    password: {
        type: String
    }
});

// 创建 collection => users
const UserModel = mongoose.model('User', userSchema);

new UserModel({
    username: 'zhoujiawei',
    nickname: '1qaz2wsx,',
    phone: '',
    password: ''
}).save();
```