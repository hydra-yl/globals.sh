---
layout: default
title: "GlobalSSH"
---

# GlobalSSH产品介绍
&ensp; &ensp;数据传输稳定性一直是海外主机管理不能回避的话题，在主机登录、资料上传时频繁出现卡顿、丢包等现象，直接影响运维效率。


```
--- 152.32.140.27 hping statistic ---
20 packets tramitted, 18 packets received, 10% packet loss
round-trip min/avg/max = 424.1/1586.3/7856.4 ms
```
<center>TCP丢包测试（未使用GlobalSSH）</center>

&ensp; &ensp; GlobalSSH旨在解决因为跨国网络不稳定的情况下，通过远程管理服务器时，经常会出现卡顿、连接失败、传输速度较慢等现象。

```
--- 152.32.140.27.ipssh.net hping statistic ---
20 packets tramitted, 20 packets received, 0% packet loss
round-trip min/avg/max = 276.1/530.4/1100.9 ms
``` 
<center>TCP丢包测试（使用GlobalSSH）</center>

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
![image](https://globalssh.io/image005.png) 
<br />（主机列表）


 ![image](https://globalssh.io/image007.png) 
 <br />（网络列表）

&emsp; &emsp; 已购买海外云主机或EIP的用户可点击列表页中的GlobalSSH图标（红框位置）开启GlobalSSH

### 1.2	购买主机时创建
 ![image](https://globalssh.io/image009.png)
 <br /><center>（创建主机GlobalSSH选项）</center>

&emsp; &emsp; 用户购买主机时，GlobalSSH选项已默认开启。用户成功购买海外主机（需绑定外网弹性IP）后，默认获得对应的GlobalSSH

### 1.3	从PathX入口创建
![image](https://globalssh.io/image011.png) 
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


 ![image](https://globalssh.io/image013.png)
<br /><center>（GlobalSSH架构原理示意图）</center>

## 关于CLI
&ensp; &ensp; CLI使用命令行开源框架，通过调用OpenAPI SDK软件包实现对UCloud云平台产品资源的调用。在成熟的框架基础上，主要实现命令行结构简洁易理解、与控制台业务逻辑保持一致，让用户通过命令行终端获取和管理云平台资源。

&ensp; &ensp; 使用便利性上，UCloud已经整合到Homebrew工具下载软件包。Mac、Ubuntu系统用户可以直接命令下载CLI 源代码及其依赖环境。Windows系统用户可以访问Git获取源码进行编译：github.com/ucloud/ucloud-cli

- 简洁命令结构、完整 help 提示

&ensp; &ensp; 命令结构设计上，以“ucloud”为主命令，平台产品“uhost”、“eip”为子命令；然后跟上“增”、“删”、“改”、“查”等基础操作，方便用户形成记忆习惯。命令提示信息上，如主机创建等相对复杂的操作，UI设计两版提示，分别适用对业务熟悉程度深浅不同的用户场景，避免信息提示不足、或信息提示过多的矛盾。

“ucloud uhost create -h” 简洁提示信息：

```
$ ucloud uhost create -h
Usage:
 ucloud uhost create [flags]
 flags may be         --async | --cpu | --memory-gb | --password | --image-id | --vpc-id |
                      --subnet-id | --name | --bind-eip | --create-eip-line |
                      --create-eip-bandwidth-mb | --create-eip-charge-mode | --create-eip-name
                      | --create-eip-remark | --create-eip-coupon-id | --charge-type |
                      --quantity | --project-id | --region | --zone | --type |
                      --net-capability | --os-disk-type | --os-disk-size-gb |
                      --os-disk-backup-type | --data-disk-type | --data-disk-size-gb |
                      --data-disk-backup-type | --firewall-id | --group | --debug | --json | --help

Use "ucloud uhost create --help" for details.
```


“ucloud uhost create - -help” 详细提示信息：

```
$ ucloud uhost create --help
Usage:

  ucloud uhost create [flags]

Flags:

  --async                               Optional. Do not wait for the long-running operation
                                        to finish.

  --cpu     int                         Required. The count of CPU cores. Optional parameters:
                                        {1, 2, 4, 8, 12, 16, 24, 32} (default 4)

  --memory-gb     int                   Required. Memory size. Unit: GB. Range: [1, 128],
                                        multiple of 2 (default 8)

  --password     string                 Required. Password of the uhost user(root/ubuntu)

  --image-id     string                 Required. The ID of image. see 'ucloud image list'

  --vpc-id     string                   Optional. VPC ID. This field is required under VPC2.0.
                                        See 'ucloud vpc list'

  --subnet-id     string                Optional. Subnet ID. This field is required under
                                        VPC2.0. See 'ucloud subnet list'

  --name     string                     Optional. UHost instance name (default "UHost")

  --bind-eip     string                 Optional. Bind eip to uhost. Value could be resource
                                        id or IP Address

  --create-eip-line     string          Optional. Required if you want to create new EIP. Line
                                        of created eip to bind with the uhost

  --create-eip-bandwidth-mb     int     Optional. Required if you want to create new EIP.
                                        Bandwidth(Unit:Mbps).The range of value related to
                                        network charge mode. By traffic [1, 200]; by bandwidth
                                        [1,800] (Unit: Mbps); it could be 0 if the eip belong
                                        to the shared bandwidth
                                        
  --create-eip-charge-mode     string   Optional. 'Traffic','Bandwidth' or 'ShareBandwidth'
                                        (default "Bandwidth")

  --create-eip-name     string          Optional. Name of created eip to bind with the uhost

  --create-eip-remark     string        Optional.Remark of your EIP.

  --create-eip-coupon-id     string     Optional.Coupon ID, The Coupon can deducte part of the
                                        payment,see https://accountv2.ucloud.cn

  --charge-type     string              Optional.'Year',pay yearly;'Month',pay
                                        monthly;'Dynamic', pay hourly(requires access)
                                        (default "Month")

  --quantity     int                    Optional. The duration of the instance. N
                                        years/months. (default 1)  
                                        
  --project-id     string               Optional. Assign project-id (default "org-ejcxl3")

  --region     string                   Optional. Assign region (default "cn-bj2")

  --zone     string                     Optional. Assign availability zone

  --type     string                     Optional. Default is 'N2' of which cpu is V4 and sata
                                        disk. also support 'N1' means V3 cpu and sata
                                        disk;'I2' means V4 cpu and ssd disk;'D1' means big
                                        data model;'G1' means GPU type, model for K80;'G2'
                                        model for P40; 'G3' model for V100 (default "N2")

  --net-capability     string           Optional. Default is 'Normal', also support 'Super'
                                        which will enhance multiple times network capability
                                        as before (default "Normal")

  --os-disk-type     string             Optional. Enumeration value. 'LOCAL_NORMAL', Ordinary
                                        local disk; 'CLOUD_NORMAL', Ordinary cloud disk;
                                        'LOCAL_SSD',local ssd disk; 'CLOUD_SSD',cloud ssd
                                        disk; 'EXCLUSIVE_LOCAL_DISK',big data. The disk only
                                        supports a limited combination. (default "LOCAL_NORMAL")

  --os-disk-size-gb     int             Optional. Default 20G. Windows should be bigger than
                                        40G Unit GB (default 20)

  --os-disk-backup-type     string      Optional. Enumeration value, 'NONE' or 'DATAARK'.
                                        DataArk supports real-time backup, which can restore
                                        the disk back to any moment within the last 12 hours.
                                        (Normal Local Disk and Normal Cloud Disk Only)
                                        (default "NONE")

  --data-disk-type     string           Optional. Enumeration value. 'LOCAL_NORMAL', Ordinary
                                        local disk; 'CLOUD_NORMAL', Ordinary cloud disk;
                                        'LOCAL_SSD',local ssd disk; 'CLOUD_SSD',cloud ssd
                                        disk; 'EXCLUSIVE_LOCAL_DISK',big data. The disk only
                                        supports a limited combination. (default "LOCAL_NORMAL")

  --data-disk-size-gb     int           Optional. Disk size. Unit GB (default 20)

  --data-disk-backup-type     string    Optional. Enumeration value, 'NONE' or 'DATAARK'.
                                        DataArk supports real-time backup, which can restore
                                        the disk back to any moment within the last 12 hours.
                                        (Normal Local Disk and Normal Cloud Disk Only)
                                        (default "NONE")

  --firewall-id     string              Optional. Firewall Id, default: Web recommended
                                        firewall. see 'ucloud firewall list'.

  --group     string                    Optional. Business group (default "Default")

  --help, -h                            help for create

Global Flags:

  --debug, -d   Running in debug mode

  --json, -j    Print result in JSON format whenever possible  
```

- 命令、参数自动补全，及时的操作效果反馈

&ensp; &ensp; CLI提供符合一般输入习惯的自动补全功能，极大提高工作效率。该功能可以覆盖部分较长参数的自动补全，建议大家使用时积极尝试。


```
$ ucloud uhost
Usage: [command]

Commands:

  list           List all UHost Instances

  create         Create UHost instance

  delete         Delete Uhost instance

  stop           Shut down uhost instance

  start          Start Uhost instance

  restart        Restart uhost instance

  poweroff       Analog power off Uhost instnace

  resize         Resize uhost instance,such as cpu core count, memory size and disk size

  clone          Create an uhost with the same configuration as another uhost

  reset-password Reset the administrator password for the UHost instances.

  reinstall-os   Reinstall the operating system of the UHost instance

  create-image   Create image from an uhost instance

Flags:

  --help, -h   help for uhost

Global Flags:

  --debug, -d   Running in debug mode

  --json, -j    Print result in JSON format whenever possible

Use "ucloud uhost [command] --help" for more information about a command.
```
 
&ensp; &ensp; 鉴于隐私与数据安全，命令补全功能在下载或编译时，没有强制自动更新终端的配置文件，请用户在本地配置文件 ~/.bash_profile or ~/.bashrc 增加自动补全脚本。

&ensp; &ensp; 除了自动补全，CLI提供操作必要的操作过程动态展示和结果反馈。如:
主机重启操作

```
$ ucloud uhost restart --uhost-id uhost-5ar4iv
UHost:[uhost-5ar4iv] is restarting...done
```


&ensp; &ensp; 命令操作失败时，原因提示：


```
$ ucloud uhost create --cpu 1
Error: required flag(s) "password", "image-id" not set
Usage:
 ucloud uhost create [flags]
 flags may be         --async | --cpu | --memory-gb | --password | --image-id | --vpc-id |
                      --subnet-id | --name | --bind-eip | --create-eip-line |
                      --create-eip-bandwidth-mb | --create-eip-charge-mode | --create-eip-name
                      | --create-eip-remark | --create-eip-coupon-id | --charge-type |
                      --quantity | --project-id | --region | --zone | --type |
                      --net-capability | --os-disk-type | --os-disk-size-gb |
                      --os-disk-backup-type | --data-disk-type | --data-disk-size-gb |
                      --data-disk-backup-type | --firewall-id | --group | --debug | --json | --help

Use "ucloud uhost create --help" for details.
```
