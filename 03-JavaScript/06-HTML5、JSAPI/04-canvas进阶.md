## **1.- 图像处理**

### **1.1 像素操作**

canvas 提供了像素操作API 可以对画布的每一个像素进行操作

- getImageData(x,y,widht,height)

- - 获取指定区域的像素数据（矩形区域）
  - x	指定要获取像素的起点x坐标
  - y	指定要获取像素的起点y坐标
  - width / height	获取像素矩形的宽高

返回值： 一个像素信息对象（ImageData对象），该对象中有三个属性： data, width , height

- - data：一个存储所有像素点信息的一维数组。

数组每四个值表示一个像素点信息（每一个像素有 red, green, blue, alpha 四个信息）

- - width： 要获取的像素区域宽度
  - height： 要获取的像素区域高度

- 1.1.2.putImageData(ImageData, x, y);

将ImageData数据添加到画布的指定区域（以矩形格式添加），三个参数分别是：

- 第一个参数：ImageData像素数据
- 第二个参数：起点的X坐标
- 第三个参数：起点的Y坐标

## **2.视频处理**

### **2.1.绘制视频**

canvas 会获取 video 元素当前帧画面作为图片绘制。

视频绘制与图像绘制相同，也是使用的 drawImage() 方法。

跟图片一样，要绘制视频，需要等待视频加载之后才可以绘制。不同的是，不需要等待视频资源完全加载完成，可以在 canplay 事件或 loadedmetadata 事件触发之后来绘制视频画面。

### **2.2.同步绘制视频**

canvas播放视频的原理是在视频播放的同时，同步获取当前帧画面并绘制到画布中。

#### **2.2.1.requestAnimationFrame() / cancelAnimationFrame()**

HTML5提供了两个新的动画函数，功能与定时器相同，但性能比定时器更好。

requestAnimationFrame() ：当浏览器在下一次重绘时自动调用的方法。

requestAnimationFrame() 方法与setTimeout() 一样，只会执行一次回调函数。要实现连续动画，需要在回调函数中递归调用 requestAnimationFrame();

cancelAnimationFrame() : 取消 requestAnimationFrame() 方法调用。

#### **2.2.2.同步绘制视频**

- 注意：canvas 本身无法播放音频。

### **2.3.视频截图**

canvas.toDataURL(type, encoderOptions): 将canvas导出为base64格式图片

type: 图片格式，默认值为： image/png，可能的值有： image/jpeg, image/webp 等，具体支持的图片格式受浏览器限制

encoderOptions: 图片压缩质量，取值范围为 0~1 之间的小数，如果参数非法，默认为 0.92。