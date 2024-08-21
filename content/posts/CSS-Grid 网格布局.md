---
share: true
title: CSS-Grid 网格布局：对应属性详解
date: 2024-08-21
description: grid网格布局的属性的详细讲解
categories: CSS
comment: false
draft: false
tags:
  - review_knowledge_code_css
  - publish
---
#review/knowledge/code/css 
#publish

Flex 布局是轴线布局，只能指定"项目"针对轴线的位置，可以看作是**一维布局**。
Grid 布局则是将容器划分成"行"和"列"，产生单元格，然后指定"项目所在"的单元格，可以看作是**二维布局**。

Grid 布局可以做出比 Flex 布局复杂很多的网页布局。

---
### 容器和项目: 项目是容器的顶级子元素，Grid 布局只对项目生效
采用网格布局的区域，称为"容器"（container）
容器内部采用网格定位的子元素，称为"项目"（item）


```html
<div>
  <div><p>1</p></div>
  <div><p>2</p></div>
  <div><p>3</p></div>
</div>
```
上面代码中，最外层的`<div>`元素就是容器，内层的三个`<div>`元素就是项目。

```ad-attention
title: 注意：项目只能是容器的顶层子元素，不包含项目的子元素
比如上面代码的`<p>`元素就不是项目。
Grid 布局只对项目生效，不对项目的子元素生效。
```

