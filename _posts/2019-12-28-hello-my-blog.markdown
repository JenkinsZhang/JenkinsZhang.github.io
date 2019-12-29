---
layout:     post
title:      "SpringMVC复习(一)"
subtitle:   "@InitBinder注解"
date:       2019-12-28
author:     "JenkinsZhang"
tags:
    - Java
    - SpringBoot
    - SpringMVC
    - Backend
---

> This document is not completed and will be updated anytime.

## 前言
临近毕业设计，选了个Java-Web的系统设计题目，但很多一些后端的东西有点忘记了，所以就打算写点博客复习复习，虽然都是些小东西，但这知识他就是进不去脑子啊，一天一小忘，三天一大忘

大概以后的博客会写成英文的，也大概不定时更新，看我自己有没有空8，反正都是写给自己看的


同步的代码我都放在了[这里](https://github.com/JenkinsZhang/springboot_springcloud_review)

---


#### SpringMVC 复习(一)
>##### @InitBinder

这个注解主要是用来对WebDataBinder进行初始化，用在@Controller注解方法中，注册一个绑定器初始化，但只对本控制器有效

示例代码如下：
```
@InitBinder
    public void initBinder(WebDataBinder binder){
        binder.registerCustomEditor(Date.class,
                new CustomDateEditor(new SimpleDateFormat("yyyy-MM-dd"),
                        false));
    }
```
这里使用@InitBinder注解创建了一个方法，为绑定器添加了一个日期转换编辑器，当然你也可以添加别的东西，比如：

```
binder.setValidator(YOUR_VALIDATOR)
```

这次就记这么多，待会儿再复习一下文件的上传以及下载。

---
