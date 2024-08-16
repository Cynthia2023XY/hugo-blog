---
sr-due: 2024-08-19
sr-interval: 4
sr-ease: 276
tags:
  - review_knowledge_code_practice
---

#review/knowledge/code/practice 


### JSON: 语法最严格，**不支持注释**，但是通用性好，支持多种语言，多余的最外层大括号人类看着不太舒服，大小写敏感
（但是JSON本来就是给机器看的，只不过是<font color="#f79646">人类可读</font>而已，作为一种交换数据格式必须要用大括号区分不同的对象）

#### JSON一定是一段合法的 [js对象字面量（object literal）]({{< relref "js%E5%AF%B9%E8%B1%A1%E5%AD%97%E9%9D%A2%E9%87%8F%EF%BC%88object%20literal%EF%BC%89.md" >}})
但是合法的js对象字面量不一定是合法的JSON
比如说JSON中的键值对都要用双引号包裹，但是js对象字面量不一定，用单引号和干脆不用引号都是ok的

[json为什么不支持注释]({{< relref "json%E4%B8%BA%E4%BB%80%E4%B9%88%E4%B8%8D%E6%94%AF%E6%8C%81%E6%B3%A8%E9%87%8A.md" >}})
[js和json区别]({{< relref "js%E5%92%8Cjson%E5%8C%BA%E5%88%AB.md" >}})

---

### YAML：功能丰富，可以描述比JSON更复杂的内容，**支持注释**，缩进用空格不能用tab，大小写敏感
~~发音：鸭某~~
#### YAML相当于JSON省略大括号`{}`和键值对的双引号`" "`

---
#### YAML可以使用**锚点和引用**

比如说张三和李四的职业都是程序员，那么可以直接引用而不是重复书写
```yaml
person:
 name: default
 job: 程序员

person1:
 name: 张三
 job: 程序员

person2:
 name: 李四
 job: 程序员
```

就可以使用锚点`&`和引用`*`简写成:
```yaml
person:
 name: default 
 job: &Job 程序员 #这里是锚点&

#直接引用而不是重复书写
person1:
 name: 张三
 job: *Job #这里是引用*

person2:
 name: 李四
 job: *Job #这里是引用*
```

---
```yaml
defaults: &defaults
  adapter:  postgres
  host:     localhost

development:
  database: myapp_development
  <<: *defaults

test:
  database: myapp_test
  <<: *defaults
```

等同于
```yaml
defaults:
  adapter:  postgres
  host:     localhost

development:
  database: myapp_development
  adapter:  postgres
  host:     localhost

test:
  database: myapp_test
  adapter:  postgres
  host:     localhost
```

`&`用来建立锚点（`defaults`），`<<`表示合并到当前数据，`*`用来引用锚点。

---
```yaml
- &showell Steve 
- Clark 
- Brian 
- Oren 
- *showell 
```

转为 JavaScript 代码如下
```javascript

[ 'Steve', 'Clark', 'Brian', 'Oren', 'Steve' ]
```

---
#### YAML缩进用空格不能用tab
不过缩进的空格数不重要，只要相同层级的元素左对齐即可

---
#### YAML 支持的数据结构有三种

##### 1. 对象：键值对的集合
又称为映射（mapping）/ 哈希（hashes） / 字典（dictionary）
例如`animal: pets`

---
##### 2. 数组：一组按次序排列的值
又称为序列（sequence） / 列表（list）
可以用一组连词线开头的行构成一个数组。
```yaml
- Cat
- Dog
- Goldfish
```

数组也可以采用行内表示法。
```yaml
animal: [Cat, Dog]
```

---
##### 3. 纯量（scalars）：单个的、不可再分的值
纯量是最基本的、不可再分的值。
以下数据类型都属于 JavaScript 的纯量。

1. 字符串
2. 布尔值
3. 整数
4. 浮点数
5. Null (`null`在yaml用`~`表示)
6. 时间 （时间采用 ISO8601 格式）
7. 日期  （日期采用复合 iso8601 格式的年、月、日表示 YYYY-MM-DD）

纯量的数值直接以字面量的形式表示
```yaml
number: 12.30
iso8601: 2001-12-14t21:59:43.10-05:00 
parent: ~ #null用~表示
date: 1976-07-31
```

---
##### 4. 对象和数组可以结合使用，形成复合结构
```yaml
languages:
 - Ruby
 - Perl
 - Python 
websites:
 YAML: yaml.org 
 Ruby: ruby-lang.org 
 Python: python.org 
 Perl: use.perl.org 
```

转为 JavaScript 如下。
```js
{ languages: [ 'Ruby', 'Perl', 'Python' ],
  websites: 
   { YAML: 'yaml.org',
     Ruby: 'ruby-lang.org',
     Python: 'python.org',
     Perl: 'use.perl.org' } }
```

---

#### YAML 允许使用两个感叹号，强制转换数据类型
例如
```yaml
e: !!str 123
f: !!str true
```
转为 JavaScript 如下
```js
{ e: '123', f: 'true' }
```

---
#### JSON中的字符串

```yaml
str: 这是一行字符串
```

---
如果字符串之中包含空格或特殊字符，需要放在引号之中
```yaml
str: '这是什么 一行字符串'
str: "这是什么？一行字符串" #注意这里是中文问号
```

---
YAML单引号和双引号都可以使用，双引号不会对特殊字符转义
```yaml
s1: '内容\n字符串' #\n在我这里就是\n
s2: "内容\n字符串" #\n在我这里是换行
```
转为 JavaScript 如下
```javascript
{ s1: '内容\\n字符串', s2: '内容\n字符串' }
```

---
单引号之中如果还有单引号，必须连续使用两个单引号转义。
```yaml
str: 'labor''s day'
``` 
转为 JavaScript 如下
```js
{ str: 'labor\'s day' }
```

---
字符串可以写成多行，<font color="#f79646">从第二行开始，必须有一个单空格缩进</font>。
换行符会被转为空格。
```yaml
str: 这是一段
  多行
  字符串
```

转为 JavaScript 如下
```javascript
{ str: '这是一段 多行 字符串' }
```

---

### TOML：功能丰富，可以描述比JSON更复杂的内容，**支持注释**，缩进不影响格式解析，支持用`.`表示层级结构
~~发音：汤某~~ （作者叫Tom）
>Tom's Obvious, Minimal Language

#### TOML被设计为可以**无二义性地转换为一个哈希表**

#### 在TOML 中多余的缩进和空格不会影响格式解析

#### 可以用`.`代替层次缩进
```toml
name = "Jason Yu"
profile.age = 18
profile.addr = "Shanghai"
```
相当于YAML
```yaml
name: "Jason Yu"
profile:
  age: 18
  addr: "Shanghai"
```
相当于JSON
```js
{
  "name": "Jason Yu",
  "profile": {
    "age": 18,
    "addr": "Shanghai"
  }
}
```
