## **一. for 循环**

### **1.1- for 循环的基本使用**

```javascript
for (起始条件；退出条件；变量变化值) {
    循环体
}

//示例：
let arr = ['1','2','3']
//                arr.length 获取数组的长度
for (let i = 0; i < arr.length;i++}) {
    prompt.write('for循环的基本使用')
}
```

- 注意 不能使用相同的变量和声明退出条件

**for 循环与 while循环的区别：**

- **明确循环的次数****推荐使用for循环**
- **否则使用while循环**

### **1.2- 退出循环**

```javascript
continue 跳出本次循环
break 退出循环
```

### **1.3- 循环嵌套**

- for 循环嵌套

```javascript
//外部循环执行1次，内部循环执行全部
for (外部声明记录循环次数的变量；循环条件；变量变化值) {
    for (内部声明记录循环次数的变量；循环条件；变量变化值) {
         循环体       
    }
}

//示例：
for (let i =1;i < 6;i++) {
    document.write(`今天是第${i}天 <hr>`)
    for (let j = 1;j < 6;j++) {
    document.write(`我记住了${j}个单词 <hr>`)
    }
}
```

### **1.4- 循环标记（游标）**

- 如果要跳出循环嵌套环中的其他指定循环语句，需要给循环语句指定标记

```javascript
//循环标记（游标）
//游标可以换行 但中间不能有其他语句
out:
for (let i = 0;i < 10;i++) {
    inner: for (let j = 0;j < 10;j++ ) {
        if (j === 5) {
            break out        
        }    
        console.log("j:",j)
    }
        console.log("i:"i)
}
```

## **二. 数组**

数组（array）是一种可以按照顺序保存数据的数据类型

### **2.1- 数组的基本使用**

#### **2.1.1- 声明语法**

```javascript
//声明语法：
let 数组名 = [数据1，数据2，数据3，...]

//示例：
let arr = ['小明','小刚','小花','小王']
```

- 数组是按照顺序保存的，所以每个数据都有自己的编号
- 计算机中的编号是从0开始的，所以小明的编号为0，小刚的编号为1...
- 在数组中，数据的编号也叫索引或下标
- 数组中可以存储任意类型的数据

#### **2.1.2- 取值语法**

```javascript
//取值语法：
数组名[]

//示例：
arr [0]    //小明
arr [2]    //小花
```

#### **2.1.3- 术语**

- 元素：数组中保存每个数据都叫做数组元素
- 下标：数组中的编号
- **长度：数组中的数据个数，通过数组的length属性获取**

```javascript
arr.length    //4
```

#### **2.1.4- 遍历数组**

- 用循环把数组中的每个元素都访问到	一般使用for循环

```javascript
//语法：
for (let i = 0;i < 数组名.length;i++) {
    数组名[i]
}

//示例
let arr = [10,20,30]
for (let i = 0;i < arr.length;i++) {
    document.write(arr[i])
}
```

### **2.2- 数组中的length属性**

- 修改length属性
- 修改的值比原来的值小，超出length范围的值会被丢弃

```javascript
var arr = [1,2,3,4,5];
arr.lentgh = 3;
console.log(arr.length);//3
console.log(arr);// [1, 2, 3]
```

- 修改的值比原来的值大，不足的位置为空，不会添加新的值

```javascript
var arr = [1,2,3,4,5];
arr.length = 10;
console.log(arr.length);//5
console.log(arr);// [1, 2, 3, 4, 5, lenth: 10]
```

### **2.3- 传值与传址**

![image-20221218152138000](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20221218152138000.png)

### **2.4- 多维数组**

- 数组嵌套

```javascript
// 多维数组： 数组的值也是一个数组。

// 一维数组
// var arr1 = [1,2,3,4];
// console.log(arr1);

// 二维数组
var arr2 = [1, [2, 3, 4], 5, 6];
console.log(arr2);

// 三维数组
var arr3 = [1, [2, 3, 4], [5, 6, [7, 8]]];
console.log(arr3);

// 三维数组
var arr4 = [1, 2, [3, 4, 5, 6, [7, 8], [9, 10]]]

```

