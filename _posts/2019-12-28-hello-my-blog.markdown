---
layout:     post
title:      "Example Post"
subtitle:   "SpringMVC复习(一)"
date:       2019-12-28
author:     "JenkinsZhang"
tags:
    - Java
    - SpringBoot
    - SpringMVC
---

> This document is not completed and will be updated anytime.

## 前言
临近毕业设计，选了个Java-Web的系统设计题目，但很多一些后端的东西有点忘记了，所以就打算写点博客复习复习，反正都是些小东西^_^


同步的代码我都放在了[这里](https://github.com/JenkinsZhang/springboot_springcloud_review)

---


#### SpringMVC 复习(一)

这个注解主要是用来对WebDataBinder进行初始化，用在@Controller注解方法中注册一个绑定器初始化，但只对本控制器有效

示例代码如下：
```
@InitBinder
    public void initBinder(WebDataBinder binder){
        binder.set
    }
```
---
