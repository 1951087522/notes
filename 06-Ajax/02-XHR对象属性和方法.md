## 一、XHR对象属性和方法.md

### 1.1 xhr 属性

- `readyState` 存有的 XMLHttpRequest 的状态从0到4发生变化。

- `response` 表示服务器的响应内容。

- `responseType` 表示服务器返回数据的类型，缺省为空字符串，可取 "arraybuffer"，"blob"， "document"， "json"， "text" 共五种类型。

- `reponseText` 以文本形式返回响应。

- `responseXML` 以XML格式返回响应

- `status` 将状态返回为数字（例如，“Not Found”为404，“OK”为200）

- `statusText` 以字符串形式返回状态（例如，“Not Found”或“OK”）

- `timeout` 用于指定ajax的超时时长，表示多少毫秒后，如果请求仍然没有得到结果，就会自动终止。

- `upload` 属性默认返回一个XMLHttpRequestUpload对象，用于上传资源

### 1.2 status 状态码

200， OK，访问正常

301， Moved Permanently，永久移动

302， Move temporarily，暂时移动

304， Not Modified，未修改

307， Temporary Redirect，暂时重定向

401， Unauthorized，未授权

403， Forbidden，禁止访问

404， Not Found，未发现指定网址

500， Internal Server Error，服务器发生错误

完整HTTP状态码：[HTTP状态码](E:\AIC\notes\06-Ajax\HTTP 响应状态码.md)

### 1.3 xhr 方法

- `open()` 用于指定发送HTTP请求的参数

- `send()` 表示实际发出HTTP请求。参数是携带的数据

- `abort()` 取消当前请求。

- `getAllResponseHeaders()` 以字符串形式返回完整的HTTP标头集。

- `getResponseHeader( name)` 返回指定HTTP标头的值。

- `setRequestHeader(key，value)` 将标签/值对添加到要发送的HTTP标头。

- `overrideMimeType()` 用于强制指定response的MIME类型，即强制修改response的Content-Type。

### 1.4 xhr 事件

- `onreadystatechange` 事件 监听readystate状态改变

- `onloadstart` 事件 在ajax请求发送之前触发，触发时机在 readyState==1 状态之后，2状态之前

- `onprogress` 事件 在readyState==3状态时开始触发。

- `onerror` 事件 在ajax请求出错后执行，通常只在网络出现问题时触发

- `onabort` 事件 用来终止已经发出的HTTP请求。取消后, readyState状态将被设置为0。

- `onload` 事件 onload事件回调方法在ajax请求成功后(readyState==4状态后)触发。

- `onloadend` 事件 在ajax请求完成后`(readyState==4状态后或者readyState==2状态后)`触发。

- `ontimeout` 事件 判断请求超时。

```js
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    <h2>XMLHttpResult 对象</h2>
    <input type="button" value="取消请求">

    <script>
        var btn = document.querySelector('input');
        // 创建Ajax 对象
        var xhr = new XMLHttpRequest()

        // 建立请求链接
        xhr.open('get', '/data')
        // 请求json
        // xhr.open('get', '/data.json')
        // 请求xml文档
        // xhr.open('get', '/book.xml')


        // 一、请求完成
        /* xhr.onreadystatechange = function () {

            // 1.response() 后端返回的数据
            // console.log(xhr.response);

            // 2.readyState() 进行到哪一步
            // console.log(xhr.readyState);
        } */

        // 9.强制修改返回的数据格式(需要明确知道后端返回的数据类型)
        // xhr.responseType = 'json'

        // 14.overrideMimeType() 强制返回数据的 mime 类型
        // xhr.overrideMimeType('text/plain;charst=utf-8')

        // 11.设置超时时间 单位为毫秒
        xhr.timeout = 3000;

        // 二、当请求成功并成功接受数据之后触发的事件
        xhr.onload = function (req, res) {
            // 3.接受字符串值
            console.log(xhr.response);
            console.log(xhr.statusText);

            // 4.接受json数据
            /* var data = JSON.parse(xhr.responseText)
            console.log(data); */

            // 5.接受xml数据 转换成dom对象
            // console.log(xhr.responseXML);

            // 6.获取xml文档里的值
            /* var doc = xhr.responseXML
            var name = doc.querySelector('name')
            console.log(name.innerHTML); */

            // 7.状态码
            // console.log(xhr.status);

            // 8.错误请求
            // res.status(500).json({ msg: "ok" })

            // 12.getResponseHeader( name) 返回指定HTTP标头的值。
            // console.log(xhr.getResponseHeader('hello'));
            // 数据类型 
            // console.log(xhr.getResponseHeader('Content-type'));
            // 13.getAllResponseHeaders() 以字符串形式返回完整的HTTP标头集。
            // console.log(xhr.getAllResponseHeaders());
        }

        // 11.请求超时触发的事件
        xhr.ontimeout = function () {
            console.log("请求超时啦!");
        }

        // 12.取消当前请求
        xhr.onabort = function () {
            console.log("请求被终止啦!");
        };

        // 发送请求
        xhr.send()

        btn.onclick = function () {
            // 12.主动取消 ajax 请求.
            xhr.abort();
        }

    </script>
</body>

</html>
```

