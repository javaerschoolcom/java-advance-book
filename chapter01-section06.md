# 文件上传和下载

### 文件上传

1.  使用开源上传组件commons-fileupload-1.3.jar,commons-io-2.0.1.jar
2.  把组件放进项目WEB-INF下面的lib目录
3.  把上传组件和项目相互关联
4.  写一个上传表单组件，method必须为post且enctype="multipart/form-data"，上传文件的组件type=“file”
5.  编写一个上传的Servlet类
6.  上传测试

