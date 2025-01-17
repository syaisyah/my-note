# Debugging

**Record**

## 1. [What Why](https://expressjs.com/en/guide/writing-middleware.html)

What:

Debugging adalah sebuah metode yang dilakukan untuk menganalisis alur kerja program, mencari dan mengurangi bug, atau kerusakan di dalam sebuah program tersebut agar bekerja sesuai dengan harapan

Why:

Debugging itu mungkin menyebalkan, developer itu lebih suka codenya langsung jalan, TAPI tidak ada yang menyangkal bahwa kemampuan debugging adalah kamampuan yang harus dimiliki setiap developer. 

“Good programmers know that they spend as much time debugging as writing so they try to learn from their mistakes.” - Brian W. Kernighan, Rob Pike

`HARUS, HARUS, HARUS BERSAHABAT DENGAN ERROR`

## 2. Sample Error

- Reference Error: ... is not defined / is not declared 

```js
function sum(a, b) {
    return a + b
}

let a = 10

console.log(sum(a, b))
```

- Unexpected token / Unexpected identifier
Kesalahan penulisan syntax

```js
let a = 10;
let b = 5;
let c = a + b +;

console.log(c)
```

- Output not same as requirement
```js
function sum(a, b) {
    return a + b
}

let a = 10;
let b;

console.log(sum(a, b))
```

- TypeError
Menggunakan built in function di tipe data yang tidak sesuai
```js
let a = 90

console.log(a.join(""))
```

```js
let iniVariable = 90

iniVariable()
```

- Infinite loop
```js
for (let i = 0; 0 < 10; i++) {
    console.log(i)
}

for (let i = 0; i > 10; i--) {
    console.log(i)
}
```

- Maximum call stack exceeded
```js
function rekursif() {
    console.log("rekursif terpanggil")

    rekursif()
}

rekursif()
```

## 3. Let's Try
```js
function stringReverser(params) {
    let result = ""

    for (let i = 0; i < params.lengh; ++i) {
        result = result + params[i]
    }

    console.log(result)
}

console.log(stringReverser("bakso"))
```

```js
function stringReverser(params) {
    let result = ""

    for (let i = params.length - 1; i >= 0; i--) {
        result = result + params[i]
    }

    return result
}

console.log(stringReverser("bakso"))
```

## 3. [Node Debugger](https://nodejs.org/dist/latest-v14.x/docs/api/debugger.html)
Node debugger adalah tools untuk debugging built in di nodejs.

To run node debugger:
`node inspect myscript.js`

- Debugger, Inserting the statement debugger.
- Next / n, Run code to next line of code.
- Continue / c, Run code to next breakpoint.
- Watch, Watch expression and variable values while debugging.

```js
// begin watching 
watch('my_expression')

// remove a watcher
unwatch('my_expression')

```

# Referensi 
- https://betterprogramming.pub/all-about-debugging-an-introduction-b9eeb0b24681
- https://nodejs.org/dist/latest-v14.x/docs/api/debugger.html