---
### 容器中的行与列
容器里面的水平区域称为"行"（row），垂直区域称为"列"（column）
![关于容器](https://cdn.beekka.com/blogimg/asset/201903/1_bg2019032502.png)

#### 行和列的交叉区域产生"单元格"（cell）
`n`行和`m`列会产生`n x m`个单元格
#### 划分行列的线称为"网格线"(grid line)
`n`行有`n + 1`根水平网格线，`m`列有`m + 1`根垂直网格线
一个 4 x 4 的网格，共有5根水平网格线和5根垂直网格线

#### 可以使用方括号，指定每一根网格线的名字，方便以后的引用

```css
.container {
  display: grid;
  grid-template-columns: [c1] 100px [c2] 100px [c3] auto [c4];
  grid-template-rows: [r1] 100px [r2] 100px [r3] auto [r4];
}
```

上面代码指定网格布局为3行 x 3列，因此有4根垂直网格线和4根水平网格线。
方括号里面依次是这八根线的名字。
网格布局允许同一根线有多个名字，比如`[fifth-line row-5]`。

---

Grid 布局的属性分成两类。
一类定义在容器上面，称为容器属性
另一类定义在项目上面，称为项目属性。

---
### 容器属性

#### `display: grid`指定一个容器采用网格布局
```css
div {
  display: grid;
}
```
容器设为网格布局以后，容器子元素（项目）的`float`、`display: inline-block`、`display: table-cell`、`vertical-align`和`column-*`等设置都将失效。

#### grid-template-columns 属性：指定每列的宽度  
```css
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 100px 100px;
}
```
一般情况下就是均分
`grid-template-columns: 100px 100px 100px;`
当然不均分也行
`grid-template-columns: 100px 200px 300px;`

如果想要按比例划分也没问题：使用 **fr(fraction)** 单位
`grid-template-columns: 1fr 2fr 3fr;`
也可以写百分数，效果是一样的
`grid-template-columns: 20% 30% 50%;`

---
#### grid-template-rows 属性：指定每行的高度
```css
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 100px 100px;
}
```

---
#### minmax()函数：指定最大最小值

minmax()函数产生一个长度范围，表示长度就在这个范围之中。
它接受两个参数，分别为最小值和最大值。
```css
grid-template-columns: 1fr 1fr minmax(100px, 1fr);
```

---
#### 使用`repeat`来简写grid-template-rows 和 grid-template-columns
```css
.container {
  display: grid;
  grid-template-columns: repeat(3, 33.33%);
  grid-template-rows: repeat(3, 33.33%);
}
```

repeat()接受两个参数，第一个参数是重复的次数（上例是3），第二个参数是所要重复的值。

repeat()重复某种模式也是可以的。
```css
grid-template-columns: repeat(2, 100px 20px 80px);
```
上面代码定义了6列，第一列和第四列的宽度为100px，第二列和第五列为20px，第三列和第六列为80px。

---

#### auto-fill 自动填充关键字：每一行（或每一列）容纳尽可能多的单元格
有时，单元格的大小是固定的，但是容器的大小不确定。
如果希望每一行（或每一列）容纳尽可能多的单元格，这时可以使用`auto-fill`关键字表示自动填充。

```css
.container {
  display: grid;
  grid-template-columns: repeat(auto-fill, 100px);
}
```

上面的代码表示每列宽度`100px`，然后自动填充，直到容器不能放置更多的列。

---
#### `auto-fit`自动设配关键字：如果容器太大，会尽量扩大单元格的宽度

`auto-fill`和`auto-fit`的行为基本是相同的。
只有当容器足够宽，可以在一行容纳所有单元格，并且单元格宽度不固定的时候，才会有行为差异：
auto-fill会用空格子填满剩余宽度，auto-fit则会尽量扩大单元格的宽度。

---

#### auto 关键字
`auto`关键字表示由浏览器自己决定长度。
以下例子中第二列的列宽会是(容器宽度-200px)的值
```css
grid-template-columns: 100px auto 100px;
```

---
#### `row-gap` + `column-gap` = `grid-gap` 指定单元格之间的间隔
如果`grid-gap`省略了第二个值，浏览器认为第二个值等于第一个值。
>也就是`gap: 20px;` 相当于 `gap:20px 20px;`
```css
.container {
  row-gap: 20px;
  column-gap: 20px;
  /*以上两行代码相当于 gap: 20px 20px;*/
}
```

---

#### justify-items 属性：设置单元格内容的水平位置（左中右）
```
start：对齐单元格的起始边缘，相当于左对齐
end：对齐单元格的结束边缘，相当于右对齐
center：单元格内部居中，相当于居中
stretch：拉伸，占满单元格的整个宽度（默认值）
```

---
#### align-items 属性：设置单元格内容的垂直位置（上中下）
```
start：对齐单元格的起始边缘，相当于上对齐
end：对齐单元格的结束边缘，相当于下对齐
center：单元格内部居中，相当于居中
stretch：拉伸，占满单元格的整个高度（默认值）
```

---
#### `place-items`属性是`align-items`属性和`justify-items`属性的合并简写形式
```css
place-items: <align-items> <justify-items>;
```
例子：左对齐，下对齐
```css
 place-items: start end;
 ```
如果省略第二个值，则浏览器认为与第一个值相等。

---
#### justify-content + align-content = place-content 整个内容区域在容器里面的相对位置

以`justify-content: start;`为例，效果相当于整个内容区域在容器里面的左对齐
{{< figure src="/images/Pasted image 20240821134836.png"  width="411" height="">}}

---

#### column-rule 属性可以在多列布局中设定**分割线**的宽度、样式和颜色

```css
column-rule: dotted;
column-rule: solid 8px;
column-rule: solid blue;
column-rule: thick inset blue;
```

---

### 项目属性

#### 通过网格线定位指定项目的位置
1. grid-column-start 属性
2. grid-column-end 属性
3. `grid-column`属性：`grid-column-start`和`grid-column-end`的合并简写形式
4. grid-row-start 属性
5. grid-row-end 属性
6. `grid-row`属性：`grid-row-start`属性和`grid-row-end`的合并简写形式

项目的位置是可以指定的，具体方法就是指定项目的四个边框，分别定位在哪根网格线
> 1. `grid-column-start`属性：左边框所在的垂直网格线
> 2. `grid-column-end`属性：右边框所在的垂直网格线
> 3. `grid-column`属性: 左边框和右边框所在的垂直网格线
> 4. `grid-row-start`属性：上边框所在的水平网格线
> 5. `grid-row-end`属性：下边框所在的水平网格线
> 6. `grid-row`属性: 上边框和下边框所在的垂直网格线


---
```css
.item-1 {
  grid-column-start: 2;
  grid-column-end: 4;
}
```
上面代码指定，1号项目的左边框是第二根垂直网格线，右边框是第四根垂直网格线。
>代码没有指定上下边框，所以会采用默认位置：
>即上边框是第一根水平网格线，下边框是第二根水平网格线
>**除了1号项目以外，其他项目都没有指定位置，由浏览器自动布局：**
>位置由容器的`grid-auto-flow`属性决定
>这个属性的默认值是`row`，因此会"先行后列"进行排列
{{< figure src="/images/Pasted image 20240821135321.png"  width="290" height="">}}

这四个属性的值还可以使用`span`关键字，表示"跨越"，即左右边框（上下边框）之间跨越多少个网格。
```css
.item-1 {
  grid-column-start: span 2;
}
```
以上代码表示，1号项目的左边框距离右边框跨越2个网格。

---

#### 设置单元格内容和单元格的相对位置：`place-self`
justify-self属性设置单元格内容的水平位置（左中右）
>跟justify-items属性的用法完全一致，但只作用于单个项目。

align-self属性设置单元格内容的垂直位置（上中下）
>跟align-items属性的用法完全一致，也是只作用于单个项目。

place-self属性是align-self属性和justify-self属性的合并简写形式

---

