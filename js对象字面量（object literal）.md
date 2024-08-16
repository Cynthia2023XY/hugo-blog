---
sr-due: 2024-11-06
sr-interval: 86
sr-ease: 270
tags:
  - review_knowledge_code_js
  - review_function
---

#review/knowledge/code/js 
#review/function 

在JavaScript中，有几种不同的方法来定义对象。
一种非常常见的方法是使用**对象字面量**，它通过在大括号内列出其属性来实现。

```js
const object1 = {
  name: 'Arto Hellas',
  age: 35,
  education: 'PhD',
}

const object2 = {
  name: 'Full Stack web application development',
  level: 'intermediate studies',
  size: 5,
}

const object3 = {
  name: {
    first: 'Dan',
    last: 'Abramov',
  },
  grades: [2, 3, 5, 3],
  department: 'Stanford University',
}
```

属性的值可以是任何类型的，比如整数、字符串、数组、对象......

---
```ad-faq
title:js的对象字面量（object literal）看起来很像C++的结构体?
JavaScript 的对象字面量确实在概念上与 C++ 中的结构体（struct）有一些相似之处，尽管它们在语法和使用上存在差异。以下是两者之间的一些相似点和不同点：

### 相似之处：

1. **数据结构**：两者都用于创建包含多个属性（或成员变量）的数据结构，这些属性可以是不同类型的数据。
2. **封装**：它们提供了一种封装数据的方式，使得数据和操作这些数据的函数或方法可以组合在一起。
3. **访问属性**：在两种结构中，属性都通过点符号（`.`）访问，例如 `object.name` 或 `structInstance.name`。

### 不同之处：

1. **语法**：
   - JavaScript 对象字面量使用花括号 `{}` 定义，属性通过键值对形式出现，例如 `{ name: "John", age: 30 }`。
   - C++ 结构体使用 `struct` 关键字定义，并且每个成员变量的类型需要明确指定，例如 `struct Person { string name; int age; };`。

2. **类型**：
   - JavaScript 是一种弱类型或动态类型语言，对象字面量中的属性可以是任意类型，甚至可以在运行时更改类型。
   - C++ 是一种静态类型语言，结构体的每个成员变量都有明确的类型，并且在编译时就已经确定。

3. **方法**：
   - JavaScript 对象可以包含方法，即对象的属性可以是函数，这些函数可以操作对象的其他属性。
   - C++ 结构体通常不包含方法，而是通过将结构体实例作为参数传递给函数来操作。

4. **原型继承**：
   - JavaScript 对象支持原型继承，对象可以从其他对象继承属性和方法。
   - C++ 结构体不支持继承，但可以通过组合或使用类（class）来实现类似的功能。

5. **构造函数**：
   - 在JavaScript中，可以使用构造函数或类来创建具有相同结构的对象，并且可以使用 `new` 关键字实例化对象。
   - C++ 中，结构体或类实例化是通过使用构造函数和 `new` 操作符来完成的。

6. **默认值**：
   - JavaScript 对象字面量可以很容易地使用默认参数值，例如 `{ name = "Default Name" }`。
   - 在C++中，结构体本身不包含构造函数，所以不能直接在定义中设置默认值，但可以通过类的构造函数来实现。
```

---
#### 一个对象的属性是通过使用 .符号或通过使用方括号`[]`来引用的
```js
console.log(object1.name)         // Arto Hellas is printed
const fieldName = 'age'
console.log(object1[fieldName])    // 35 is printed
```

#### 可以通过使用点符号或方括号来为一个对象即时添加属性
```js
object1.address = 'Helsinki'
object1['secret number'] = 12341
```

后者的添加必须使用括号，因为当使用点符号时，由于有空格字符，_secret number_ 不是一个有效的属性名称。

#### 对象字面量中可以定义函数
我们可以通过定义作为函数的属性来给对象分配方法。
```js
const arto = {
  name: 'Arto Hellas',
  age: 35,
  education: 'PhD',
  greet: function() {    
  console.log('hello, my name is ' + this.name)  
  },
}

arto.greet()  
// "hello, my name is Arto Hellas" gets printed
```

#### 即使在对象创建之后，方法也可以被分配给对象

```js
const arto = {
  name: 'Arto Hellas',
  age: 35,
  education: 'PhD',
  greet: function() {
    console.log('hello, my name is ' + this.name)
  },
}

arto.growOlder = function() {  this.age += 1}
console.log(arto.age)   // 35 is printed
arto.growOlder()
console.log(arto.age)   // 36 is printed
```