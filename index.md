# GlobalSSH产品介绍
&ensp; &ensp;数据传输稳定性一直是海外主机管理不能回避的话题，在主机登录、资料上传时频繁出现卡顿、丢包等现象，直接影响运维效率。

![image](http://globalssh.cn-sh2.ufileos.com/image001.png)
<br /><center>TCP丢包测试（未使用GlobalSSH）</center>

&ensp; &ensp; GlobalSSH旨在解决因为跨国网络不稳定的情况下，通过远程管理服务器时，经常会出现卡顿、连接失败、传输速度较慢等现象。

![image](http://globalssh.cn-sh2.ufileos.com/image003.png) 
<br /><center>TCP丢包测试（使用GlobalSSH）</center>

&ensp; &ensp; 截止目前，产品共有华盛顿、洛杉矶、香港、新加坡、东京、法兰克福6个节点，可覆盖美洲、亚洲、欧洲等大多数海外区域。

# GlobalSSH免费使用规则
新旧计费方式对比：<br />

分类 | 原计费方式 | 新计费方式（免费）
实例 | 60/个 | 0元/个
流量 | 2元/GB | 0元/GB

本次计费方式的变更于2018-11-08上线，自变更之日起：
1.	新创建的实例将执行新的计费方式，即免费使用。
2.	现有实例同样不再计费，用户可继续使用。
3.	为保证免费资源的公平合理性，系统会自动回收自创建之日起7日流量未超过2M的实例。若仍需使用，请进入控制台重新创建。


# 创建与使用GlobalSSH
## 1. 使用控制台创建
### 1.1	从主机或网络列表创建
![image](http://globalssh.cn-sh2.ufileos.com/image005.png) 
<br />（主机列表）


 ![image](http://globalssh.cn-sh2.ufileos.com/image007.png) 
 <br />（网络列表）

&emsp; &emsp; 已购买海外云主机或EIP的用户可点击列表页中的GlobalSSH图标（红框位置）开启GlobalSSH

### 1.2	购买主机时创建
 ![image](http://globalssh.cn-sh2.ufileos.com/image009.png)
 <br /><center>（创建主机GlobalSSH选项）</center>

&emsp; &emsp; 用户购买主机时，GlobalSSH选项已默认开启。用户成功购买海外主机（需绑定外网弹性IP）后，默认获得对应的GlobalSSH

### 1.3	从PathX入口创建
![image](http://globalssh.cn-sh2.ufileos.com/image011.png) 
 <br /><center>（PathX入口）</center>
 
&emsp; &emsp; GlobalSSH同样也适用于第三方云主机，用户可以进入全球动态加速PathX页面，点击GlobalSSH标签，进行创建

## 2	使用CLI（命令行终端）创建

&ensp; &ensp; CLI 基于UCloud Golang SDK，为用户提供简易命令行操作管理云服务产品，降低资源使用成本，目前支持主机、GlobalSSH、EIP等产品的创建、删除、修改等功能。

### 2.1	Homebrew 安装
&ensp; &ensp; Homebrew是Mac、Ubuntu系统支持的软件包管理器，支持大部分的个人工具软件下载，无需运行root权限。

```
brew install ucloud
```
安装过程中若出现报错，请先更新管理器版本解决:

```
brew update
```

其它系统用户下载请参考： github.com/ucloud/ucloud-cli
### 2.2	启动配置 CLI
根据提示，输入UCloud OpenAPI验证信息完成配置

```
ucloud init
```

### 2.3	创建GlobalSSH
假设要为东京的云主机创建GlobalSSH，IP是52.59.198.254， SSH端口8022

```
ucloud gssh create --location Tokyo --target-ip 52.59.198.254 --port 8022
```

成功创建后显示：

```
Succeed, GlobalSSHInstanceId: uga-1t0pqx
```

创建完成后，52.59.198.254.ipssh.net即为加速域名
## 3	使用GlobalSSH
成功创建后，会生成一个加速域名，如：52.59.198.254.ipssh.net。
Linux用户在命令行工具中输入：
```
ssh root@52.59.198.254.ipssh.net
```

Windows用户在远程桌面程序中的计算机处，填写该加速域名，点击连接即可。

# 架构原理
## 关于GlobalSSH
&ensp; &ensp; GlobalSSH是一款保障海外数据中心运维的网络产品，采用了UCloud众多IaaS产品如：ULB4（四层LoadBalance服务）、UDPN（洲际数据中心内网互联、0丢包）及高包量云主机。引入智能DNS服务赋予用户就近接入的能力。网络转发基于成熟稳定的GRE、NAT技术，支持TCP端口（除80、443）四层转发。


 ![image](http://globalssh.cn-sh2.ufileos.com/image013.png)
<br /><center>（GlobalSSH架构原理示意图）</center>

## 关于CLI
&ensp; &ensp; CLI使用命令行开源框架，通过调用OpenAPI SDK软件包实现对UCloud云平台产品资源的调用。在成熟的框架基础上，主要实现命令行结构简洁易理解、与控制台业务逻辑保持一致，让用户通过命令行终端获取和管理云平台资源。

&ensp; &ensp; 使用便利性上，UCloud已经整合到Homebrew工具下载软件包。Mac、Ubuntu系统用户可以直接命令下载CLI 源代码及其依赖环境。Windows系统用户可以访问Git获取源码进行编译：github.com/ucloud/ucloud-cli

- 简洁命令结构、完整 help 提示

&ensp; &ensp; 命令结构设计上，以“ucloud”为主命令，平台产品“uhost”、“eip”为子命令；然后跟上“增”、“删”、“改”、“查”等基础操作，方便用户形成记忆习惯。命令提示信息上，如主机创建等相对复杂的操作，UI设计两版提示，分别适用对业务熟悉程度深浅不同的用户场景，避免信息提示不足、或信息提示过多的矛盾。

“ucloud uhost create -h” 简洁提示信息：

 ![image](http://globalssh.cn-sh2.ufileos.com/image015.png)

“ucloud uhost create - -help” 详细提示信息：

 ![image](http://globalssh.cn-sh2.ufileos.com/image017.png)

- 命令、参数自动补全，及时的操作效果反馈

&ensp; &ensp; 习惯快速敲击命令的工程师们，CLI提供符合一般输入习惯的自动补全功能，极大提高工作效率。该功能可以覆盖部分较长参数的自动补全，建议大家使用时积极尝试。

![image](http://globalssh.cn-sh2.ufileos.com/image019.png)
 
&ensp; &ensp; 考虑UCloud对用户数据安全、隐私的重视，命令补全功能在下载或编译时，没有强制自动更新终端的配置文件，请用户在本地配置文件 ~/.bash_profile or ~/.bashrc 增加自动补全脚本。

&ensp; &ensp; 除了自动补全，CLI提供操作必要的操作过程动态展示和结果反馈。如:
主机重启操作

![image](http://globalssh.cn-sh2.ufileos.com/image021.png) 

命令操作失败时，原因提示：

![image](http://globalssh.cn-sh2.ufileos.com/image023.png) 



