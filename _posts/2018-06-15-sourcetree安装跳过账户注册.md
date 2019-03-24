---
layout: post
title: sourcetree安装跳过账户注册
categories: 软件&网站
description: sourcetree安装跳过账户注册
keywords: source tree, git
---

## source tree 安装跳过账户注册

### 1.点击安装包安装，到第二步注册账号时，退出程序

[![img](https://4.bp.blogspot.com/-7KYRoNhV554/WyPJ16UuAEI/AAAAAAAAAC0/Dc2lsGRRPoY7vok8WqHCdFGGOLuXpGSgQCEwYBhgL/s640/1.png)](https://4.bp.blogspot.com/-7KYRoNhV554/WyPJ16UuAEI/AAAAAAAAAC0/Dc2lsGRRPoY7vok8WqHCdFGGOLuXpGSgQCEwYBhgL/s1600/1.png)

[![img](https://1.bp.blogspot.com/-CrMTKkF85ys/WyPJ5GgmokI/AAAAAAAAAC8/Mctpn8XwgjY_PPUrrRMH5O3Px3odPxcDQCEwYBhgL/s640/2.png)](https://1.bp.blogspot.com/-CrMTKkF85ys/WyPJ5GgmokI/AAAAAAAAAC8/Mctpn8XwgjY_PPUrrRMH5O3Px3odPxcDQCEwYBhgL/s1600/2.png)





### 2.进入电脑文件夹：C:\Users\{电脑账号用户名}\AppData\Local\Atlassian\SourceTree

[![img](https://1.bp.blogspot.com/-T9d65d9z2bw/WyPJ4waDXvI/AAAAAAAAADU/dZEQyOCG-W4EmHimt_ABmJw3ypY1ydc9wCPcBGAYYCw/s640/3.png)](https://1.bp.blogspot.com/-T9d65d9z2bw/WyPJ4waDXvI/AAAAAAAAADU/dZEQyOCG-W4EmHimt_ABmJw3ypY1ydc9wCPcBGAYYCw/s1600/3.png)





### 3.新建文件accounts.json， 添加如下代码

[

  {

​    "$id": "1",

​    "$type": "SourceTree.Api.Host.Identity.Model.IdentityAccount, SourceTree.Api.Host.Identity",

​    "Authenticate": true,

​    "HostInstance": {

​      "$id": "2",

​      "$type": "SourceTree.Host.Atlassianaccount.AtlassianAccountInstance, SourceTree.Host.AtlassianAccount",

​      "Host": {

​        "$id": "3",

​        "$type": "SourceTree.Host.Atlassianaccount.AtlassianAccountHost, SourceTree.Host.AtlassianAccount",

​        "Id": "atlassian account"

​      },

​      "BaseUrl": "https://id.atlassian.com/"

​    },

​    "Credentials": {

​      "$id": "4",

​      "$type": "SourceTree.Model.BasicAuthCredentials, SourceTree.Api.Account",

​      "Username": "",

​      "Email": null

​    },

​    "IsDefault": false

  }

]

[![img](https://2.bp.blogspot.com/-bCGvZQN7PS0/WyPJ7NwOEEI/AAAAAAAAADY/T9CrguE0smQ8KL9kneoOnJ_eKGDHdmdLQCPcBGAYYCw/s640/4.png)](https://2.bp.blogspot.com/-bCGvZQN7PS0/WyPJ7NwOEEI/AAAAAAAAADY/T9CrguE0smQ8KL9kneoOnJ_eKGDHdmdLQCPcBGAYYCw/s1600/4.png)



[![img](https://1.bp.blogspot.com/-FnQwfNmb8cE/WyPJ-PNlHbI/AAAAAAAAADg/erGjVeRo9nkg9el2udAbsG2H3o6yV-1oQCPcBGAYYCw/s640/5.png)](https://1.bp.blogspot.com/-FnQwfNmb8cE/WyPJ-PNlHbI/AAAAAAAAADg/erGjVeRo9nkg9el2udAbsG2H3o6yV-1oQCPcBGAYYCw/s1600/5.png)

###  4.保存关闭，重新打开安装程序

[![img](https://2.bp.blogspot.com/-IfjJEYiJVY4/WyPJ8l4r5JI/AAAAAAAAADc/qBourF7Zy7ouL7x07MyVLEPzunL_qyVJgCPcBGAYYCw/s1600/6.png)](https://2.bp.blogspot.com/-IfjJEYiJVY4/WyPJ8l4r5JI/AAAAAAAAADc/qBourF7Zy7ouL7x07MyVLEPzunL_qyVJgCPcBGAYYCw/s1600/6.png)



[![img](https://3.bp.blogspot.com/-TQqEEGX_C80/WyPJ-3yf1WI/AAAAAAAAADk/oItoWbWjXT0Jzn0gSGxtc8zN_HqJTea6ACPcBGAYYCw/s1600/7.png)](https://3.bp.blogspot.com/-TQqEEGX_C80/WyPJ-3yf1WI/AAAAAAAAADk/oItoWbWjXT0Jzn0gSGxtc8zN_HqJTea6ACPcBGAYYCw/s1600/7.png)



[![img](https://4.bp.blogspot.com/-h4iX0RSe-Vg/WyPJ_OmJPDI/AAAAAAAAADk/0_gPrw18WwwigOwPbCKKPUmU5ec1fDLvACPcBGAYYCw/s640/8.png)](https://4.bp.blogspot.com/-h4iX0RSe-Vg/WyPJ_OmJPDI/AAAAAAAAADk/0_gPrw18WwwigOwPbCKKPUmU5ec1fDLvACPcBGAYYCw/s1600/8.png)



ok！！