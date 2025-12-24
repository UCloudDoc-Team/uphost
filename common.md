# 操作指南

## 创建裸金属云主机

在UCloud控制台选择裸金属云主机产品，点选【创建主机】即可进入创建页面。

在创建页面中，选择地域、可用区、机型、镜像等内容，并设置网络以及管理信息，点选【立即购买】，支付完成后会自动跳转至主机管理页，等待主机变为运行状态即可开始使用。

![image](/images/create_uphost01.png)

**Tips：**
- 若您的账号无某些地域的权限（表现为控制台不可见），可联系客户经理或反馈给SPT；
- 裸金属云主机目前仅支持按日、按月以及按年支付，其中按年支付可享受83折，暂不支持按时支付；
- 若控制台资源显示售罄，可联系客户经理或反馈给SPT，可获知确切资源上线时间；
- 若控制台尚未提供能满足您业务需求的配置，欢迎各种渠道的反馈，产品侧会综合考量。

## 裸金属云主机管理

您的每一台裸金属云主机，都可以进行常规的启动、关闭、重启、重装、删除等基本操作，且均支持批量处理。

**Tips：**
- 重装系统，无需额外支付费用；
- 删除主机时，剩余时间的费用会自动退回。

## 紧急登录

紧急登录功能有别于云主机提供的VNC登录，目的在于主机遇到故障时，您可第一时间登录机器排查问题。当然，您也可以用此功能取代SSH或跳板机的登录方式。

**Tips：**
- 部分裸金属云主机暂不支持IPMI功能，若出现主机网络不通、宕机等情况，请紧急联系SPT，所有主机支持IPMI功能将于2020年4月下旬上线。

**配置java环境**

请访问java官网，按照
[说明文件](http://java.com/zh_CN/download/help/ie_online_install.xml)
安装最新的Java运行环境。

**配置java安全名单**

以Windows为例。打开控制面-\>选择Java-\>安全选项卡-\>编辑站点列表。

![image](/images/login1.png)

![image](/images/login2.png)

根据您裸金属云主机所在的可用区，配置以下地址为例外站点：

| 地域  | 可用区  | IP                     |
| --- | ---- | ---------------------- |
| 华北（北京2） | 可用区B | 123.59.90.26  |
| 华北（北京2） | 可用区C | 106.75.0.135  |
| 华北（北京2） | 可用区D | 125.59.130.41 |
| 华南（华南（华南（华南（华南（广州）））））  | 可用区B | 106.75.128.62 |
| 香港  | 可用区A | 23.91.96.36   |

**控制台登录**

在控制台菜单选择：紧急登录。此时会下载jnlp文件，若您已经配置了java环境，此时可以打开此文件，正常登录裸金属云主机。

![image](/images/login3.png)

请忽视相关的安全提示。

![image](/images/login4.png)

为了更优质的使用体验，推荐使用Firefox浏览器或Chrome浏览器进行登录操作。

## 监控

裸金属云主机支持监控功能，监控指标与云主机一致。

需要安装监控代理（UCloud Monitor Agent），具体步骤参考
[监控代理说明文档](https://docs.ucloud.cn/cloudwatch/uboltagent/UboltAgent_Linux_Installation_Guide)；
**请注意**：当前仅部分机型支持安装，您可前往[支持的裸金属机型列表](https://docs.ucloud.cn/cloudwatch/uboltagent/GPUPHostList)查询您的裸金属实例是否支持安装，如果不支持，请联系您的客服经理，我们会尽快安排适配。



## GPU裸金属云主机安装温度监控

推荐给您的GPU裸金属云主机安装UMA，以进行温度监控。

一、安装NVIDIA驱动

1. [访问NVIDIA官网获取下载地址](https://www.nvidia.com/download/index.aspx?lang=cn)
   - 产品家族选择具体机型
   - CUDA ToolKit选择驱动支持的CUDA版本，没有选择则是默认版本
2. 选择完成后，点击搜索→下载，复制链接地址（下图以A100为例：）
     ![image](/images/downloadnv1.png)</br>
     ![image](/images/downloadnv2.png)</br>
     ![image](/images/downloadnv3.png)</br>
3. 进入主机
   - 执行命令下载驱动(wget后面是复制的链接地址)：`wget {上一步复制的链接地址}`
   - 检查gcc、make软件库是否安装，及安装gcc 和 make
   ```sh
   ## which make 检测make是否安装, 安装命令 # sudo apt-get install make
   ## gcc --version  检测gcc是否安装, 安装命令 # sudo apt-get install gcc
   ```  
4. 开始安装：执行`sh NVIDIA-xxxxxxx.run `，即开始安装驱动，注：遇到权限问题命令前添加sudo即可
5. 验证nvidia驱动：执行 `nvidia-smi`

二、安装监控agent

[监控代理说明文档](https://docs.ucloud.cn/cloudwatch/uboltagent/UboltAgent_Linux_Installation_Guide)

告警设置在监控-\>监控模板中，可额外添加GPU温度项目，推荐将告警阈值设置为90℃。
