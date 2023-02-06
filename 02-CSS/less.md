# less

## **1.less 嵌套**

**提前安装less插件  Easy LESS**

直接书写在大括号内

```css
.box {
    width:200px
    .box1{
        height:100px            
    }
}
```

## **2.less 并集**

&连接

```css
.box {
    width:100px;
    &hover:{
        color:red;    
    }
}
```

## **3.less 变量**

```css
@变量名：变量值

@color:red;
.box {
    color:@color;
}
.top {
    color:@color;
}
```

## **4.less 导入其他less文件**

```css
// @important 后必须有空格
//必须分号结束

@important './ ...'
```

### **4.1-less 导出插件配置**

- less导出在单独的css文件夹里

![image-20221218123640010](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20221218123640010.png)

![image-20221218123701671](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20221218123701671.png)

### **4.2-控制单个less 文件导出**

```css
1在less文件 第一行书写
// out: ./文件夹名称/
// out: ./例子/


2可以更改导出css文件名称
// out: ./文件夹名称/css文件名.css
// out: ./例子/例子.css
```

### **4.3 控制单个 less文件禁止导出**

```css
在第一行书写    注意没有分号
// out: false
```