### 1.5 xhr 案例-上传下载进度

```js
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .process-wrap {
            width: 500px;
            height: 26px;
            line-height: 26px;
            border: 1px solid #333;
            position: relative;
            text-align: center;
            font-size: 16px;
            font-weight: bolder;
            font-family: "微软雅黑";
        }

        .process-under,
        .process-up {
            position: absolute;
            overflow: hidden;
            left: 0;
            top: 0;
            height: 100%;
            width: 100%;
            color: #000;
        }

        div.process-up {
            background: rgb(53, 135, 211);
            width: 0%;
        }

        .text {
            color: #fff;
            width: 500px;
            text-align: center;
        }

        .pic img {
            width: 400px;
        }
    </style>
</head>

<body>

    <h2>文件上传</h2>
    <form>
        <input type="file" name="file">
        <input type="button" value="上传">
    </form>

    <br>
    <br>
    <div class="process-wrap">
        <div class="process-under">上传进度：<span>0</span>%</div>
        <div class="process-up">
            <div class="text">上传进度：<span>0</span>%</div>
        </div>
    </div>
    <br>
    <br>
    <button>下载</button>
    <br>
    <br>
    <div class="process-wrap">
        <div class="process-under">下载进度：<span>0</span>%</div>
        <div class="process-up">
            <div class="text">下载进度：<span>0</span>%</div>
        </div>
    </div>
    <div class="pic"></div>

    <script>
        var wrap = document.querySelectorAll('.process-wrap');
        var btn = document.querySelector('[type=button]');
        var download = document.querySelector('button');
        var form = document.querySelector('form');
        var pic = document.querySelector('.pic');

        // 上传功能
        btn.onclick = function () {
            var process = wrap[0].querySelector('.process-up');
            var spans = wrap[0].querySelectorAll('span');
            var value = form.file.value;

            if (!value) {
                console.log("请选择文件");
                return
            }

            var fd = new FormData(form)

            var xhr = new XMLHttpRequest()

            xhr.open('post', '/upload')

            xhr.onload = function () {
                console.log(xhr.response);
            }

            xhr.onerror = function () {
                console.log('请求失败');
            }

            xhr.upload.onloadstart = function () {
                console.log('准备上传');
            }

            xhr.upload.onprogress = function (ev) {
                // console.log(ev);
                // 计算上传的百分比进度
                var percent = Math.round(ev.loaded / ev.total * 100);
                spans[0].innerHTML = spans[1].innerHTML = percent;
                process.style.width = percent + '%';
            };

            xhr.upload.onloadend = function () {
                console.log('上传完成');
            }

            xhr.send(fd)
        }


        // 下载
        download.onclick = function () {
            var process = wrap[1].querySelector('.process-up');
            var spans = wrap[1].querySelectorAll('span');
            var xhr = new XMLHttpRequest();

            // xhr.open("get", "/images/2.png");

            // ajax 不能请求外站资源
            xhr.open("get", "/images/1.png");

            // 下载文件,需要将返回数据类型设置为 blob 类型
            xhr.responseType = "blob";

            xhr.onload = function () {
                // console.log( xhr.response );
                // URL.createObjectURL() : 获取下载文件的地址
                var url = URL.createObjectURL(xhr.response);

                var img = new Image();
                img.src = url;

                // console.log(img);
                pic.appendChild(img);

            };

            xhr.onerror = function () {
                console.log("请求失败啦!");
            };

            xhr.onloadstart = function () {
                console.log("准备下载...");
            };

            xhr.onprogress = function (ev) {
                // console.log(ev);
                // 计算下载的百分比进度
                var percent = Math.round(ev.loaded / ev.total * 100);
                spans[0].innerHTML = spans[1].innerHTML = percent;
                process.style.width = percent + '%';

            };

            xhr.onloadend = function () {
                console.log("下载完成!");
            };

            xhr.send();
        }
    </script>
</body>

</html>
```























