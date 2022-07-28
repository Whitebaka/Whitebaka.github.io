---
title: 如何在obs里添加悬浮窗
date: 2022-06-08 20:29:58
tags:
---
# 前言
先略过晚点再补
# 设置
## ACT设置
### Step1.确保安装悬浮窗插件（OverlayPlugin.dll)
![](https://whitebaka-1301161068.cos.ap-nanjing.myqcloud.com/image/20220608203202.png)

### Step2.确保悬浮窗WS服务打开
![](https://whitebaka-1301161068.cos.ap-nanjing.myqcloud.com/image/20220608204150.png)

### Step3.获取WS链接
在提供的悬浮窗链接后添加?HOST_PORT=ws://{本地IP}:{端口号}/  
如https://overlays.ffcafe.cn/mopimopi2/?HOST_PORT=ws://127.0.0.1:10501/  
一般的，大多数常用悬浮窗可以直接使用Step2内生成器生成链接。  
<br></br>
至此act部分设置完成。

## Obs设置
### Step1.添加浏览器源，并在url输入刚才的链接。正确输入大小
![](https://whitebaka-1301161068.cos.ap-nanjing.myqcloud.com/image/20220608204741.png)
### Step2.调整悬浮窗参数。
右键创建的浏览器源-》互动，在这个界面可以如同自己电脑上一般操作悬浮窗的各项设置。

## 完成！
进行测试看obs能否正确读取act解析数据即可完成。

# Q&A
暂时没有