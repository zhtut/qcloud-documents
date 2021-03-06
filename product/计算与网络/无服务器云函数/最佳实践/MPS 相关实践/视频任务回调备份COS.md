## 操作场景


本文为您介绍如何将 [视频处理 MPS](https://cloud.tencent.com/document/product/862) 产生的回调任务通过云函数 SCF 及时备份至 [对象存储 COS](https://cloud.tencent.com/document/product/436)。其中，SCF 主要提供回调消息处理，MPS 主要用于视频处理任务，COS 主要提供终端永久性存储能力。




## 操作步骤


<span id="step01"></span>

### 创建云函数

1. 登录云函数控制台，选择左侧导航栏中的【[函数服务](https://console.cloud.tencent.com/scf/list)】。
2. 在“函数服务”页面上方选择**北京**地域，并单击【新建】进入新建函数页面，配置以下参数：
 - **函数名称**：命名为 “MPSToCOS”。
 - **运行环境**：选择 “Python 2.7”。
 - **创建方式**：选择【模板函数】。
 - **模糊搜索**：输入“MPS 消息转储至 COS”，并进行搜索。
![](https://main.qcloudimg.com/raw/66b319afc63ae207e61bdba85c036ab4.png)
>?单击模板中的【查看详情】，可在弹出的“模板详情”窗口中查看相关信息或进行下载。
3. 基本信息配置完成之后，单击【下一步】进入函数配置页面。
4. 函数配置保持默认配置，单击【完成】即可创建函数。

<span id="step02"></span>

### 配置 MPS 触发器

1. 完成上述 [创建云函数](#step01) 步骤之后，将默认进入该函数的管理页面。
2. 在左侧菜单栏中选择【触发管理】页签进入“触发器管理”页面。
3. 单击【创建触发器】，在弹出的“创建触发器”窗口中添加 MPS 触发器。如下图所示：
![](https://main.qcloudimg.com/raw/096c1fb010826afb6f03e1402890d97e.png)
主要参数信息如下，其余配置项请保持默认：
	- **事件类型**：MPS 触发器以账号维度的事件类型推送 Event 事件，目前支持工作流任务（WorkflowTask）和视频编辑任务（EditMediaTask）两种事件类型触发。
	- **事件处理**：MPS 触发器以服务维度产生的事件作为事件源，不区分地域、资源等属性。每个账号只允许两类事件分别绑定单个函数。如需多个函数并行处理任务，请参见 [函数间调用 SDK](https://cloud.tencent.com/document/product/583/37316)。
>?初次创建 MPS 触发器，需单击【SCF_QcsRole】、【MPS_QcsRole】对相关服务角色进行授权。
 
 
<span id="step05"></span>

### 测试函数功能
1. 登录 [视频处理控制台](https://console.cloud.tencent.com/mps)，执行视频处理工作流。
2. 在云函数控制台【[函数服务](https://console.cloud.tencent.com/scf/list)】页面中，单击上述 [创建云函数](#step01) 步骤中创建的云函数名称，进入函数详情页面。
3. 在函数详情页面中选择【日志查询】页签，可以查看到打印出的日志信息。如下图所示：
![](https://main.qcloudimg.com/raw/b4d8dd0a4a236ab4cb35f2e7d3160649.png)
4. 切换至 [对象存储控制台](https://console.cloud.tencent.com/cos5)，查看数据转储及加工结果。
>?您可以根据自身的需求编写具体的数据加工处理方法。
