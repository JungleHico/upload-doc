# 文件上传

## 获取上传文件

```html
<body>
  <input id="uploader" type="file" />

  <script>
    const uploader = document.getElementById('uploader')
    let files = []

    uploader.addEventListener('change', e => {
      files = e.target.files
      const formData = new FormData()
      formData.append('file', files[0])
      // 上传接口
      upload({ file }).then(res => {
        console.log(res.data.url)
      })
    })
  </script>
</body>
```

## File 转 Blob

```js
function fileToBlob (file) {
  return window.URL.createObjectURL(file)
}
```

## File 转 Base64

```js
function fileToBase64 (file) {
  const reader = new FileReader()
  reader.readAsDataURL(file)

  return new Promise((resolve, reject) => {
    reader.addEventListener('load', () => {
      resolve(reader.result)
    })
  })
}
```

## 去除 Base64 头部

```js
base64 = base64.substring(base64.indexOf(',') + 1)
```

## 获取 Base64 文件格式

```js
const type = base64.substring(base64.indexOf(':') + 1, base64.indexOf(';'))
```

## Base64 转 Blob

```js
function base64ToBlob (base64) {
  const type = base64.substring(base64.indexOf(':') + 1, base64.indexOf(';'))
  // 去掉url头，并转化为byte
  const bytes = window.atob(base64.split(',')[1])

  // 处理异常,将ascii码小于0的转换为大于0
  const ab = new ArrayBuffer(bytes.length)
  const ia = new Uint8Array(ab)
  for (let i = 0; i < bytes.length; i++) {
    ia[i] = bytes.charCodeAt(i)
  }

  return new Blob([ab], { type: type })
}
```