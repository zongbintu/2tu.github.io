title: 使用FeignClient上传文件  
tags: [FeignClient,feign-form,FormEncoder]  
date: 2019-11-22 21:05:17  
categories: Java
---
####  问题
使用FeignClient上传文件时居然失败了？意外，不敢相信SpringCloud中的组件不可能不支持该功能   
版本  
SpringCloud: Dalston.Release  
feign: 9.4.0  

期间有各种报错，如：
FileUploadException: the request was rejected because no multipart boundary was found  
MissingServletRequestPartException: Required request part 'file' is not present  

####  原因
因为feign 9.4.0默认并不支持multipart/form（文件上传），需要手动引入feign-form、feign-form-spring  

#### 使用方式  
引入feign-form依赖 
 
``` 
<dependencies>
  <dependency>
    <groupId>io.github.openfeign.form</groupId>
    <artifactId>feign-form</artifactId>
    <version>3.3.0</version>
  </dependency>
  <dependency>
    <groupId>io.github.openfeign.form</groupId>
    <artifactId>feign-form-spring</artifactId>
    <version>3.3.0</version>
  </dependency>
</dependencies>
```
  
  
改造上传文件FeignClient  
<!-- more -->

```
@FeignClient(
    name = "file-upload-service",
    configuration = FileUploadServiceClient.MultipartSupportConfig.class
)
public interface FileUploadServiceClient extends IFileUploadServiceClient {

  public class MultipartSupportConfig {

    @Autowired
    private ObjectFactory<HttpMessageConverters> messageConverters;

    @Bean
    public Encoder feignFormEncoder () {
    	// 支持文件上传FormEncoder及之前默认的其它Encoder
      return new SpringFormEncoder(new SpringEncoder(messageConverters));
    }
  }
}
```


到此已可用   


#### 不同版本选择  
上文中采用的feign-form版本是与feign版本是对应的。如果版本对应错误，是不兼容的。 
 
| spring-cloud | feign | feign-form |
| -------- | ----- | ---- | ---- |
| Dalston    | 9.4.0  | 3.5.0以前（20191122最新：3.4.1） |
| Greenwich        | 10.4.0  | 3.5.0以后（20191122最新：3.8.0） |

####  MultipartFile接收时找不到问题  
MissingServletRequestPartException: Required request part 'file' is not present  
正常情况下可能是指定的文件名不对，其它的可能不谈，只谈我在使用过程中遇到问题  
用户端：  上传文件  
Server1：  接收用户上传文件  
Server2：  接收服务器端1上传文件  
1、Server2代码示例  

```
@RequestMapping(value = "upload", method = RequestMethod.POST,
        consumes = {MediaType.MULTIPART_FORM_DATA_VALUE})
@ResponseBody
public void checkin(@RequestPart("file") MultipartFile file) {
			// do something
}

```


2、Server1代码示例  


```
@RequestMapping(value = "upload", method = RequestMethod.POST,
        consumes = {MediaType.MULTIPART_FORM_DATA_VALUE})
@ResponseBody
public RespBody checkin(@RequestPart("file1") MultipartFile file1) {
			Server2FeignClient.upload(file1);
        return RespBody.ok();
}

```


Server2FeignClient关键代码


``` 
void upload(@RequestPart("file") MultipartFile file);
```

结果就是Server2可以接收到请求，但是收不到file文件，此时我们查看Server1中的MultipartFile对象发现，虽然已经在Server2FeignClient中设置MultipartFile 的key为file，但是Server2收到的仍然是file1。

解决方法:  通过反映方式将MultipartFile的fieldName改为Server2需要的key


```
Field partField = multipartFile.getClass().getDeclaredField("part");
partField.setAccessible(true);
ApplicationPart part = (ApplicationPart) partField.get(multipartFile);
Field fileItemField = part.getClass().getDeclaredField("fileItem");
fileItemField.setAccessible(true);
org.apache.tomcat.util.http.fileupload.disk.DiskFileItem fileItem = (org.apache.tomcat.util.http.fileupload.disk.DiskFileItem) fileItemField.get(part);
fileItem.setFieldName("file");
```


##### 引用 
[https://github.com/OpenFeign/feign-form](https://github.com/OpenFeign/feign-form)