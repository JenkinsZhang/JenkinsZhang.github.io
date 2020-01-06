---
layout:     post
title:      "SpringMVC复习(三)"
subtitle:   "多文件的上传"
date:       2020-1-6
author:     "JenkinsZhang"
tags:
    - Java
    - SpringBoot
    - SpringMVC
    - Backend
---

## 前言
这是复习的第三篇，对上一片文章进行了一点点的补充，同步的代码可以点[这里](https://github.com/JenkinsZhang/springboot_springcloud_review)

前一篇:[SpringMVC复习(二)](https://jenkinszhang.github.io/2019/12/29/spring-mvc-review2/)

这里是[合集](https://jenkinszhang.github.io/archive/?tag=SpringMVC)

---
### SpringMVC 复习(二)

>#### 多文件的上传  
 
多文件的上传与上一篇所讲的单文件上传基本完全一致，只是今天突然用到了就再复习一下，下面是代码：
```
@PostMapping("/upload")
    public String upload(MultipartRequest request, @RequestParam("target") String target) {
        // you should change this path when deploying on the server
        List<MultipartFile> files = request.getFiles("file");
        String filePath = "/Users/jenkinszhang/PROGRAMMING/Java/New_Backend_review/eureka-service/src/main/java/com/jenkins/eurekaservice/files/uploadFiles/";
        System.out.println(target);
        for (MultipartFile file : files) {
            System.out.println(file.getName());
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
        }

        return "success";
    }
```
可以注意到，方法中的参数由原来的MultipartFile 变成了MultipartRequest 来获取携带文件传输的request对象。  
随后我们就可以通过```request.getFiles("file")```方法来获取文件的List对象，随后遍历这个List就可以获取到我们想要的文件了。  
>注意：这里的文件key名"file"是前端所规定的，比如说前端是这么写的:  
>form.append("file",this.file1)  
>form.append("file",this.file2)  
>那么"file"这个key下就有两个文件，使用```request.getFiles("file")```就可以获取到具有两个MultipartFile类型对象的List
