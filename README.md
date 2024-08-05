# 身份证拍照选择组件（带引导线、图片转Base64） 

目前暂仅支持微信小程序下使用。

通过打开身份证拍照弹窗（带拍照引导框），可选择即时拍照上传或相册上传，组件会返回图片在微信中的临时链接和图片base64字符串。

## 参数 Props

| 参数| 类型 | 说明 | 可选值 | 默认值 |
| --- | --- | --- | --- | --- |
| quality | String | 拍照图片质量 | high (高质量), normal (中等质量), low (低质量)  | normal |
| sizeType | Array | 相册选择图片质量 | original (原图), compressed (压缩图) | ['original', 'compressed'] |

## 事件 Events

| 事件名| 说明 | 回调参数 |
| --- | --- | --- |
| getData | 获得图像信息 | Object(type,base64,url) |
| chooseImageFail | 选择相册图片失败 | Error |
| cameraInitDone | 相机加载完成 | Event |
| cameraInitError | 相机加载失败 | Error |
| takePhotoError | 拍照失败 | Error |
