#### html5图片上传
##### 原始图片上传
前端图片上传，原始的方法之一是使用ajaxfileupload。但是ajaxfileupload会使原来的控件事件消失，所以需要重新绑定。

```
 complete: function (data, status) {
    $('#upload').change(function () {
            uploadFileToServer();
            });
}
```
##### 压缩图片的上传
前端图片压缩上传的思路是，通过FileReader读取文件，然后裁剪到一个隐藏的Image标签上，然后通过HTML5的Canvas的toDataURL方法将图片转换给Base64格式的字符串，然后上传。

**注意：** 在调用toDataURL方法是，一定要在image的onLoad函数里面进行，否则在Safari为基础的iOS网页端，会出现上传的图片是空或者全黑

```
mpImg.render(resImg, { maxWidth: 500, maxHeight: 500, quality: 1 }, function () {
    resImg.onload = function () {
    var canvas = document.createElement("canvas");
    canvas.width = resImg.width;
    canvas.height = resImg.height;
    var ctx = canvas.getContext("2d");
    ctx.drawImage(resImg, 0, 0, resImg.width, resImg.height);
    var dataURL = canvas.toDataURL("image/jpeg");
    uploadBase64Image(dataURL);
    };
});
```