### **2.5- 操作数组**

- **数组本质是数据集合，操作数据无非就是** **增 删 改 查** **语法：(修改原数组)**

![image-20221218152205055](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20221218152205055.png)

#### **2.5.1- 查** 

```javascript
    let arr = ['red' , 'green' , 'blue']
    //访问 查询
    //数组名[下标]
    
    //示例:
    arr[0]        //console.log(arr[0])        red
```

#### **2.5.2- 改** 

```javascript
    let arr = ['red' , 'green' , 'blue']
    //重新赋值
    //数组名[下标] = 新值
    
    //示例:
    arr[1]='skyblue'        //console.log(arr[1])    skyblue
```

#### **2.5.3-** **增**

数组中添加新的数据：

**数组末尾添加：**

- 数组名.push(新增的内容) 方法将一个或多个元素添加到数组的末尾，并返回数组的新长度

```javascript
//语法：
arr.push(元素1，元素2，...)
```

示例：

```javascript
    let arr = ['red' , 'green' , 'blue']
    
    //末尾增加元素
    arr.push('pink')
    console.log(arr)        //[red,green,blue,pink]
    
    //并返回数组的新长度    需要删除上面增加的元素否则会新增两遍
    console.log(arr.push('pink'))    //4
    
    //添加多个值
    arr.push('gold','white')
```

**数组开头添加：**

- 数组名.unshift(新增的内容)方法将一个或多个元素添加到数组的开头，并返回数组的新长度

```javascript
//语法：
arr.unshift(元素1，元素2，...)
```

示例：

```javascript
    let arr = ['red' , 'green' , 'blue']
    
    //开头添加元素
    arr.unshift('gold')
    console.log(arr)    //[gold,red,green,blue]
    
    //返回数组新的长度 注意删除之前新增的元素，否则会重复新增
    console.log(arr.unshift('red'))    //4
    
    //添加多个值
    arr.unshift('red','gold')
```

#### **2.5.4-** **删**

数组删除元素：

删除最后一个元素：pop

- 数组.pop()方法从数组中删除最后一个元素，并返回改元素的值

```javascript
//语法：
arr.pop()
```

示例：

```javascript
    let arr = ['red' , 'green' , 'blue']
    
    //删除数组中最后一个元素
    arr.pop()
    console.log(arr)    //red,green
    
    //并返回删除的元素值
    console.log(arr.pop())    //blue
```

删除第一个元素:	shift

- 数组.shift()方法从数组中删除第一个元素，并返回该元素的值

```javascript
//语法：
arr.shift
```

示例：

```javascript
    let arr = ['red' , 'green' , 'blue']
    
    //删除数组中第一个元素
    arr.shift()
    console.log(arr)    //red
```

### **2.6- 数组提取方法	slice（不修改原数组）**

slice()	从数组中提取指定范围的子数组

1. 提取范围包括开始位置的值，不包括结束位置的值
2. 开始位置必须小于结束位置
3. 只指定一个参数表示从指定下标位置开始，提取后面所有的值
4. 不指定参数，会复制整个数组
5. 参数可以为负数，表示从后往前数位置（但start表示的位置必须在end之前）

```javascript
        var arr = [1, 2, 3, 4, 5, 6, 7, 8];

        console.log(arr);

        //提取范围包括开始位置，但不包括结束位置
        var newArr = arr.slice(2, 4);   //3,4

        //如果只指定一个参数，表示从开始位置，提取到后面所有的值
        var newArr = arr.slice(2);  // 3,4,5,6,7,8

        // 不指定参数，表示复制整个数组
        var newArr = arr.slice();   // 1,2,3,4,5,6,7,8

        // 开始位置表示的值的下标必须小于结束位置的下标
        var newArr = arr.slice(6, 2);
        var newArr = arr.slice(2, 2);

        // 参数可以为负数，负数表示从后往前数值的位置
        var newArr = arr.slice(2, -2);  //3,4,5,6
        
        console.log(newArr);
```

### **2.7- 数组方法	splice（修改原数组）**

