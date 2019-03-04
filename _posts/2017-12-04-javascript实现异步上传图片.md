---
layout: post
title: "javascript实现异步上传图片"
date: 2017-12-04
description: "javascript实现异步上传图片"
tag: JavaScript
---

﻿####最近遇到图片上传的需求，由于第一次在移动端尝试写上传，还是小心为上，做了一个简单的demo，便于在此基础之上慢慢完善。

**html代码：**
```
<form id="uploadForm" action="http://storage.test.com/file/upload" method="post" enctype="multipart/form-data">
      <input type="hidden" name="key" id="key" value="VTZ18HM64#D_L3WX" />
      <input type="file" name="uploadFiles" value="" id="fileImage" multiple='multiple' />
      <div class="upload_submit">
        <button type="button" id="fileSubmit" class="upload_btn">保存</button>
        </div>
</form>

```

**js代码：**

```
var Fileupload = {
     fileInput: $("#fileImage").get(0),
     dragDrop: $("#fileDragArea").get(0),
     upButton: $("#fileSubmit").get(0),
	 url: $("#uploadForm").attr("action"),
     })(),
     //文件上传
     funUploadFile: function() {
         var self = this;

         for (var i = 0, file; file = this.fileFilter[i]; i++) {
             (function(file) {
                 var xhr = new XMLHttpRequest();
                 if (xhr.upload) {
                     // 上传中
                     xhr.upload.addEventListener("progress", function(e) {
                         self.onProgress(file, e.loaded, e.total);
                     }, false);
                     // 文件上传成功或是失败
                     xhr.onreadystatechange = function(e) {

                       if (xhr.readyState == 4) {
                             if (xhr.status == 200) {
                                 self.onSuccess(JSON.parse(xhr.responseText));
                                 self.funDeleteFile(file);
                                 if (!self.fileFilter.length) {
                                     //全部完毕
                                     self.onComplete();
                                 }
                             } else {
                                 self.onFailure(file, xhr.responseText);
                             }
                         }
                     };


          //准备FormData对象
          var formData = new FormData();
          //将文件放入FormData对象中
          formData.append('uploadFiles', file);
          // 开始上传
          xhr.open("POST", self.url, true);
          // xhr.setRequestHeader("Content-Type","multipart/form-data");
          xhr.send(formData);
                 }
             })(file);
         }

     },

     init: function() {
         var self = this；
         //上传按钮提交
         if (this.upButton) {
             console.log('提示： 当前存储服务器地址', this.url)
             this.upButton.addEventListener("click", function(e) {
                 self.funUploadFile(e);
             }, false);
         }

         self.bindEvent();

     }
 };
 Fileupload = $.extend(Fileupload);
 Fileupload.init();
```

[类似博文参考](https://segmentfault.com/a/1190000008304339)

转载请注明：[化风的博客](http://ChhXin.github.io) » [javascript实现异步上传图片](/2017/12/javascript实现异步上传图片/)                   
