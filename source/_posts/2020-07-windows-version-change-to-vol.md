---
title: Windows零售版转换成vol批量可激活版
categories:
  - 玩系统
tags:
  - Windows
toc: true
comments: true
keywords: '当前Windows不是VL版，无法使用KMS激活！'
description: '如果没有购买正版Windows，或者不想用电脑厂商出厂带的OEM版「一般是家庭版，有些功能没有」，重装后我们一般会找个KMS激活，但如果装的时候没选好版本，可能用KMS工具激活的时候会提示该系统非vl版，激活失败之类的。'
date: 2020-07-08 15:48:19
updated: 2020-07-08 15:48:19
top:
---
# 前言
如果没有购买正版Windows，或者不想用电脑厂商出厂带的OEM版「一般是家庭版，有些功能没有」，重装后我们一般会找个KMS激活，但如果装的时候没选好版本，可能用KMS工具激活的时候会提示该系统非vl版，激活失败之类的。
![](../images/windows_vl_kms.png)

# 解决
如果装的系统不是 `VL` 版本，此时你也许会重新下载个 `VL` 版重装，这当然是可以的。

另外一方面，我们也不需要这么麻烦，因为系统镜像本身就大同小异，只是里面一两个配置不一样而已，我们只需要 用 `DISM` 命令将已经装好的系统更改为 `VOL` 版本，当然也可以用这个命令从 `Home` 升级为 `Professional` 。

## 确认已安装的版本
在CMD或者Powershell中，用下面的命令可以知道自己当前已经安装的是什么版本：
```
DISM /online /Get-CurrentEdition
```

运行结果是这样：
```
Windows PowerShell
Copyright (C) 2012 Microsoft Corporation. All rights reserved.

PS C:\Users\Administrator> DISM /online /Get-CurrentEdition

Deployment Image Servicing and Management tool
Version: 6.2.9200.22807

Image Version: 6.2.9200.22807

Current edition is:

Current Edition : ServerStandard

The operation completed successfully.
```

以及可以升级到什么版本
```
DISM /online /Get-TargetEditions
```

## 准备秘钥
密钥可以网上搜索，找个自己已安装版本对应的密钥，可以到微软官网 [Appendix A: KMS Client Setup Keys](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj612867(v=ws.11)) 去找个，因为反正目的都是为了KMS激活。

以自己vps里装的一个为例，通过上面的命令可知版本是 `Windows Server 2012 Server Standard` ，那么从上面的官网上可以找到可用密钥为：`XC9B7-NBPP2-83J2H-RHMBY-92BT4`

## 转换成VL版
转换命令：
```
DISM /online /Set-Edition:<edition ID> /ProductKey:XXXXX-XXXXX-XXXXX-XXXXX-XXXXX /AcceptEula 
```
`Edition ID` 就是前文得到的那个版本 `ServerStandard`，`ProductKey` 后面把序列号填上去就行，这个方法可以解决 `slmgr /ipk` 命令无法安装 `VL` 密钥的情况，也适用于 `Standard` 转 `DataCenter`

按实际情况组合完为：
```
DISM /online /Set-Edition:ServerStandard /ProductKey:XC9B7-NBPP2-83J2H-RHMBY-92BT4 /AcceptEula
```

命令执行完会要求重启，按要求重启系统。

# 系统激活
转换为 `VL` 版后，再去激活系统就简单多了，市面上几乎所有的工具都能一键激活，KMS工具激活就不提了，都在界面上，自行点点就激活完成了，不想用工具的可以用以下命令：
```
#设置kms服务器
slmgr /skms kms.03k.org
#自动激活
slmgr /ato
```

# 参考文档
1. [Windows Server 2012从Evaluation版转成正式版](https://www.cnblogs.com/zjoch/archive/2013/04/02/2995250.html)
2. [KMS服务~一句命令激活windows/office](https://03k.org/kms.html)