```javascript
    var arr = [1, 2, 3, 4, 5];

        // 1替换效果
        // 从数组的第二个位置开始，删除后面的两个值，并添加30，40
        // arr.splice(2, 2, 30, 40);

        // 2添加效果
        // 从数组的第二个位置开始，删除0个值，并添加30，40
        // arr.splice(2, 0, 30, 40);

        // 3删除效果
        // 从数组的第二个位置开始，删除三个值
        // arr.splice(2, 3);
        // 只有一个参数表示从开始位置，删除后面所有的值
        // arr.splice(2);

        // 不指定参数，不会删除任何值
        // arr.splice();
        // console.log(arr);

        // 4返回值： 返回被删除的值，如果没有删除值，返回值为空数组
        console.log(arr.splice(2))
        console.log(arr.splice())
```

删除指定元素：	splice

- 数组.splice()方法，删除指定元素

```javascript
//语法：
arr.splice(start,deletecount)
arr.splice(起始位置，删除第几个元素)

//start 起始位置，从第几个元素开始删除（从0计数）
//deletecount    表示要移除的元素个数，如省略默认从指定位置删除到结尾
```

示例：

```javascript
    let arr = ['red' , 'green' , 'blue']
    
    //删除指定元素 起始位置（从0开始），删除几个    
    arr.splice(1,1)
    console.log(arr)    //red,blue
```

### **2.8- 数组方法 数组排序（修改原数组）**

- #### **2.8.1 reverse()	 将数组顺序完全颠倒过来，没有参数，返回值为颠倒之后的数组**

```javascript
var arr = [1,2,3,4];
arr.reverse();
console.log(arr);    //[4,3,2,1]

var arr2 = ['a', 'b', 'c'];
console.log(arr2.reverse());    //['c','b','a']
console.log(arr2);    //['c','b','a']
```

- #### **2.8.2 sort()		将数组按照指定规则排序**

1. **默认排序**

- 以字符比较大小的规则进行排序：**比较的是字符串的编码，并且是单个字符比较。**

```javascript
var arr1 = [123,24,53,21];
arr.sort();
console.log(arr);    //// [123, 21, 24, 53]
```

#### **自定义排序**

自定义排序要求给sort传递参数，参数必须符合以下规则：

1. 参数只能是函数（通常为排序函数）
2. 函数必须有返回值
3. 返回值只能是有效数字

定义排序的排序函数：

- sort()方法每次排序时，都会自动调用排序函数，并将需要排序的两个值作为参数传递给排序函数

```javascript
var arr1 = [123,24,53,21];
arr1.sort(function(a,b) {
    //每次只会传入两个参数
    //console.log(arr);
    
    //1.从小到大排序    第一个参数 - 第二个参数
    return a - b;
    
    //2.从大到小排序    第二个参数 - 第一个参数
    return b - a;
})
console.log(arr1);
```

#### ==**随机排序**==

```javascript
var arr2 = [1,2,3,4,5,6];
arr2.sort(function() {
    // 随机排序
    return Math.random() - 0.5;
});
console.log(arr2)
```

==知识点 面试题：==

不同浏览器模拟reverse() 方法：

```javascript
var arr2 = [1,2,3,4,5,6];
//Firefox浏览器模拟reverse() 方法：
arr2.sort(function () {
    //返回任意一个正数
    return 123；
})
console.log(arr2);

//webkit内核的浏览器模拟reverse() 方法：
arr2.sort(function() {
    // 返回任意一个负数
    return -123;
})
console.log(arr2);
```

### **2.9- 数组合并方法 concat（不修改原数组）**

- 合并一个或多个数组
- 语法：

```javascript
数组1.concat(数组2，数组3，...);
```

- 特点：

1. 合并顺序为书写顺序
2. 返回值是合并之后的新数组
3. 原来的数组可以被多次复制

