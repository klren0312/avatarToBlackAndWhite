做了个将头像转换成黑白的页面: [https://github.com/klren0312/avatarToBlackAndWhite](https://github.com/klren0312/avatarToBlackAndWhite)

### 代码
```html
  <div class="avatar" id="js-avatar">
    <canvas class="canvas" id="js-canvas" height="300" width="300"></canvas>
  </div>
  <div class="tip hide" id="js-tip">请长按图片保存</div>
  <label for="upload" class="btn">
    上传头像
    <input class="hide" type="file" name="file" id="upload" onchange="uploadFile(this)">
  </label>
  <button class="convert-btn" onclick="convert()">转换为黑白</button>
```

```js
const btn = document.getElementById('js-button')
const tip = document.getElementById('js-tip')
const canvas = document.getElementById('js-canvas')
const ctx = canvas.getContext('2d')
// 上传文件回调, 将图片显示在canvas里
function uploadFile (e) {
  showCanvas()
  const img = e.files[0]
  const url = URL.createObjectURL(img)
  const image = new Image()
  image.src = url
  image.onload = function () {
    ctx.drawImage(image, 0, 0, 300, 300)
  }
  e.value = null
}
// 转换函数, 将当前canvas转换为黑白, 并生成为图片
function convert () {
  const imgData = ctx.getImageData(0, 0, 300, 300)
  const data = imgData.data
  for (var i = 0; i < data.length; i += 4) {
    const average = (data[i + 0] + data[i + 1] + data[i + 2] + data[i + 3]) / 3
    data[i + 0] = average // 红
    data[i + 1] = average // 绿
    data[i + 2] = average // 蓝
  }
  ctx.putImageData(imgData, 0, 0)
  const afterURL = canvas.toDataURL('image/png')
  // 生成图片
  const img = document.createElement('img')
  img.setAttribute('id', 'js-img')
  img.src = afterURL
  document.getElementById('js-avatar').appendChild(img)
  hideCanvas()
}
// 隐藏canvas, 让图片替换, 方便长按保存
function hideCanvas () {
  canvas.classList.add('hide')
  tip.classList.remove('hide')
}
// 显示canvas, 清除canvas画布, 并移除之前生成的图片
function showCanvas () {
  canvas.classList.remove('hide')
  if (document.getElementById('js-img')) {
    document.getElementById('js-img').remove()
  }
  tip.classList.add('hide')
}
```

### 页面截图
![](https://upload-images.jianshu.io/upload_images/2245742-8ea5900d9a8c71e6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
