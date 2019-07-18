# 操作指南

{{indexmenu_n>2}}

## 创建物理主机

在UPHost管理控制台，点击创建主机：

在弹出页面中，选择云主机的镜像、机型、网络、付费方式、数量：

![image](/images/create_uphost.png)

## 物理云主机管理

对于您的每一台物理云主机，都可以进行常规的启动、关闭、重启、重装、删除等基本操作。这些操作都支持批量处理。

![image](/images/uphost_detail.jpg)

## 信息修改

主机名和备注信息都可以进行编辑修改；

## 业务组

您可以对主机进行分组，使得某些操作以业务组为单位进行，省去了批量操作的繁琐，便于集中管理。
在主机信息列表的业务组名称这一栏，您可以对主机设置分组，组名相同的主机即被分在一个组。如查看监控数据时，就可以以组为单位进行筛选；建立规则或者修改规则时，规则的对象也可以是组。

![image](/images/business_group.png)

## 重装系统

选中要重装系统的主机，确定该主机为“关机”状态，然后点击重装系统按钮。填写要更改的系统镜像名称和管理员密码描述，点击确定即可创建重装系统：

![image](/images/reinstall_uphost_1.png)

输入系统镜像名称和管理员密码之后，点击确定按钮，即可生成新的主机系统，此时页面即跳转到物理云主机管理页面

![image](/images/uphost_list.png)

## 紧急登录

紧急登录功能有别于云主机提供的VNC登录功能，诣在物理机遇到故障时，您可以第一时间登录机器排查问题。当然，您也可以用此功能取代SSH或跳板机的登录方式。

注意：

1）部分物理机机型不支持此功能。若发生物理机宕机，网络不通等情况，请第一时间直接联系技术支持。

2）暂不支持北京一可用区A与上海可用区A（金融云）的物理云主机。

* 配置java环境

请访问java官网，按照
[说明文件](http://java.com/zh_CN/download/help/ie_online_install.xml)
安装最新的Java运行环境。

* 配置java安全名单

以Windows为例。打开控制面-\>选择Java-\>安全选项卡-\>编辑站点列表。

![image](/images/login1.png)

![image](/images/login2.png)

根据您物理机的所在可用区，配置以下地址为例外站点：

| 地域  | 可用区  | IP                     |
| --- | ---- | ---------------------- |
| 北京二 | 可用区B | <http://123.59.90.26>  |
| 北京二 | 可用区C | <http://106.75.0.135>  |
| 北京二 | 可用区D | <http://125.59.130.41> |
| 广东  | 可用区B | <http://106.75.128.62> |
| 香港  | 可用区A | <http://23.91.96.36>   |

* 控制台登录

在控制台菜单选择：紧急登录。此时会下载jnlp文件，若您已经配置了java环境，此时可以打开此文件，正常登录物理主机。

![image](/images/login3.png)

请忽视相关的安全提示。

![image](/images/login4.png)

为了更优质的使用体验，推荐使用Firefox浏览器或Chrome浏览器进行登录操作。

## 监控

物理云主机支持监控功能，监控指标与云主机一致。

但是，请安装监控代理（UCloud Monitor Agent）。具体步骤参考
[监控代理说明文档](..//../management_monitor/umon/guide/agent)

备注：Windows物理云主机暂不支持此功能。

## 1080Ti安装温度监控

由于NVidia 1080Ti是游戏级GPU，在过热保护方面没有硬件级的保护。因此推荐安装UMA，进行温度监控。

一、安装NVIDIA驱动

1、下载最新驱动：

    wget  http://ucloud.mirrors.ucloud.cn/ubuntu/upm_monitor/NVIDIA-Linux-x86_64-384.90.run

2、安装依赖包：

    ubuntu:    apt-get install gcc make
    yum:    yum install gcc make

3、安装NVIDIA驱动

``` 
chmod 775 NVIDIA-Linux-x86_64-384.90.run
./NVIDIA-Linux-x86_64-384.90.run

按提示accept/OK/Yes 等完成安装

```

4、测试是否正常

    nvidia-smi

二、安装uma

官方文档：<https://docs.ucloud.cn/management_monitor/umon/agent>

告警设置在监控-\>监控模板中，可额外添加GPU温度项目。推荐将告警阈值设置为90℃

## 外网访问配置

> 因架构原因，在香港可用区，无法像其他的物理机资源一样进行弹性IP的绑定，若您希望开放对香港物理机的外网访问，可以通过配置虚拟路由器及端口转发，并修改主机内网网关的方式实现外网对此两种机型的访问，操作方法如下：

1.选择UNet-\>弹性IP，申请外网弹性IP，该IP为对此两种机型的访问入口：

![image](/images/bigdata1.png)

2.选择UNet-\>路由器，并创建转发路由器：

![image](/images/bigdata2.png)

3.选择UNet-\>弹性IP，并将刚刚申请的弹性IP绑定到转发路由器上：

![image](/images/bigdata3.png)

4.配置主机中的路由，将路由指向转发路由器，并修改网络设备端口中的网关设置：

**Linux系统中**

```
ip route change default via $router_ip
```

> Note
> 
> 这里的$router\_ip为10.9.31.156，即转发路由器的内网IP地址。

修改/etc/sysconfig/network-scripts/ifcfg-eth\* 中GATEWAY

```
GATEWAY=$router_ip
```

**windows系统**

```
route change 0.0.0.0 mask 0.0.0.0 $router_ip [if n] /p
```

n为网卡号码，单网卡不需要输入中括号中内容。可以通过在cmd中执行route print结果中的第一列查看。

![image](/images/route_print.png)

> **note**
> 
> 这里的$router\_ip为10.9.31.156，即转发路由器的内网IP地址。 /p参数保证路由配置在服务重启后不会失效。

5.选择UNet-\>路由器-\>点击路由器ID-\>端口转发-\>添加转发规则，填入源端口(外网端口)及需要转发的主机内网IP及主机端口：

![image](/images/bigdata4.png)

![image](/images/bigdata5.png)

> Note
> 
> 为了安全起见，这里尽量使用非常规端口作为转发端口。
