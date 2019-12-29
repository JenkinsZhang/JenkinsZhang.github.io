---
layout:     post
title:      "SpringMVC复习(二)"
subtitle:   "文件的上传与下载"
date:       2019-12-29
author:     "JenkinsZhang"
tags:
    - Java
    - SpringBoot
    - SpringMVC
    - Backend
---
> This document is not completed and will be updated anytime.

## 前言
这是复习的第二篇，主要来讲讲SpringMVC的文件上传与下载，同步的代码可以点[这里](https://github.com/JenkinsZhang/springboot_springcloud_review)

前一篇:[SpringMVC复习(一)](https://jenkinszhang.github.io/2019/12/28/hello-my-blog/)

这里是[合集](https://jenkinszhang.github.io/archive/?tag=SpringMVC)

---
### SpringMVC 复习(一)
>#### 文件的下载

这个不需要前端代码，只需要后端写好，在浏览器打开相关链接就可以看到效果，代码如下：  

```
@GetMapping("/download")
    public String download(HttpServletRequest request, HttpServletResponse response) {
        try {
            // you should change this path when deploying on the server
            String filePath = "/Users/jenkinszhang/PROGRAMMING/Java/New_Backend_review/eureka-service/src/main/java/com/jenkins/eurekaservice/files/downloadFiles/";
            InputStream inputStream = new BufferedInputStream(new FileInputStream(filePath + "/timg.jpeg"));

            //if you want to download other files,please change this line of code
            String filename = "test.jpeg";


            byte[] body = new byte[inputStream.available()];
            inputStream.read(body);
            inputStream.close();
            ServletOutputStream outputStream = response.getOutputStream();
            response.setHeader("Content-Disposition", "attachment;filename=" + filename);
            outputStream.write(body);
            outputStream.flush();


        } catch (IOException e) {
            return "download failed";
        }
        return "success";
    }
```

接下来是解释:  
首先要获取到response，在方法中的参数写上```HttpServletResponse response ```即可  

然后规定你想要浏览器（客户端）获取的文件路径，在这里我写的是我自己项目存放文件的路径，如果到时候要部署到服务器上，这个路径可以根据个人需要进行更改，不过感觉这样蛮麻烦的  

接着获取文件输入流，我这里使用了 ```BufferdInputStream```，获取到文件的byte[]类型数组，都是些常规的IO操作  

最后获取response的输出流，将文件写入到response中，注意response的响应头要进行设置，如:  
```response.setHeader("Content-Disposition", "attachment;filename=" + filename);```  

>Content-Disposition属性有两种类型:  
>&nbsp;&nbsp;&nbsp;&nbsp;1.inline:将文件内容直接显示在页面  
>&nbsp;&nbsp;&nbsp;&nbsp;2.attachment:弹出对话框让用户下载  
>filename:浏览器（客户端）下载的文件名  

在这里我用的是attachment,打开浏览器访问你的controller路径，就可以下载文件了

---
>#### 文件的上传

文件的上传总体来说还是挺方便的，与下载也是大同小异，只需要在方法中加上参数:```MultipartFile file```即可  
代码如下：  
```    
       @PostMapping("/upload")
       public String upload(@RequestParam("file") MultipartFile file, HttpServletRequest request) {
           // you should change this path when deploying on the server
           String filePath = "/Users/jenkinszhang/PROGRAMMING/Java/New_Backend_review/eureka-service/src/main/java/com/jenkins/eurekaservice/files/uploadFiles/";
           if (!file.isEmpty()) {
               try {
                   File dest = new File(filePath, file.getOriginalFilename());
                   if (!dest.exists()) {
                       dest.mkdirs();
                   }
                   file.transferTo(new File(filePath, file.getOriginalFilename()));
               } catch (IOException e) {
                   e.printStackTrace();
               }
           } else {
               return "failed";
           }
   
           return "success";
       }
```
filePath与下载差不多，是存放你上传的文件的文件夹路径，获取到了路径以后，使用```transferTo()```这个方法，将参数中的file文件上传到你指定的文件夹，后端的操作就完成了  
接下来上前端的示例代码，这里我用的是vue+bootstrap，直接把人家组件拉过来用了
```
<template>
	<div>
		<!-- Styled -->
		<b-container>
			<b-row>
				<br><br>
			</b-row>
			<b-row align-v="center" align-h="center">
				<b-col md="6">
					<b-form-file
							v-model="file"
							:state="Boolean(file)"
							placeholder="Choose a file or drop it here..."
							drop-placeholder="Drop file here..."
					
					></b-form-file>
				</b-col>
			</b-row>
			<div class="mt-6">Selected file: {{ file ? file.name : '' }}</div>
			<b-button @click="upload_file" type="submit" variant="primary">summit</b-button>
			<!-- Plain mode -->
			<b-form-file v-model="file2" class="mt-3" plain></b-form-file>
			<div class="mt-3">Selected file: {{ file2 ? file2.name : '' }}</div>
		</b-container>
	</div>
</template>

<script>
    export default {
        name: "upload",
        data() {
            return {
                file: null,
                file2: null
            }
        },
        methods: {
            upload_file() {
                var form = new FormData()
                form.append("file", this.file)
                this.axios({
                    url: "http://localhost:1235/upload",
                    method: 'post',
                    data: form,
                    headers: {
                        'Content-Type': 'multipart/form-data'
                    }
                }).then((res) => {
                    console.log(res.data);
                })

            }
        }
    }
</script>

```
template的代码是直接复制黏贴的，不解释了，说说上传的逻辑  
>使用```var form = new FormData()```创建表单数据，使用将```append```方法将```data```中的```file```传入，并取名为"file"  
>随后使用axios进行上传，设置路径，方法，携带数据，头部信息即可，Content-Type是固定写法（至少我是这样）

如果遇到了跨域问题，那么在java的Controller中加入@CrossOrigin注解即可访问，亲测有效

---

