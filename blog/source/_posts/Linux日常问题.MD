---
title: Linux日常使用遇到的小问题
date: 2023-11-08 06:32:20
---

# 1.换新设备时，拉取github/gitee项目
1. 生成ssh key
```
ssh-keygen -t rsa -b 4096 -C "anicaazhu@gmail.com"
```

2. 复制该public key
```
kate ~/.ssh/id_rsa.pub
```

3. 黏贴到github中的ssh部分，创建一个新的ssh即可。

4. 配置一下global/local的鉴权信息
```
git config --global user.email "your email"
git config --global user.name "your name"
```



# 2. 全角标点启用和禁用

> 有效的解决了每次写笔记的时候, .和。需要切换。

```
ctrl + . 开启关闭
```

可以在输入法的附加设置中的标点中看到。



# 3. 蓝牙开机后处于禁用状态

```kate
kate /etc/bluetooth/main.conf
```

找到Autoenable, 取消注释, 改为true.