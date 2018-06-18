## 一、生成器函数的特点。
>1、生成器的出现主要是为了简化掉创建迭代器繁琐的过程，同时保证逻辑的清晰性。
```js
function *makeIterator (arr) {
    for (let i = 0;i<arr.length;i++){
        yield arr[i]
    }
}

const gen = makeIterator(['吃饭','睡觉','打豆豆'])
```
>2、yield只能定义一个对象或者数组，不能定义一个字符串或者布尔值。
## 二、异步请求的几种方法。
>1、回调函数
```js
function readFile (cb) {
    fs.readFile('./package.json',(err,data) => {
        if (err) return cb(err)

        cb(null,data)
    })
}

readFile ((err,data) => {
    if(!err) {
        data = JSON.parse(data)

        console.log(data.name)
    }
})
```
>2、promise请求
```js
function readFileAsync (path){
    return new Promise((resolve,reject) => {
        fs.readFile(path,(err,data) => {
            if(err) reject(err)
            else resolve(data)
        })
    })
}

readFileAsync('./package.json')
    .then(data =>{
        data = JSON.parse(data)

        console.log(data.name)
    })
    .catch(err =>{
        console.log(err)
    })
```
>3、co + Generator Function + Promise
```js
const co = require('co')
const util = require('util')

co(function *() {
    let data = yield util.promisify(fs.readFile)(
        './package.json'
    )
    data = JSON.parse(data)
    
    console.log(data.name)
})
```
>4、Async函数
```js
const readAsync = util.promisify(fs.readFile)

async function init () {
    let data = await readAsync('./package.json')

    data = JSON.parse(data)
    console.log(data.name)
}
(注：node版本8.0以上可直接调用此方法)
```



