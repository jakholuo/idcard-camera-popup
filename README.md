# 身份证拍照选择组件（带引导线、图片转Base64） 

目前暂仅支持微信小程序下使用。

通过打开身份证拍照弹窗（带拍照引导框），可选择即时拍照上传或相册上传，组件会返回图片在微信中的临时链接和图片base64字符串。

| 参数| 类型 | 说明 | 可选值 | 默认值 |
| --- | --- | --- | --- | --- |
| quality | String | 拍照图片质量 | high (高质量), normal (中等质量), low (低质量)  | normal |
| sizeType | Array | 相册选择图片质量 | original (原图), compressed (压缩图) | ['original', 'compressed'] |