```javascript
        // 数组合并 concat
        var arr1 = [1, 2, 3, 4];
        var arr2 = [5, 6, 7, 8];

        // 将数组 arr1 和 arr2合并成一个数组 并赋值给变量 arr3
        var arr3 = arr1.concat(arr2);
        console.log(arr3);    //[1,2,3,4,5,6,7,8]

        // concat方法 是复制原来的数组，产生一个新数组。原来的数组可以被多次复制。书写的顺序决定了合并之后数组中的数值顺序
        var arr3 = arr1.concat(arr1, arr2, arr2);    // [1, 2, 3, 4, 1, 2, 3, 4, 5, 6, 7, 8, 5, 6, 7, 8]
        var arr3 = arr1.concat(arr2, arr1);    // [1, 2, 3, 4, 5, 6, 7, 8, 1, 2, 3, 4]
        console.log(arr3);
```

数组降维：	**通过循环判断**

将二维数组	变成		一维数组

```javascript
//将二维数组 变成一维数组（数组降维）
        var arr4 = [[1, 2, 3, 4], [5, 6, 7, 8]]
        // 声明新的数组
        var arr5 = [];
        // 数组4中的第0个值 拼接 第1个值 =赋值给 数组5
        // arr5 = arr5.concat(arr4[0], arr4[1]);
        // 简写：
        var arr5 = [].concat(arr4[0], arr4[1]);
        console.log(arr5);
```

#### **2.10- 数组合并为字符串	join （不修改数组）**

- 将数组中的所有值转成字符串，并使用连接字符串将所有字符串依次拼接成字符串

```javascript
        //  数组方法 合并为字符串jojn()
        var arr = [1, 2, 3, 4];
        
        // 不设置 默认以逗号作为链接字符
        var str = arr.join();
        console.log(str);
        
        //  链接字符
        var str = arr.join('&');
        console.log(str);

        /* 
            join():     将数组中的所有值，合并成一个字符串
                参数：指定一个作为连接符的任意长度字符串

                如果不指定参数，默认以逗号作为连接符
        */
```

### **3.- 冒泡排序**

```javascript
        //  1.声明数组
        let arr = [123, 234, 345, 456, 567]
        //  2.for循环   外层循环执行次数    次数=数组的长度-1
        //  i=0(数组从0开始)
        for (let i = 0; i < arr.length - 1; i++) {
            // 3.内层循环   每执行一次,每次里面的数值交换几次   次数=数组的长度-数值里数值的个数i-1
            for (let j = 0; j < arr.length - i - 1; j++) {
                // 4.判断 数组中的 第0个值 是否大于第1个值
                // 数组中数据是从0开始排序,则第一个值是0,第二个值是0+1
                if (arr[j] > arr[j + 1]) {
                    // 5.符合条件的进行交换变量
                    // 声明一个临时变量用于存储
                    let temp = arr[j]
                    arr[j] = arr[j + 1]
                    arr[j + 1] = temp
                }
            }
        }
        // 6.循环完 打印数组名称即可
        console.log(arr)
```

### **4.- 柱形图案例**

- 用户输入每个季度的数据，渲染在页面	

```css    <style>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        .box {
            display: flex;
            width: 700px;
            height: 300px;
            border-left: 1px solid pink;
            border-bottom: 1px solid pink;
            margin: 50px auto;
            justify-content: space-around;
            align-items: flex-end;
            text-align: center;
        }

        .box>div {
            display: flex;
            width: 50px;
            background-color: pink;
            flex-direction: column;
            justify-content: space-between;
        }

        .box div span {
            margin-top: -20px;
        }

        .box div h4 {
            margin-bottom: -35px;
            width: 70px;
            margin-left: -10px;
        }
    </style>
```

```javascript
    <script>
        // 1.声明一个空数组 用于存储用户输入每个季度的数据
        let arr = []
        // 2.循环弹出4次输入框
        for (let i = 1; i < 5; i++) {
            // 3.用户输入的数据依次存储数组
            arr.push(prompt(`请输入第${i}季度的数据`))
        }
        //  4.先渲染表格的头部
        document.write(`<div class="box">`)
        // 6.循环四个柱形
        for (let i = 0; i < arr.length; i++) {
            document.write(`
            <div style="height: ${arr[i]}px;">
                <span>${arr[i]}</span>
                <h4>${i + 1}</h4>
            </div>
            `)
        }
        //  5.最后渲染表格的尾部
        document.write(`</div>`)
    </script>
```

