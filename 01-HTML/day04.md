# day04

## 1.表单

表单的作用：收集用户信息
表单的组成：**表单域 form**  表单控件 input select  textarea  提示信息  label

### 1.1- input表单的通用属性：

*   **name** 名称  表示数据名称（必须设置，才能提交在地址栏看到）
*
        注意：单选按钮要想实现单选name必须要有一组保持一致  name定义收集数据的地址
*   value 代表数据的默认值 (不会覆盖)
*   autocomplete 是否自动补全 历史记录
*   autofocus 自动获取焦点
*   **placeholder** 提示信息
*
          注意：css3.0新增 低版本浏览器不支持 value与placeholder同时存在显示的是value
*   size input文本框尺寸长度
*   **checked** 默认选中
*   **disabled** 禁用
*   readonly 只读 不能进行输入 但是功能还在
*   form 允许在form的外部收集用户的数据 （1.form标签设置id2.表单控件input设置form）
*   **maxlength** 最大可以输入的字符数 输入后校验
*   **minlength** 最小可以输入的字符数 输入后校验
*   **requried**  必填的 没有输入就可以校验
*   **pattern**   正则校验
*
          <!-- 匹配电话号码，1开头，后面跟着10个数字 -->
          <input type="text" name="tel" pattern="^1\d{10}$">

### 1.2- input type类型

（注意：记得书写表单域form）
如果和label连用 删除for属性 否则不生效
for指向id 不能和嵌套一起使用

*   文本框：text

```HTML
<form action="">
<input type="text">
</form>
```

*   密码框：password

```HTML
<input type="password">
```

*   复选框：checkbox

```HTML
<input type="checkbox">用户协议
```

*   单选按钮：radio

				name 名称  注意：单选按钮要想实现单选name必须要有一组保持一致

```HTML
<input type="radio" name="1">男
<input type="radio" name="1">女
```

*   提交按钮： submit

```HTML
<input type="submit" name="" id="">
```

*   普通按钮： button

				value 用于按钮命名

```HTML
<input type="button" value="按钮">
```

*   重置按钮：reset

```HTML
<input type="reset" value="重置">
```

*   图像域：image  可以当做提交按钮使用

```HTML
<input type="image" src="../../images/头像.jpg" width="100">
```

*   文件域：file multiple 可以实现多选
*   accept限制上传文件的类型
    image/\* 图片

```HTML
<input type="file" multiple>
```

*   隐藏：hidden

```html
<input type="hidden">
```

### 1.3- select

> select 下拉菜单
>
> 	option 下拉列表
> 	
> 			selected 默认选中
> multiple 复选下拉框 可以选择多条
> size 设置显示几条数据
> optgroup 实现分组 label设置值

```html
<form action="">
        <select>
            <option>河北</option>
            <option selected>北京</option>
            <option>上海黑客攻击老规矩浪科技</option>
            <option>上海黑客攻击老规矩浪科技</option>
            <option>上海黑客攻击老规矩浪科技</option>
            <option>上海黑客攻击老规矩浪科技</option>
        </select>
</form>
```

万年历 type属性date属性值

```html
multiple 多文件选择
<input type="date" multiple>
```

### 1.4- textarea

属性值是数字；单位是字节

> 多行文本域 textarea
> cols 宽度 单位字符  一行能够显示的字符数
> rows 高度 单位字符  可以显示多少行

```html
<textarea cols="10" rows="5" placeholder="请输入你的留言"></textarea>
```

### 1.5- 提示信息lable（点击昵称也可以选中）

如果和label连用 删除for属性 否则不生效
for指向id 不能和嵌套一起使用

```html
<form>
        用户名：<input type="text"><br>
</form>
```

```html
<form>
				<!-- 方式一 -->
        <label>用户名：<input type="text"></label><br>
</form>
```

```html
<form>
				<!-- 方式二 -->
  			<!-- 注意：for一定要于input的id对应 -->
        <label for="use">用户名：</label>
        <input type="text" id="use"><br>
  
        <label for="pwd">密码：</label>
        <input type="password" id="pwd">
</form>
```

## 2.- form

form  没有form不影响显示效果 影响功能

			name 表单的名称
	
			method 表单的提交方式和方法
	
						值：get 默认值  获取 会将信息放在地址栏中 安全性低 效率高
	
								post 上传 不会 安全性高  效率低action 提交地址
	    	target 提交时的窗口默认是当前窗口 \_blank 新窗口

## 3.- 按钮

==button 按钮可以当做标签来使用==

    <form>
            <button>按钮</button>
            <button><a href="http://www.baidu.com">百度</a></button>
            
            不可以使用：
            <input type="submit" value="<a href='http://www.baidu.com'>百度</a>">
            
    </form>

## 4.- 废弃标签

没有语义

```html
<b>加粗</b>

<i>倾斜</i>

<u>下划线</u>

<s>删除线</s>
```

## 5.- 语义化标签

```html
保留空白格式
    pre
        <pre>hello 
    
    
                    ickt</pre>
代码
    code
    <code>
        h1 { color: red; }
    </code>

<!-- 强调加粗 -->
<strong>hello</strong>
<!-- 强调斜体 -->
<em>hello</em>
<!-- 下划线 -->
<ins>hello</ins>
<!-- 删除线 -->
<del>hello</del>
<!-- 简写 -->
<abbr title="美国职业篮球联盟">NBA</abbr>
<!-- 地址 -->
<address>北京市，昌平区，北七家镇，平西府</address>
<!-- 下标 -->
O<sub>2</sub>
<!-- 上标 -->
100<sup>2</sup>

空格 
	（英文空格）&nbsp; 
	（中文空格）&emsp;
```

## 6.- 字符实体

```html
版权图标 &copy;
注册商标 &reg;

小于号 &lt;
大于号 &gt;

人民币符号&yen;
```

## 7.- html的规范

1：标签名必须小写

2：标签的属性小写 属性值双引号包裹

3：起名的时候要结构化语义化

4：class必须用小写 横线分隔单词

5：id使用驼峰命名法 第二个单词开始首字母大写

6：用四个空格设置缩进

   例如 whatIsYourName  header-left

## 8.- 快捷键

    div             创建标签
    div#id          设置id
    div.class       设置class
    div{内容}       设置内容
    div[title=msg]  设置属性
    div*5           创建多个
    div$*5          $用来计数，默认从1开始
    div$@5*3        @表示计数起始值
    div>p           表示嵌套
    div+p           创建同级的
    (div>p+span)*3  ()设置整体