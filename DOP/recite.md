[kaoshibao](https://www.kaoshibao.com/online/?paperId=16345324&practice=&modal=1&is_recite=&qtype=&text=顺序练习&sequence=83&is_collect=1&is_vip_paper=0)

* 单选题

# 1
* Q
    * ALB + Lambda
    * 许多**不同版本**APP在同时使用
    * 应用程序版本在 **用户代理标头**
    * 修改后，**出现问题**
    * 观察 收集， API，指标
    * Lambda 已经修改
* A
    * 修改**Lambda**函数 **写入**  **CloudWatch Logs** 日志组，**指标过滤器**，为每个API操作名称 递增指标
    
# 2
* Q
    * Lambda
    * 初始化，DynamoDB加载数据，冷启动8-10s
    * 一天中间 10倍 其他时间 10%
    * 要全天减少延迟
* A
    * Lambda **预配置并发数**。最小1，最大100
    
# 3
* Q
    * 当前**日志级别**在**Apcache设置**中
    * 希望 部署时**动态改变**， 无需 应用程序修订版本
* A
    * 创建脚本， CodeDeploy
    * DEPLOYMENT_GROUP_**NAME**
    * .yml文件，**BeforeInstall** 生命周期钩子
    
# 4
* Q
    * 给**EBS打标签**，*Backup_Frequency*
    * 偶尔忘记，希望**始终**有
* A
    * AWS Config，**托管**规则
    
# 5 - new
* Q
    * Aurora集群，通过**实例端点**读写
    * 需要：维护期间，保持可用，减少中断
* A
    * 打开**Multi-AZ**，使用 Aurora**集群端点** 写操作。更新集群的读取端点 读操作
    
# 6
* Q
    * KMS 密钥 + 手动轮换
    * 希望：90天后，**未**轮换通知
* A
    * AWS **Config** + SNS
    
# 7
* Q
    * AWS CodeBuild 使用未经验证的  S3
    * 需要：不允许 S3 未经验证的请求
* A
    * 存储桶策略，**删除**未经验证的访问
    * 修改 **CodeBuild** 服务角色，包括S3访问权限
    
# 8
* Q
    * 数百个EC2实例，要求必须使用EC2实例配置文件 （或者默认配置）
    * 发现有不合要求的
    * 希望：所有现在、未来的EC2，附加配置文件
* A
    * `ec2-instance-profile-attached` **AWS Config** 托管规则， AWS System Manager Automation 运行簿 附加默认配置
    
# 9
* Q
    * 构建持续部署管道，程序使用 Lambda
    * 希望：减少**不成功部署**对用户的**影响**。监控问题
* A
    * AWS Serverless Application Model (AWS **SAM**) 模版定义
    * **Canary**10Percent15Minutes
    
# 10
* Q
    * EC2带有公网IP，启动时安装程序工件
    * 要求改变，**不能访问互联网**。实例健康，**但程序没有安装**
* A
    * **工件放入S3**，创建S3 VPC端点
    
# 11
* Q
    * CodeCommit, CodePipeline
    * 用远程主分支 触发 管道
    * 代码变更 10分钟 **管道无反应**
* A
    * 检查 是否 **main分支** 创建 **EventBridge** 规则
    
# 12
* Q
    * 防止EC2修改安全组，不受限制的入站访问
    * 已经写好处理的Lambda函数
    * 要求：近乎实时的检测更改
* A
    * *EventBridge***事件**规则，默认总线 为源，定义规则 匹配
* K
    * （A. 选项不对，因为订阅不能实时）
    
# 13
* Q
    * CloudFormation部署Web服务：私有子网， ALB+EC2
    * 必须接受IPv6的访问
* A
    * VPC+ALB子网，添加**IPv6 CIDR**
    * ALB上制定**dualstack IP**
* K
    * dualstack IP - clients can communicate with the load balancers using both IPv4 and IPv6 addresses）
        
# 14
* Q
    * 公司使用 企业支持计划
    * 确保新账户使用
    * 使用 Account Factory for Terraform (AFT) 配置
* A
    * **AFT aft_feature_enterprise_support** = True
* K
    * Account Factory for Terraform (AFT) is an *AWS Control Tower* **feature** that provisions new accounts using Terraform templates.
    
# 15
* Q
    * AWS System Manager 维护 EC2
    * 需要：收到 AWS Health通知后，重启EC2。自动化，已经有EventBridge规则
* A
    * AWS **Health 事件源**，目标设为**System Manager文档**
* K
    * AWS Health
        * is a service that provides personalized information, notifications, and proactive insights to help you *manage events and changes in your AWS environment*. It gives visibility into service disruptions, scheduled changes, and operational issues that might affect your AWS resources.
        
# 16
* Q
    * 已经 应用程序容器化
    * EC2 运行 Jenkins， 实例需要 打补丁和升级
    * 要求 加密构建工件
* A
    * AWS **CodeBuild** 加密，**替换** EC2 上 **Jenkins**
    
# 17
* Q
    * CloudFormation 堆栈删除时，错误
    * S3 存储桶没有被删除
    * 确保删除
* A
    * 添加Lambda，DependOn属性，删除所有对象
    
# 18
* Q
    * EC2， 元数据存在S3
    * 实例重启时，需**检索**元数据
    * 实在需在**无响应时，自动重启** （restart or relaunch）
* A
    * **OpsWorks**，自动修复。拉取S3元数据，更新到实例
    
# 19 - 图
* Q
    * AWS IAM Identity Center (AWS Single Sign-On) + AWS Toolkit for Microsoft Azure DevOps 集成
    * 属性映射：${path:enterprise.department}， ${path:enterprise.costCenter}
    * 三部门 （d1, d2, d3）
    * 授权每个用户，仅访问对映部门 EC2
    * 需要：根据匹配的属性，创建策略。部门 与 EC权限对映
* A
    * ![](vx_images/593756464355033.png)
    
# 20
* Q
    * 安全审计发现，被审计账户，可以修改或删除 审计IAM角色
    * 防止除授信人员修改
* A
    * **SCP**， **拒绝**更改。条件允许授信人员。**加到组织根**
    
# 21
* Q
    * Go 应用
    * 希望 蓝/绿部署，A/B测试
* A
    * Elastic **BeanStalk**
    
# 22
* Q
    * EC2 自动扩展组 ELB
    * ELB HTTP **健康检查后 终止** ，**收不到日志**
    * 如何收集到日志
* A
    * 自动扩展 生命周期 Terminating:**Wait**，被触发**Lambda** Eventbridge， Lambda调用**SSM**收集日志
        * 被触发Lambda 很重要。这样才能衔接上操作
    
# 23
* Q
    * SecOps团队，需要
    * 关闭 S3**阻止公共访问功能**， 收SNS通知
    * 个别账户不能关闭通知
* A
    * **AWS Config**， s3-bucket-level-public-accessprohibited
    
# 24
* Q
    * 容器化 迁移 EKS
    * **EKS活动 电子邮件通知**
    * 已准备 SNS 和 分发Lambda
* A
    * 启用 **CloudWatch Logs** 记录*EKS组件*， **订阅过滤器**
        * 没说要求看的事情，不要Insights
    
# 25
* Q
    * 电子健康记录，患者隐私要求
    * EC2 操作系统、补丁 **持续合规**
    * 使用 默认和**自定义** 存储库 自动化 **操作系统 和 应用补丁**
* A
    * **AWS System Manager** 补丁基线
    * **自定义**存储库
    * **AWS-RunPatchBaseline**

# 26
* Q
    * CodePipeline， CodeDeploy 蓝/绿部署 on ECS
    * 转移流量前，测试 绿色版本。如错误，回滚
* A
    * CodeDeploy **AppSpec**文件 加 钩子， AfterAllow**Test**Traffic
    * 调用**Lambda**，运行测试。错误，回滚
        * 生命周期，调Lambda，很重要
    
# 27
* Q
    * AWS Storage Gateway （文件网关模式） 在 S3 前面
    * S3被多个资源使用
    * S3中有数据，AWS Storage Gateway 却看不到
* A
    * Lambda运行 Storage Gateway 的 **RefreshCache** 命令
    
# 28
* Q
    * 卫星数据
    * API Gateway + SQS
    * **保留失败的数据**，供科学家审查
* A
    * SQS **死信**，Maximum Recives 1
    
# 29
* Q
    * CloudFormation
    * **多个版本**
    * **确保资源根据公司政策部署**
* A
    * 创建 AWS **Service Catalog**产品
    
# [30]
* Q
    * CodePipeline，增加手动批准
    * Webhook 聊天工具
    * 如何配置 管道活动的**状态更新** 和 批准请求
* A
    * **CodePipeline Pipeline** Execution State Change
    * Amazon **EventBridge** rule
    * **Lambda** send SNS
    
# 31
* Q
    * 堡垒机
    * 安全组限制：SSH访问限制在特定IP
    * 安全组规则**被修改时，通知**
* A
    * 使用 XXX 创建 **AWS Config** 规则
    
# 32
* Q
    * Amazon Aurora Multi-AZ DB cluster
    * 已经创建一个跨区域只读副本
    * 希望：故障时，自动化副本升级
* A
    * AWS Systems Manager Parameter Store 存 Aurora端点
    * EventBridge + Lambda 提升副本，修改 Parameter Store
    
# 33
* Q
    * EC2 + EBS
    * 网络连接、电源故障 （不是机器坏了）
    * 快速恢复，减少数据丢失
* A
    * StatusCheckFailed CloudWatch 警报
    * EC2 恢复 （recover）
    
# 34
* Q
    * 希望 AWS工具 替换 **bash**部署脚本
    * 包括：**单元测试**、停止并启动服务等
* A
    * **CodePipeline**触发**CodeBuild**测试， appspec.xml **调用 bash脚本**
* R
    * 思路：改用 Codepipeline，还要调用到 bash脚本
    
# 35
* Q
    * 使用 AWS Control Tower 创建新账户时
    * 同时应用 SCP 和 CloudFormation，自动部署资源
* A
    * **Customizations for AWS Control Tower** (CfCT)
    
# 36
* Q
    * 六个月扩展到欧洲和亚洲
    * EC2 ELB Auto Scale Group
    * Aurora数据库实例
    * 希望： **单一**产品目录，客户信息保存在**每个区域**
* A
    * 每区域 **Aurora读取副本**，**额外实例**存储客户信息
    
# 37
* Q
    * 全球访问 API
    * 确保 北美 欧洲 **高可靠、低延迟**
* A
    * Route53 指向 北美 欧洲
    * **延迟路由**（latency-based routing）+ **健康检查**
    * DynamoDB **全球表**
    
# 38
* Q
    * 当前，AWS控制台手动创建。开发环境 具有共同标准
    * 希望，**自动化 开发环境 创建**
    * 预计应用设施将增长
* A
    * **嵌套堆栈**（nested stacks） 定义共同的基础组件
    * Fn:ImportValue内联函数
    * （**没有**：TemplateURL）
    
# 39
* Q
    * 要求：未加密EBS都标记不合格
    * 合规检查始终存在
* A
    * AWS Config。 SCP禁止删除
    
# 40
* Q
    * 代码合并，未通过测试的 拉取请求（pull request）
    * 导致 **分支**回滚，浪费精力
    * 需求：自动化CodeCommit的拉取请求测试。 容易看到 自动化测试结果
* A
    * 响应 pullRequestCreated **and** pullRequestSource**Branch**Updated

# 41
* Q
    * 加 AWS WAF 担心成本 不批准 先证明
    * 希望：**接近实时**接收IP拒绝列表上的访问
    * 工程师 为生产VPC创建了**VPC流日志**。
    * 最具**成本效益**
* A
    * Amazon **CloudWatch Logs** 创建日志组
    * VPC 流日志
    * 为拒绝列表创建 **指标过滤器**
    * **5分钟周期**
        * 也可以理解从成，延迟5分钟比较省钱。这个方案本身用的服务少，也省钱
    
# 42
* Q
    * 灾难恢复目的，正在使用第二个区域
    * 会话必须实时复制，1%到辅助区验证
    * 中断，自动路由辅助区
* A
    * 2个区域中，Elastic Beanstalk
    * DynamoDB **全局表**
    * Route53 **加权路由**，健康检查
    
# 43
* Q
    * EC2 舰队
    * 第二舰队，容量同1舰队。部署完成转移流量，1小时后终止1舰队
* A
    * **CodeDeploy**，配置 蓝绿部署，1小时后终止1舰队
    
# 44
* Q
    * S3 视频
    * 受欢迎，分析访问量 某个文件的用户数量
* A
    * **激活**S3服务访问日志
    * Athena创建**外部表**，SQL查询
    
# 45
* Q
    * CloudFormation。开发人员 没有 模版所需资源的权限
    * 允许开发人员部署堆栈
    * **最小权限原则**
* A
    * 创建 **CloudFormation 服务角色**
    * 授予 **iam:PassRole**
    
# 46
* Q
    * 手动登录过的EC2，24小时内终止
    * 已经有 CloudWatch Logs
* A
    * 创建 **CloudWatch Logs**订阅。EC2登录添加标签
    * Eventbridge **每天调一次**Lambda 终止有标签的实例
        * A。不要用 Step Function，复杂了
    
# 47
* Q
    * 所有账户启用CloudTrail。 账户数量增加500
    * 组织，为每个账户**启用 AWS Config**，也自动为新账户创建
* A
    * 在组织管理账户中，创建 **CloudFormation** 堆栈集 （*StackSets*)。在**组织创建时**自动部署
    
# 48
* Q
    * 多语言和框架 开发应用程序。不同操作系统
    * **技术栈迁移**。**减少复杂性**，集中控制源代码
* A
    * CodeCommit， CodeCommit 构建 **Docker**，部署到Fargate
    
# 49
* Q
    * 所有账户，必须开启CloudTrail和AWS Config
    * 个人账户，**不能改基线资源**，但可以**编辑或删除自己**的CloudTrail **轨迹** 和AWS Config**规则**
* A
    * 指定 **AWS Config**管理账户
    * CloudFormation StackSets 创建**AWS Config 记录器**
    * SCP 禁止修改/删除AWS Config 记录器
    
# 50
* Q
    * EC2 + ALB + Route53
    * 改为新的 无服务，用 API Gateway + Lambda
    * 推出前 **一小部分用户上测试**
* A
    * **CloudFormation** 部署 API Gateway + Lambda
    * 使用Lambda 版本
    * **金丝雀策略** 更新API版本
* R
    * D。OpsWorkds，使用 蓝/绿，全量切换，不符合。
    
# 51
* Q
    * CodeCommit,CodePipeline
    * 随时间，拉取请求数量增加，测试失败，流水线受阻
    * 希望：合并前，**对拉取请求** **单元测试**
* A
    * 创建EventBridge规则： CodeCommit **pullRequestCreated**
    * **CodeBuild**运行单元测试
* R
    * C。CodePipeline。这个选项的问题，是没有CodeBuild做单元测试。
    
# 52
* Q
    * 应用程序需要频繁重启
    * 目前，根据错误消息**手动重启**
    * 希望，不重启实例，**自动重启应用**
* A
    * AWS Systems Manager Automation runbook
    * 实例上运行一个脚本
    * 配置**EventBridge**规则，CloudWatch警报 **ALARM**状态
    
# 53
* Q
    * 流水线，私有Github
    * 流水线要支持手动批准
* A
    * CodePipeline
    * **CodeDeploy**
    
# 54
* Q
    * EC2 应用启动，需要从S3中处理数据
    * 增长， 需要几分钟 才能提供服务
* A
    * 自动扩展组 **warm pool**，包含**停止**状态的warmed EC2实例
    * autoscaling:EC2_INSTANCE_LAUNCHING 生命周期钩子
    
# 55
* Q
    * CodeBuild打包应用程序，放入S3
    * buildspec.yml文件
    * 发现：任何人都可以下载
* A
    * 修改post_build，去掉 **--acl authenticated-read**
    * 存储桶策略，只允许相关账号读取

# 56
* Q
    * 数据库 密码 硬编码 在CodeCommit存储
    * 需要：自动检测，并防止硬编码
* A
    * Amazon CodeGuru **Reviewer**
    * AWS **Secrets Manager**
* R
    * A。选项问题是，Parameter Store存密码，并不保密
    
# 57
* Q
    * 确保 现有 新建 S3 全部启用 服务端加密
    * 默认加密类型 AES-256
* A
    * **AWS Config**托管规则 `s3-bucket-server-side-encryption-enabled`
    * AWS-EnableS3BucketEncryption AWS Systems Manager Automation runbook
    * 手动运行重新评估
    
# 58
* Q
    * SaaS，用户分布在多个应用程序的ALB上
    * **不需要构建阶段**
    * 提交CodeCommit，就**触发同时部署**
    * **最少配置**
* A
    * CodePipeline，**单一**CodeDeploy (single)
    * **每个** 应用程序+ALB 的**组对**（pair）
    
# 59
* Q
    * S3 静态网站，Route53 加权路由， TTL 1天
    * 改为，动态网站， ALB+EC2。Route53加条目，指向ALB，权重255，TTL 1小时
    * 仍然显示旧 静态网站
* A
    * 从**托管区域（host zone** **删除**指向S3记录
    * **等待DNS传播**
    
# 60 - 图
* Q
    * ![](https://up.zaixiankaoshi.com/5240831/question/1fb2ce7c048f79c0f618b4de8d90af7d.png)
    * 哪种类型事件匹配
* A
    * 所有 管道中，所有 拒绝/失败 操作
    
# 61
* Q
    * EC2， CloudFormation
    * 希望EC2启动时，有最新的配置文件。并希望CloudFormation模版更新时，反映到实例上
* A
    * CloudFormation模版中，添加 **CloudFormation init 元数据**。`cfn-init` 启动运行，`cfn-hup` 轮询
    
# 62
* Q
    * AMI 每月电子邮件发送
    * 希望：自动化 提供AMI ID
    * 可扩展
* A
    * Amazon EC2 **Image Builder** 新 AMI
    * AMI ARN 放 **AWS Systems Manager Parameter Store**
    
# 63
* Q
    * ALB + EC2
    * CodeDeploy新版本，AllowTraffic生命周期 *期间* 失败，部署日志无原因
    * 可能原因
* A
    * ALB目标组 **健康检查配置不正确**
    
# 64
* Q
    * 20团队 微服务
    * VPC CIDR相同
    * 要求：所有微服务 **私有**网络链接，不能通过公网
* A
    * 每个 VPC 建 NLB， 用 **PrivateLink** 创建 VPC端点
    
# 65
* Q
    * 专有 企业级内存数据存储 网格系统
    * 多节点，每次**节点增删**，重新配置集群
    * **更新** /etc/cluster/nodes.config
    * 希望：自动化
* A
    * AWS **OpsWorks Stacks**
    * **Chef 配方**
* K
    * OpsWorks Stacks
        * Allows you to manage applications and resources using a **declarative** approach
        * 在AWS中自动化集群配置管理的最有效方法是使用AWS OpsWorks Stacks。这允许创建一个*Chef配方*，该配方可以**在集群发生更改时**，例如添加或删除节点时，动态**更新**/etc/cluster/nodes.config文件

# [66]
* Q
    * 每周多次 CodePipeline 部署API
    * 从 API Gateway 导出SDK到 S3，CF分发
    * 需要：自动更新**SDK 可用**
* A
    * 建 **CodePipeline**操作， **Lambda** 从 API Gateway **下载** SDK， **上传 S3**, CF失效
        * 没有导出的功能，就是下载
    
# 67
* Q
    * CodeDeploy 部署 Lambda 在最后阶段
    * 部署后，出现**几秒**钟故障。由于 数据库变更未完全传播
* A
    * **Codedeploy 钩子**， 在完全传播前 暂停部署
    
# 68
* Q
    * 激活 **restricted-ssh AWS Config**托管规则
    * 对不符合的，实时通知
* A
    * **EventBridge**规则，**匹配 restricted-ssh 规则的 NON_COMPLIANT**
    * 配置**输入转换器**，发SNS
    
# 69
* Q
    * 5个 Lambda， CodePipeline 顺序部署
    * 完成时间长
* A
    * 改CodePipeline配置，相同**runOrder**，并行
    
# 70
* Q
    * CloudFormation failed to update
    * “ERROR: both the deployment and the CloudFormation stack rollback failed. The deployment failed because the following resource(s) failed to update: [AutoScalingGroup].”
    * 堆栈仍处于 `UPDATE_ROLLBACK_FAILED`
* A
    * 更新IAM角色 权限
    * 运行命令 `continue-update-rollback`

# 71
* Q
    * EC2实例
    * 需要：**查询**应用程序日志、API活动
* A
    * CloudWatch 代理，EC2， 发 **CloudWatch Logs**
    * **CloudTrail** 发 CloudWatch Logs
    * CloudWatch Logs **Insights** 查
* R
    * C D 用的服务都太多，没必要

# 72
* Q
    * EC2 安全性
    * 发现 **漏洞**通知
    * 跟踪 **登录**
* A
    * Inspector， CloudWatch Agent
    
# 73
* Q
    * EC2, 实例无法 成功启动， 几个小时发现
    * 希望：启动失败时发邮件
* A
    * **配置 自动扩展组**，**实例启动失败**时 SNS
    
# 74
* Q
    * CloudFormation StackSets 为每个账户 启用 AWS Config
    * 新的安全策略，AWS Config 共同基线
    * 使用**中央账户的 共同基线**，**不能修改** 这些基线
* A
    * 创建 AWS Config **合规包**（conformance pack），用 AWS Config 委派 **合规包**
    
# 75
* Q
    * Amazon Kinesis Data Stream
    * Kinesis消费者 应用程序落后， 处理前 丢弃记录
* A
    * 根据 **GetRecords.IteratorAgeMilliseconds** 指标，水平扩展 消费者
    * 增加 Kinesis 数据保留期
    
# 76
* Q
    * 希望：检测潜在受损EC2实例、可疑网络活动、不寻常API活动。发SNS
    * **现有及未来账户**
* A
    * **GuardDuty**  AWS账户配置为**管理员账户**
    * **添加现有账户**（**不是邀请**。要求强制所有）
    * EventBridge
    
# 77
* Q
    * 多账户环境，AWS Transit Gateway，出站流量路由，防火墙检查
    * 防火墙日志到 **CloudWatch Logs**，分级，CRITICAL**报警**
* A
    * **CloudWatch Logs** **指标过滤器**
    
# 78
* Q
    * 每个团队 自有账户
    * **限制** 只能使用 批准的** AWS服务**
    * 必须通过 请求 批准 流程
* A
    * **新的**顶级OU，SCP **拒绝 **受限AWS服务。SCP 附加到OU
        * 新建OU，避免影响太大，产生未知问题
    
# 79
* Q
    * CloudFormation 自定义资源 设置 AD Connector
    * Lambda 运行并创建 AD Connector
    * 不能 from CREATE_IN_PROGRESS to CREATE_COMPLETE
    * 解决
* A
    * Lambda 返回 对预签名 URL的响应
* K
    * 因为CloudFormation期望从自定义资源提供者那里接收一个信号
    
# 80
* Q
    * CodeCommit源代码控制
    * 现在 开发人员 可以直接推送到 主分支
    * 限制 功能分支 -> 主分支
    * **托管策略**
* A
    * 创建 **额外策略** （policy）， GitPush PutFile **拒绝**规则
        * **托管策略**（AWS）是**不允许修改的**
    * 对特定存储库限制，条件 引用 主分支
    
# 81
* Q
    * ALB +EC2 + Auto Scale + RDS MySQL + Route53
    * 实现DR，RTO **4小时**，RPO 15分钟
* A
    * 启动 RDS外 所有副本
    * RDS**只读副本**
    * Route53 健康检查 **故障转移路由**
    * 只读 **提升为主副本**
    
# 82
* Q
    * 开发、测试、生产环境分开
    * 部署期间 获得密码凭据 最安全、灵活
* A
    * AWS Secrets Manager

# 83
* Q
    * 依赖 CloudTrail 检测 敏感安全问题
    * 需要：自动 纠正 CloudTrail 被关闭问题
    * 日志交付停机**时间最少**
* A
    * CloudTrail StopLogging事件
    * **EventBridge**， Stop后调用 StartLogging
* R
    * **其他选项都有时间间隔**

# 84
* Q
    * 一系列 多 CloudFormation， 按**顺序**部署
    * 有效的部署，并通知变更
* A
    * CloudFormation **嵌套堆栈 和 堆栈集**。 SNS
        * Leverage CloudFormation *nested stacks *and *stack sets* for deployments.

# 85
* Q
    * ... CloudFormation
    * CI/CD 已部署模版新版本， ALB健康检查失败
    * CloudFormation 状态是 UPDATE_COMPLETE
    * Apache Web 未能启动成功
    * 如果确保 实际未能完成，CloudFormation部署失败？
* A
    * **cfn-signal**辅助脚本 发送成功/失败信号
    * CloudFormation 使用 WaitOnResourceSignals 更新策略， 超时

# 86
* Q
    * **数据敏感** EC2没有访问互联网
    * 允许专门的**AMI**
    * 管理员需要能**登录EC2**，访问必须自动化，**集中控制**。访问收到通知
* A
    * EC2 **Image Builder** 自定义 AMI，System Manager 代理
    * EC2实例附加角色 **AmazonSSMManagedInstanceCore**
    * System Manager 登录EC2
    
# 87
* Q
    * S3
    * 希望：对 现有 未来 的，都启用加密、日志记录、版本控制，不应公开读写
* A
    * AWS Config规则，AWS Systems Manager 文档 自动纠正
    
# 88
* Q
    * 无法用Docker，必须在EC2上，数据在 **NFS**，**容忍中断**
    * 配置软件需要**30分钟**
* A
    * EFS，EC2 **竞价实例**，**自定义AMI**
    
# 89
* Q
    * 有API Gateway
    * 希望：新版本部署 **最小中断**，**快速回滚**
    * **最小变化**
* A
    * **并行** 独立环境
    * API Gateway **金丝雀发布**
    
# 90
* Q
    * EC2 自动扩展组 蓝/绿部署
    * 更新时，所有用户被登出
    * 部署用 不可变实例
    * 需要：部署期间，保持登录状态
    * 最具操作效率
* A
    * ElastiCache 存储会话
    
# 91
* Q
    * 三层应用 蓝/绿部署
    * ALB可以配置 将流量发送到 蓝 或 绿 环境目标组
    * Route53
    * 必须 **一次性** 将流量 从 蓝 到 绿
* A
    * （Route53切换不行，无法一次性全部流量）
    * 启动绿环境 滚动重启 部署新软件
    * AWS CLI 命令 **ALB 流量**发送到 绿 环境
    
# 92
* Q
    * 所有EC2计划维护通知
    * 发到Slack 和 邮箱
* A
    * **EventBridge** 监视 AWS **Health**
    * 事件发送 SNS
    
# 93
* Q
    * CodePipeline Codedeploy
    * Codedeploy问题，管道失败
    * 希望：监控 通知，**减少解决时间**
* A
    * **CodePipeline 和 CodeDeploy** 实施 **EventBridge**
    * **Eventbridge，** **Lambda评估**
        * （利用 EventBridge，工程师可以捕获部署事件并基于这些事件触发动作）
    
# [94]
* Q
    * 集中的DevOps账户
    * DevOps账户存在一个用于部署的IAM角色
    * 应用程序 部署 **EKS**
    * 在集中的DevOps账户中**存在一个用于部署的IAM角色**
    * 部署通过 DevOps账户 的 CodeBuild 进行
    * 部署失败 未经授权错误
* A
    * 应用程序账户 部署IAM角色， DevOpen账户， 建立信任
    * 允许 **sts:AssumeRole**
    * 配置EKS集群 **aws-auth ConfigMap**
* R
    * 快速找法，必须2个黑体，然后排除D（AWS Control Tower） C（sts:AssumeWithSAML）
    
# 95
* Q
    * **除非紧急，不允许登录EC2**
    * 如果登录，15分钟内通知
* A
    * CloudWatch **代理**
    * **指标过滤器**， SNS通知
* R
    * 其他选项。Inspector不对。Kinesis、Athena都太麻烦
    
# 96
* Q
    * 遗留应用程序
    * 自动化构建
    * 部署工件存在S3，部署时引用
    * 最具操作效率
* A
    * 遗留程序 **docker**
    * ECR注册docker
    * CodeBuild，用docker镜像构建部署工件
    * 存S3
* R
    * D。额外的事情
    
# 97
* Q
    * 原 CodeBuild 将 Docker 镜像上传 S3
    * 替换为 ECR， 信息放入 buildspec.yml
    * 使用 **docker build 和 docker push**，**访问ECR失败**
* A
    * 使用 **aws ecr get-login-password** AWS CLI command 登录ECR
    * docker login 获取身份 **令牌访问** ECR
    
# 98
* Q
    * 外部 SAML2.0 (Idp)
    * 现有 活动目录系统 中的 组 
    * 希望：使用现有凭据访问AWS
* A
    * **外部Idp**为身份源，使用**SCIM**
        * **没说是AD**，所以不选C

# 99
* Q
    * 避免配置错误，强制使用 CloudFormation
    * 使用控制台修改生产环境
    * 要求：接近实时，**识别不合规**的配置错误，15分钟内自动**纠正**
* A
    * **AWS Config** 识别，AWS Config**合规包**（conformance packs）自动修复
    * 启用**AWS Security Hub** --no-enable-default-standards

# 100
* Q
    * OUs 默认 FullAWSAccess
    * 从 Development OU中删除 FullAWSAccess， 允许EC2上执行所有操作
* A
    * Development OU上允许EC2所有API，所有**其他**API被**拒绝**

# 101
* Q
    * 灾难恢复能力，希望 能 切换 第二个AWS区域
    * 主区域，CodeCommit 源代码工具
    * 次要区域，开发代码能力 
* A
    * **次要区域**，建 **CodeCommit**，主的 **Git镜像**操作
    * 响应主的合并事件
    
# 102
* Q
    * 正在合并应用程序代码修订
    * 生产环境 Amazon RDS Multi-AZ **DB** cluster
    * 持续集成，需要 部署在生产环境前，**测试**
* A
    * **CodeBuild** buildspec文件 从**快照恢复DB**
    * 运行测试，测试后删除恢复的DB
    
# 103
* Q
    * 多租户
    * GuardDuty 发现发送 Security Hub
    * **可疑流量** 生成大量发现 （finding）
    * 需要：发现可以来源，**拒绝VPC流量**
* A
    * **Network Firewall**， **Drop** 规则， Lambda响应 发现
    
# 104
* Q
    * Codepipeline 部署到**多个区域**
    * 管道为**每个区域配置了一个阶段**
    * 希望：管道继续配置**下一个区域**前，确认应用程序健康状态
    * 已经创建了 CloudWatch + Route53 健康检查
* A
    * AWS **Step Functions** 检查 CloudWatch警报
    
# 105
* Q
    * CloudWatch 监控 EC2
    * **12 小时窗口， 至少 3小时** 平均 NetworkPacketsIn 小于5，**停止**实例
    * 数据**缺失**，EC2实例**继续运行**
* A
    * **12中3**
    * **不**将确实数据**视为违规**
    

# 106 - 不懂
* Q
    * 想调查 EBS 未附加标签
    * Lambda **30分钟运行一次**
    * 标记所有 7天或更长
    * 需要 部署这个Lambda
* A
    * 组织 配置 一个**委派管理账户**
    * **EventBridge 定时规则**
    * CloudFormation **StackSets**
    * 创建 CloudFormation 目标，部署这个规则 到 所有成员
* R
    * 先找EventBridge**定时规则**
    * 只是想不是 Lambda，和其他的CodeDeploy、FullAccess无关
    
# 107 - 图
* Q
    * CodeDeploy 蓝绿部署
    * **开始处理流量前**，下载 **安装** **许可证文件**
    * 加 钩子
* A
    * **BeforeInstall**

# 108
* Q
    * Python代码存在 CodeCommit， Lambda运行用
    * Python代码在生产环境故障，要加**单元测试**，集成到 CodePipeline
    * **生成测试报告**
* A
    * 创建 **CodeBuild**， CodePipeline 配置测试阶段。buildspec.yml定义单元测试，输出**JUNITXML**
    
# 109 - 图
* Q
    * SCP，**防止** 成员账户的**根用户**，进行**任何**AWS API调用
* A
    * ![](vx_images/1943643967768.png)
    
# 110
* Q
    * 流量模式可预测
    * 已配置 VPC 流量日志， 发到 CloudWatch Logs
    * 需要：**识别VPC流量随时间异常**
* A
    * Kinesis Data Stream, **Kinesis Data Analytics**
* R
    * 从分析入手，其他选项都不行

# [111]
* Q
    * 故事：想在 DevOps工程师的账户中， 假设OrganizationAccountAccessRole角色，访问新成员账户。错误
    * AnyCompany 收购 Example Corp
    * **假设** OrganizationAccountAccessRole 角色 访问 **成员账户**
    * **错误**："Invalid information in one or more fields. Check your information or contact your administrator."
* A
    * **新成员****创建** OrganizationAccountAccessRole 新角色
    * 信任策略
* R
    * 选择**完全步骤**
    
# 112
* Q
    * 旧 REST API， Kinesis数据流， 10%存在错误数据
    * Lambda 按**批次** 处理
    * 发现 死信 包含没有错误 且 已经处理的记录
* A
    * 出现错误时，**拆分批次**
    
# 113
* Q
    * 原手动 **测试成功后 部署**
    * 需要自动化，且**流量逐渐转移**
* A
    * CodePipeline， **post-commit钩子**，测试通过后触发
    * CodeDeploy **canary**
    
# 114
* Q
    * Lambda + API Gateway
    * 希望：推送 代码 CodeCommit, 环境分支代码，自动化部署Lambda
    * 条件：**分 测试、生产环境 设置管道**，**仅对测试环境自动部署**
* A
    * **2个** CodePipeline**配置**
    * 配置生产管道 手动
    * **一个** CodeCommit，**每个环境一个分支**

# 115
* Q
    * 迁移程序
    * 需要特定版本的 **Apache Tomcat、HAProxy、Varnish Cache**
    * 要求：自动化部署，基础设施可扩展
* A
    * CodeCommit， **appspec.xml** **安装必要软件**
    * 自动扩展组，CodePipeline
    
# 116
* Q
    * CodeDeploy 部署组 集成EC2自动扩展组
    * .**OneAtATime**执行部署，5台中2台 旧版本
    * **原因**
* A
    * **新部署尚未完成** 启动2个新实例 
        * （如果新实例在部署过程中启动，如果部署尚未完成，它们可能不会收到最新的应用程序修订版）
        
# 117
* Q
    * 不允许 开发人员 附加EIP到实例
    * 生产服务器 任何时候 EIP， 通知
* A
    * **IAM策略** 开发人员IAM组，**拒绝** associate-address权限
    * （另外），AWS Config规则 检查 **地址关联情况**（不是检查IAM角色权限）
    
# 118
* Q
    * 多账户
    * **定期更新** Linux **AMI**，装新版本 *Chef代理*
    * 提供新AMI
    * **最小管理开销**
* A
    * EC2 **Image Builder**
    * AWS **Resource Access Manager**
    
# 119
* Q
    * 自动缩放，部署应用
    * 希望：**一次部署一个实例**，确保继续服务。**CPU超过85%**，**自动回滚**
* A
    * CodeDeploy + EC2自动缩放
    * CodeDeployDefault.**OneAtATime** 部署策略
    
# 120
* Q
    * 现: **代码存S3**
    * 希望：避免代码冲突、丢失。构建测试环境，部署测试，**2个环境**
* A
    * CodeCommit
    * **主分支生产，测试分支测试**    

# 121
* Q
    * 使用**大量 EBS支持的 EC2**
    * 减少手动工作，在EC2**计划退役**时自动化 重启 操作
* A
    * **AWS Health** Amazon EventBridge
    * （AWS系统管理器自动化运行薄）AWS Systems Manager Automation runbooks
    * 停止并启动EC2
    
# 122
* Q
    * AWS Control Tower
    * 为团队启用 Amazon GuardDuty
    * 问：如何配置 CloudFormation 模版防止 StackSets 发生故障
        * 其实，问题想问：在CloudFormation中，**如何判断，并启用 GuardDuty**
* A
    * CloudFormation**自定义资源 调用Lambda**
    
# 123
* Q
    * 请人 漏洞检查。安全扫描。**结果没被发现**
    * 需要：EC2被扫描，自动通知 SNS
* A
    * Amazon **GuardDuty**
    
# 124
* Q
    * EKS 负载均衡 表现不佳
    * 部署后**立即扩展到最大数量**，还没有用户流量前
* A
    * EKS **垂直pod伸缩**（Vertical Pod Autoscaler）
* K
    * 原因：单个pod size不对，导致水平扩展。也因为已经水平扩展，所以不是水平扩展问题
        
# 125
* Q
    * EC2 控制措施 使用 实例元数据服务版本2（Instance Metadata Service Version 2 (IMDSv2)）
    * **阻止使用 版本1， 终止**
* A
    * **AWS Config** 检查， AWS Systems Manager **Automation** 终止以修复
    
# 126
* Q
    * 自动化账户 CI/CD 创建新AWS账户
    * 服务账户，希望：**CreateAccount API** 调用时，能收到AWS CloudTrail事件
* A
    * *自动化账户* 中 创建 **EventBridge规则**，发送到 服务账户 **默认事件总线**
    * 允许来自自动化账户 事件
* R
    * 不需要**额外创建** 事件总线。其他3项都不对
    
# 127
* Q
    * SQS -> Lambda -> DynamoDB
    * 最大化Lambda可扩展性，**防止多次处理 成功处理的SQS消息**
* A
    * FunctionResponseTypes 列表包含 **ReportBatchItemFailures**
* K
    * 当 Lambda 处理 SQS 消息时，如果某些消息成功处理而某些失败，Lambda 可以报告成功与失败的具体记录。成功处理的消息会自动从 SQS 删除，而失败的消息会被再次处理。
    
# 128
* Q
    * CodePipeline CodeBuild 运行测试，输出报告，保留90天
* A
    * CodeBuild 添加**报告组** 存 报告。**Eventbridge 调 Lambda**，复制 S3。90天生命周期
    
# 129
* Q
    * AWS Directory Service for Microsoft Active Directory Idp
    * CloudFormation 包含 Windows EC2 
    * 需要：所有**EC2 加入 Microsoft AD 域**
* A
    * CloudFormation模版中，**AWS-JoinDirectoryServiceDomain** Automation runbook
    
# 130
* Q
    * 一个根OU，一个子OU
    * 根OU SCP允许所有
    * 子OU SCP 允许DynamoDB Lambda，拒绝所有
    * vender-data账户在**子OU，启动EC2失败**
* A
    * **更新**子OU SCP，**允许EC2操作**
* R
    * C。错误，因为SCP不能作用在账户上，要在OU上
    
# 131
* Q
    * Image Builder 构建AMI
    * 需要：自动伸缩组 **必须使用最新 AMI**
    * **最高操作效率**
* A
    * 配置 Image Builder **分发设置** 更新启动模版
    * 配置 自动伸缩组 **用最新版本**

# 132
* Q
    * 环境：在S3中，新建 修改 对象时，向Lambda函数传入 名称 键
    * 发现：**对象改变时，Lambda未运行**
* A
    * **资源策略** for Lambda，触发权限
    
# 133
* Q
    * **Amazon Route 53 Application Recovery Controller** （ARC）
    * DR测试，不小心关闭2个 Region 路由
    * 需要：确保**至少一个路由控制 开启状态**
* A
    * 断言安全规则。**ATLEAST**类型规则，值1
    
# 134
* Q
    * *健康软件许可证* 成本增长
    * 审计流程，程序仅在**专用EC2**主机运行
    * 最少行政开销
* A
    * **AWS Config**
    
# 135
* Q
    * 只能通过 **预先批准** 的**CloudFormation模版** 启动资源
    * **偏离预期**状态监控
* A
    * **AWS Service Catalog**部署 的CloudFormation堆栈
    * **AWS Config** 检测
* K
    * AWS Service Catalog允许公司提供**预先批准**的CloudFormation模板

# 136
* Q
    * 希望：在账户创建**资源接近限制**时，警报
* A
    * AWS **Trusted Advisor**
* K
    * AWS Trusted Advisor提供了服务限制的仪表板，并且可以定期刷新这些检查
    
# 137
* Q
    * CloudFormation。 ECS 和 EC2 创建成功，**但EC2关联到不同集群**
* A
    * AWS::AutoScaling::**LaunchConfiguration**
* K
    * 在AWS::AutoScaling::LaunchConfiguration资源的UserData属性中引用ECS集群，可以确保EC2实例在启动时加入到正确的ECS集群中
    
# 138
* Q
    * 用仪表板 显示产品**交易** **饼图**
    * 与 CloudWatch 集成
    * **最高运营效率**
* A
    * 更新 电商程序，交易发json对象，CloudWatch**日志洞察**（CloudWatch Logs **Insights**）  查询日志组，饼图，附加仪表板
    
# 139
* Q
    * 应用 必须仅适用 **已批准的AWS服务**
    * 需要：创建 新组织账户， SCP仅支持当前在用AWS服务
    * **AWS Identity and Access Management (IAM) Access Analyzer**
* A
    * 新SCP，允许 识别服务，新OU，新SCP**附加新OU**
    * **分离** 新OU的 FullAWSAccess SCP
    
# 140
* Q
    * 必须为 **EC2 标签** 创建者、**成本中心ID**
    * 已经有Lambda、CloudTrail、*S3*
* A
    * EC2作为事件源 的 EventBridge规则，匹配 CloudTrail，指向Lambda
        * 没有 S3
* R
    * 这里，打标签的过程，不关S3什么事情。其他选项都错
    
# 141
* Q
    * 单独账户 CodePipeline，ECS，ECR
    * 需要 将生产集群 移动 同Region 单独AWS账户，必须私有连接下载镜像
* A
    * ECR **VPC端点，S3网关端点**
    * **主账户中**（**干扰项** 单独的AWS账户中）ECR
    
# 142
* Q
    * 账户中，所有VPC 现在和未来，**都配置 流日志** （flow logs）
    * CloudFormation管理VPC
* A
    * AWS **Config** 检查打开，自动补救
    
# 143
* Q
    * AWS **WAF保护基础设施**
    * 日志**特定模式** 创建警报
    * **最小运营开销**
* A
    * CloudWatch Logs
    * **AWS WAF web ACL** 发送日志
    * **指标过滤器**
    
# 144
* Q
    * Codepipeline 包括： 源、构建、部署阶段
    * 每个阶段 包含 **runOrder 1** 的操作
    * 需要：只 部署 **通过单元测试的**
* A
    * 修改 **构建阶段**，添加 **runOrder 2** 的测试操作。*CodeBuild*运行单元测试
    
# 145
* Q
    * 所有数据传输中加密
    * 发现 S3允许非加密连接
    * **强制实现传输加密**
* A
    * **AWS Confi**g, s3-bucket-ssl-requests-only
    * AWS Systems Manager Automation runbook， **aws:SecureTransport** = false, 拒绝对S3访问
    
# 146
* Q
    * AWS RDS，**AWS Control Tower**
    * 所有数据 静止状态下加密 **（at rest）**
    * 需要收到不合规通知
    * 最高运营效率
* A
    * AWS Control Tower **护栏（guardrail）**
    
# 147
* Q
    * AWS CodePipeline, AWS CodeBuild, and AWS CodeDeploy
    * 部署引入错误
    * 需要：**检测到错误，回滚**
    * 最高运营效率
* A
    * CodeDeploy 设置 **LambdaCanary10Percent10Minutes**
    * 部署组设置自动回滚
    * 创建**CloudWatch警报**
* R
    * A and C: <CodeDeploy to LambdaAllAtOnce>: Replacing all at once would not allow roll back
    
# 148
* Q
    * 1个region下，多az，Aurora，ECR，docker
    * 8小时 RPO，2小时RTO
    * **docker构建 > 2小时**
    * 要求：**DR到另一个region**，最具成本效益
* A
    * Aurora 跨区域复制 自动备份
    * **ECR跨区域复制**

# 149
* Q
    * EC2 程序写入日志（含用户名、日期）
    * 日志发布到 CloudWatch Logs
    * 需要：特定用户 过去7天登录次数
* A
    * CloudWatch Logs Insights， **聚合函数**
    
# 150 - recite
* Q
    * 更改当前的**单个**标签组，只包括Environment=Production标签。添加另一个只包括Name=ApplicationA标签的单个标签组。
* A
    * 更改 only the Environment=Production tag
    * 加 only the Name=ApplicationA tag
* K
    * All tags in one tag group are OR operator. Tags are in multiple tag groups are AND operator

# 151
* Q
    * S3， 三个应用访问，**访问数据之前，对每个进行不同编辑**
* A
    * S3 Object **Lambda 访问点**
    
# [152]
* Q
    * AWS Control Tower and AWS CloudFormation
    * 要求 用 CloudFormation 创建 S3，都必须 用 AWS Key Management Service (AWS KMS) 加密
* A
    * **AWS Control Tower**，**控制（不是检测控制）**（proactive， not detective）
    * 使用 CloudFormation hooks
    
# 153
* Q
    * Lambda 针对 CloudFormation 支持的资源 **漂移检测**
    * 已创建 **EventBridge** **每小时** 执行 Lambda，SNS
    * 实现，检测漂移 **尽快通知**
* A
    * **AWS Config** cloudformation-stack-**drift-detection**-check
    * **建第2个 Eventbridge**
    
# 154 - 读不懂 - recite - 懂了
* Q
    * 每个实例 凭证 限制
    * 配置实例的**安全性**
    * 其实：就是想让IP锁实例
* A
    * aws:EC2InstanceSourceVPC = aws:SourceVpc and aws:EC2InstanceSourcePrivate**IPv4** = aws:VpcSource**Ip**
    * **SCP应用于OU**
    
# 155
* Q
    * CloudFormation堆栈 **成功更新后**，**对特定实例应用标签**
    * 已创建Lambda，应用标签
* A
    * **EventBridge**,匹配UPDATE_COMPLETE
    
# 156
* Q
    * 2个region，分别部署 应用程序+S3。访问同region的
    * S3配置了双向复制
    * 如果：**复制失败，则重试**
* A
    * **Lambda，S3批量操作**，重试失败对象
    
# 157
* Q
    * 需要**故障转移**
    * CF， ALB 在一个region
    * 次要region热备，实现0秒 RTO
* A
    * 次要ALB 做 CF新源，**原ALB做主源**。
        * 题干没说 Route53，所以在cf上做文章
        * 因为有CF，也不能在CF这层上转移，只能是CF的源做转移
        * 2个CF更不合理
    
# 158
* Q
    * 云团队 使用 Org  和 IAM Identity Center (AWS Single Sign-On)
    * 研究团队 可以管理所有资源，但不能创建 IAM用户
    * **研究团队，不能创建 IAM用户**
* A
    * **SCP， 拒绝 iam:CreateUser**
    
# 159
* Q
    * SQS -> Lambda -> S3
    * Lambda批处理 10， 几秒钟
    * 队列 积累速度 > 处理速度，错过处理时间线，许多无效数据
* A
    * Lambda批处理**大小不变**，报告失败的批处理项目。**死信队列**
    
# 160
* Q
    * Org，使用IP范围 分配 VPC CIDR 和 非云
    * **防止IP范围 外 执行AWS操作**
* A
    * **SCP，拒绝，之外，附加根**
    
# 161
* Q
    * Org
    * S3 更改**IP地址限制范围**，**撤销**2个OU的访问权限
* A
    * 基于**资源的策略**，只允许IP范围 访问S3。**新SCP** 拒绝访问S3，附加2OU
* K
    * SCPs cannot directly enforce IP-based restrictions because SCPs manage permissions at the account level rather than at the resource level.
    * An SCP denying access to the S3 buckets is valid, but the IP range enforcement must be done at the bucket policy level.
    
# 162
* Q
    * Org
    * 希望 **防御性、侦查性** 管理账户，确保安全
        * preventive and detective
* A
    * **AWS Control Tower landing zone**

# 163
* Q
    * CodeBuild单元测试，通过，必须标记最新的**提交**
* A
    * CodeBuild**原生Git** 克隆，**原生Git**创建标签
    
# 164
* Q
    * ECS运行在几个EC2上
    * 记录 审查 所有停止**任务**以差错
* A
    * **Eventbridge，** CloudWatch Logs，CloudWatch Logs Insight
* K
    * Lifecycle hooks are triggered during EC2 instance scaling events, not ECS task state changes. 

# 165
* Q
    * CodeDeploy
    * 部署期间，必须一定数量实例可用
    * 必须一次 重新路由**一半**到新环境实例
* A
    * 蓝绿部署
    * 使用 CodeDeployDefault.**HalfAtAtime** 部署
    * BeforeAllowTraffic 钩子， 删除临时文件

# 166 - 题库错
* Q
    * Org, AWS Control Tower landing zone
    * 所有账户 使用 **AWS Control Tower controls (guardrails)**
    * 需要：所有账户 配置 **初始基线**
* A - 题库答案错误，应为A
    * 一个基线配置， AWS Control Tower Account Factory **Customization** (AFC) 
    
# 167
* Q
    * AWS Config 自定义 Lambda
    * **监控** ECR ， 策略语句，**执行 ecr:***
    * 不合规时，SNS通知
    * 当评估自定义AWS Config规则时，
    * **Lambda函数无法运行**
* A
    * 修改 Lambda **资源策略**，授权 AWS 调用权限
        * 问的是 Lambda不能访问，所以和ECR访问权限没关系
    
# 168
* Q
    * 为概念验证 考虑 **创建** 服务链接**IAM角色**
* A
    * **创建 IAM角色**，**附加边界**到该角色
    
# 169
* Q
    * 希望减少开发新功能时间
    * CodePipeline CodeDeploy
    * 需要：新功能 **发布平均时间**、失败部署后恢复平均时间
    * **最少配置**
* A
    * Lambda， **CloudWatch自定义指标**，EventBridge，仪表板
        * **不用** 每5分钟 调Lambda
        
# 170
* Q
    * CloudFormation，S3， 无法干净删除堆栈
    * 如何缓解当前和未来
* A
    * 原因 S3 不为空，Lambda函数，RequestType为Delete，清空桶

# 171
* Q
    * EC2 **审计** **禁止安装的应用**
* A
    * 每个实例配置 AWS Systems Manager， 
    * Systems Manager **Inventory**， 
    * AWS **Config** 监控 Systems Manager Inventory 改变
    
# 172
* Q
    * EKS，AWS Securets Manager， KMS
    * 已经 assume Kubernetes role 访问 密码
    * 访问被拒绝 403 Forbidden。原因？
* A
    * **密钥的 policy 策略** 不允许 **Kubernetes**账户IAM角色 使用
        * 尝试 Securets Manager，所以不是 KMS
        * EKS集群不访问，是K8s服务账号
    
# 173
* Q
    * 本地数据中心，迁移 混合环境 **4 region**
    * **NetApp ONTAP**
    * 数据，始终一致，去重
* A
    * 每个region，建 Multi-AZ Amazon **FSx for NetApp ONTAP**
    * **SnapMirror**
    
# 174
* Q
    * EKS 托管 ML
    * 随 ML 增大， **pod 启动时间增加**
    * 需要：启动需要**减少到几秒钟**
    * 已经创建 EventBridge规则，AWS System Manager自动化，ECR
    * 配置了集群、节点标签
* A
    * 建 IAM角色、策略，在**EKS集群****节点**运行命令，
    * 创建System Mangager State Manager 关联（association）
    * 使用 节点 **标签**预提取镜像
    
# 175
* Q
    * CodeDeploy CloudFormation ECS **蓝绿部署**
    * 必须每分钟**10%**流量转移
* A
    * AWS::CodeDeploy**BlueGreen 转换**
    * AWS::CodeDeploy::**BlueGreen 钩子**
    * CodeDeployDefault.ECS**Linear**10PercentEvery1Minutes 部署配置
        * 没说金丝雀
    
# 176
* Q
    * Lambda 创建新账户
    * Lambda 在专用账户中
* A
    * 管理账户
    * 建IAM
    * 允许 该角色 **被 Lambda执行角色** 在**新**账户assume role
    
# 177
* Q
    * 一个Region部署 DynamoDB、S3
    * 希望部署次要Region，数据必须**立即跨Region传播**
    * 最高操作效率
* A
    * S3 主要 次要，**双向复制**
    * Dynamo**全局表**，次要Region 为附加Region
    
# 178
* Q
    * RDS 自动缩放， CloudWatch仪表板上可视化
* A
    * **EventBridge**规则，响应缩放事件
        * 不是 CloudTrail
    * **Lambda** 发布 CloudWatch自定义指标
        * 没有配置警报的必要呀

# 179
* Q
    * 解决：创建 标准的基础**镜像**。每周发布到**3个region**
* A
    * **ImageBuilder**，流水线配置，**分发到3个**region(直接，不再多次复制) 的 ECR，每周执行
    
# 180
* Q
    * 所有EC2安装防病毒软件
    * 检测所有EC2，病毒软件未安装时
    * **软件没装时**，用**AWS SystemManager文档**来安装
* A
    * Systems Manager **State Manager**，软件包含在**关联**中

# 181
* Q
    * 所有EC2 都使用批准的 AMI
    * **纠正 未经批准**
    * 个别账户**不能移除限制**
* A
    * **AWS Config**， approved-amis-by-id
    * AWS **Systems Manager**自动化运行簿，AWS-StopEC2Instance
    * 部署 合规性包
    
# 182
* Q
    * 工程师 能 **假定 管理员角色**
    * 希望：假定时，近乎实时收到通知
* A
    * AWS **API调用** 和 **CloudTrail**事件模式
    * EventBridge Lambda SNS
    
# 183
* Q
    * App Runner中运行容器
    * ECR存储容器
    * 方案：**检测 操作系统漏洞 语言包漏洞**， 创建新的容器镜像
* A
    * **Image Builder**
    * ECR 开启**增强**扫描
    
# 184
* Q
    * AWS Systems Manager 文档，为**物理笔记本**电脑 进行引导
    * **引导代码 Github**
* A
    * 使用**aws-downloadContent**插件，并设置**sourceType为GitHub**
    
# 185
* Q
    * 数据库团队 软件开发团队，各自单独 CloudFormation
    * **软件开发团队 需要使用 数据库团队 资源**
    * 希望 **各自保持** 管理 审查
* A
    * 从数据库CloudFormation**模板创建堆栈导出**，并将其**导入到Web**应用程序CloudFormation模板中
    
# 186 - 图
* Q
    * IAM策略，S3策略，都是CloudFormation部署
    * 模版和代码，都在CodeCommit
    * 最近，一些 账户 无法访问 S3
* A
    * **确保** **SCP没有阻止** S3 访问，确保 **IAM策略边界** 没有阻止访问
        * 确保，这个表明了意图。Ensure
        * 不想再读题了，又臭又长
    
# 187 - 图
* Q
    * ![](vx_images/107641105887896.png)
    * config.txt deployment 结果
* A
    * 两个路径映射
        * 消息名字，也别点错了

# 188
* Q
    * Auto Scaling EC2，处理记录，多步骤顺序操作
    * 当前限制，**步骤失败，也从头开始**
    * 希望，只重新处理失败步骤
* A
    * **Step Function**
    
# 189
* Q
    * Windows Linux 应用 迁移 AWS
    * 需要访问SMB和NFS
* A
    * Amazon FSx for **NetApp ONTAP**
    * 配置NetApp **SnapMirror**复制功能
    
# 190
* Q
    * 视频用户 10000 -> 50000，出现登录问题
    * 汇总视图，查看资源行为、交互
        * 登录尝试，api调用，网络跟踪，EKS潜在的恶意行为
    * 最小化管理日志开销
* A
    * Enable Amazon **GuardDuty** for EKS Audit Log Monitoring.
    * Enable Amazon **Detective**
    
# 191 - 题库错
* Q
    * ALB EC2 多个可用区中。原先配置在一个可用区，导致部分中断
    * 需要**测试故障转移**
* A
    * ~~**关闭 ALB 跨可用区负载均衡**~~
        * ~~not on the ALB’s **target group**~~
    * B。关闭 ALB 的 **目标组** 的 跨可用区负载均衡
        
# 192
* Q
    * AWS Network Firewall **流日志 S3**
    * Athena分析
    * 入S3前，转换
* A
    * **Kinesis Data Firehose**， Lambda转换器
    
# 193 - 图 - 看不懂 太长
* Q
    * 两个团队 通过 IAM Identity Center 访问账户
    * 安全团队 希望 DevOps团队 无法访问组织管理账户的 IAM Identity Center
    * 已经设置SCP，但仍可以被访问
* A
    * **选最长的**。最后有**IDelete**
    * 在  IAM Identity Center 中更新 DevOps 权限集
    
# 194
* Q
    * AMI 安装了 System Manager Agent
    * EC2 启动 Auto Scaling组 会应用标签
    * 要求：**EC2必须具有正确操作系统配置**
* A
    * Systems Manager **State Manager**
    
# 195 - 太长 会 但背
* Q
    * Org
    * root -> Department OU -> Engineering OU 都附加 FullAWSAccess
    * **计划移除 FullAWSAccess， 允许 EC2 API**。发生什么变化
* A
    * **允许 EC2，拒绝 其他API**
    
# 196
* Q
    * CloudWatch Logs **数据发送到S3**
    * 支持所有现在和未来
* A
    * 目的地 **Kinesis Data Firehose**，S3作为流的目标
        * 题目不是备份，其他选项都是备份
        
# 197
* Q
    * 单Region中：EKS, Aurora **MySQL**, **ECR**, CodeBuild
    * 复制到**第2个Region**
    * 最有效操作方式
* A
    * 启用ECR **跨区域复制**
    * 启用Aurora**全球数据库**
    
# 198
* Q
    * BeginResponse Lambda 后，调用大量 Lambda
    * 每个必须并发，且支持重试。不丢数据
* A
    * **每个Lambda一个SQS**，**每个SQS订阅到SNS** （fan-out）
    * 创建**一个SNS**
    
# 199
* Q
    * 多region， API Gateway 冗余部署，自定义域 URL，**优化用户请求性能**
* A
    * 每个 region 部署 一个 API Gateway
    * **Route53 延迟路由策略**
    
# 200
* Q
    * CodeBuild 频繁生产软件包，**大型docker镜像**，在多个构建中使用这些镜像
    * 希望提高构建性能，最小化成本
* A
    * ECR存储镜像，CodeBuild **Docker层缓存**
    
# 201
* Q
    * 大公司 收购 小公司
    * 限制使用 **t3.small** EC2，在**美国** region
    * SCP
* A
    * **拒绝** EC2 条件 不等于 t3，**拒绝**EC2 条件 区域不等于 us
    
# 202
* Q
    * 处理时间延长，SQS **队列超出预期大小**
    * 需要警报
    * 最高操作效率
* A
    * CloudWatch警报，ApproximateNumberOfMessages**Visible**
        * 其他答案都不是Visible
        
# 203
* Q
    * S3存储机密数据
    * 希望 **洞察各种活动，检测任何问题**
* A
    * CloudWatch异常检测，**Athena**
    
# 204
* Q
    * EKS，希望 根据**CPU利用率 pod自动扩展**
    * 最有效操作方式
* A
    * 水平pod自动扩展**（HPA）**，**Kubernetes自动扩展**，配置集群扩展自动发现
    
# 205 - 图 - 基本重复 19

# 206
* Q
    * 希望CloudTrail监控，确保账户**不能关闭 CloudTrail**
* A
    * **SCP** 拒绝cloudtrail:StopLogging操作和cloudtrail:**DeleteTrail**操作
    
# 207
* Q
    * 三层应用 蓝绿部署
    * ALB EC2
    * **一次性** 流量 蓝 -> 绿
* A
    * 绿环境 扩展组 滚动重启 部署版本
    * **滚动重启完成后**
    * **CLI 更新 ALB 切流量**
    
# 208
* Q
    * 自动扩展 EC2 处理 SQS
    * 处理一组消息 几小时，**CPU利用率 没到扩展阈值**
    * 队列需要被快速处理
    * 最小操作开销
* A
    * 创建 **目标跟踪扩展策略**
    * ApproximateNumberOfMessages**Visible**
    * GroupInServiceInstances
    * 指标**数学计算**
        * 不用Lambda（其他选项都用Lambda）
        
# 209
* Q
    * 数百个EC2
    * 所有**必须附加 实例配置文件**。附加到 任何没有附加的实例
* A
    * AWS Config **ec2-instance-profile-attached**
    * AWS Systems Manager Automation runbook 附加
此题 pdf #13    

# 210
* Q
    * 全球数据库
    * 2个次要region （集群）
    * RPO 60s
    * 写操作 偶尔阻塞 因为RPO设置
* A
    * 移除一个次要集群
* K
    * 在 Amazon Aurora 全球数据库 中，当主集群与次级集群之间的复制延迟接近 RPO（恢复点目标，Recovery Point Objective） 的设置值时，Aurora 会暂时阻止主集群的写操作，以确保跨区域的
    
# 211
* Q
    * Python CodeCommit CodePipeline
    * 代码扫描 部署生产环境前
    * 发现漏洞，**停止部署**
* A
    * 配置Amazon **CodeGuru Security** 扫描代码，**”引发错误“**
    
# 212
* Q
    * EC2 fleet
    * 希望： 监控 应用程序 网络流量、内存利用率 **之间的关系**，60s 间隔
* A
    * CloudWatch**详细**监控 NetworkIn，EC2安装 代理 **mem_used**
    
# 213
* Q
    * AWS System Manager 管理一系列 安装了 SSM Agent的EC2
    * 希望所有新实例，**自动由 AWS System Manager 管理**
* A
    * 创建IAM角色（**因为agent已经安装**）
    * 附加策略 **AmazonSSMManagedEC2InstanceDefaultPolicy**
    * default-ec2-instance-management-role
    
# 214
* Q
    * 为Lambda函数配置 **S3 事件源**
    * 创建或修改 触发
    * 发现：**Lambda未执行**
* A
    * **Lambda资源策略**，授权S3调用Lambda
    
# 215
* Q
    * ALB -> 3 AZ
    * 1 AZ 测试 版本，有问题
    * 希望：**流量从问题AZ转移，程序保持静态稳定性**
    * 最高操作效率
* A
    * ALB 目标组 **禁止跨区域 负载均衡**
    * 发起ALB **区域转移** （**zonal shift** on the ALB）
    * 先 禁用， 再转移
* K
    * AWS offers Zonal Shift (through AWS Elastic Disaster Recovery or AWS Global Accelerator) to temporarily shift traffic away from a specific Availability Zone. This is highly operationally efficient because it allows automated rerouting without manual changes to target groups or instance configurations.
    
# 216
* Q
    * 多账户，每个运行 Amazon Connect
    * 每个账户，用EventBridge默认事件总线
    * 希望：**在DevOps账户中接收事件**
* A
    * 更新 **DevOps resource-based policy**，允许**接收**事件，目标为**DevOps的默认总线**
    
# 217
* Q
    * EC2 + FSx for ONTAP
    * 没有数据存储中 EC2 上
    * **10分钟RPO**
* A
    * **FSx for ONTAP**
    * 5分钟， NetApp **SnapMirror**计划
    
# 218 - 答案错
* Q
    * 审计发现 SSH 0.0.0.0/0 流量
    * 检测并解决
* A
    * AWS Config，**restricted-ssh**，Lambda修复
* K
    * Why not A. Use vpe-sg-port-restriction-check rule:
        The vpe-sg-port-restriction-check rule is a managed AWS Config rule, but it may not cover specific requirements for SSH traffic monitoring.
        It also requires a periodic trigger, which introduces delays compared to real-time monitoring of configuration changes.
        
# 219
* Q
    * 30个本地VM + 15个EC2 安装软件包
    * 监控漂移，警报
    * **Direct Connect**
* A
    * 安装AWS Systems Manager **Agent** (SSM Agent)
    * AWS **Config**监控，通知SNS
    
# 220
* Q
    * 安全团队 发现 开发人员 和 用于部署的 角色 拥有比必要多的权限
    * 开发人员 不得具有 **额外权限**
* A
    * 创建 IAM **权限边界** （IAM permission boundary policy）
    * **更新**CDK引导程序（bootstrapping）使用权限边界
    
# 221
* Q
    * ECR 扫描镜像 软件包漏洞
* A
    * ECR**增强**扫描
* K
    * Basic continuous scanning only scans images at the time of push and lacks real-time scanning for vulnerabilities in existing images. It provides less thorough coverage compared to enhanced scanning.
    
# 222
* Q
    * AWS System Manager 执行维护任务
    * EC2实例 收到 AWS Health 通知后重启
    * EventBridge 通知
* A
    * AWS **Health作为事件源**
    * AWS-**RestartEC2Instance** Systems Manager Automation运行手册以重新启动EC2实例
    
# 223
* Q
    * CodePipeline， 部署成功后，URL 返回200 
    * **发现返回503**
    * 需要：自动化测试，部署后返回200。添加 **CheckURL阶段**
* A
    * **Lambda，** 检查响应码，报告给CodePipeline
    
# 224
* Q
    * CloudWatch日志 包括字段 响应码 和 程序名称
    * 希望：**建指标（metric）**，跟踪请求数量
* A
    * 建 CloudWatch Logs **度量过滤器（CloudWatch Logs metric filter）**
    
# 225
* Q
    * EKS 与 OpenID Connect (OIDC) 关联
    * 配置集群通用卷（EBS）
    * 发起 PersistentVolumeClaim (**PVC**) 请求
    * **EC2:UnauthorizedOperation error**
* A
    * 创建 IAM角色
    * EBS Container Storage Interface **(CSI) **driver
    
# 226
* Q
    * EKS，允许多个pod
    * 需要：**配置pod身份访问**。已经用pod身份更新了node IAM 角色
* A
    * 为EKS 创建 IAM OpenID Connect (OIDC)提供商
        * 为EKS集群创建一个IAM OpenID Connect (OIDC)提供商是配置Pod身份访问所必需的，允许pod使用节点IAM角色安全地访问AWS资源

# 227
* Q
    * 多账户
    * 需要：**识别**所有账户中，使用最多的用户和角色
    * **最高操作效率**
* A
    * **userIdentity.arn**
    * **CloudTrail**，CloudWatch **Contributor Insights**规则
    
# 228
* Q
    * 对所有EBS和SQS，在**CloudFormation堆栈** 创建或更新**之前**，**强制执行服务端加密**
* A
    * CloudFormation **StackSets信任访问**， 创建CloudFormation **hook**，强制执行加密，添加到OU
* K
    * While SCPs can prevent non-compliant actions, they cannot validate specific configurations within a CloudFormation stack

# 229
* Q
    * 配置存储库，存储容器镜像。超过15天自动删除
    * 最高操作效率
* A
    * **ECR，生命周期策略**
    
# 230
* Q
    * 所有资源上 使用 CostCenter标签
    * 一些开发人员 使用CostCenter标签 + 有人为了躲避，加了任意字符串
    * 需要：确保所有EC2都使用适当的标签
* A
    * 创建**SCP**，标签值来自 **成本中心 列表**

# 231
* Q
    * 每个EC2，包含 web应用、数据库、redis
    * 请求负载 到 单个EC2
    * 希望：分离程序组件，提高可用性、性能
* A
    * 一个均衡器、一个扩展组、**多AZ** Aurora、ElasticCache
        * 错误项，DNS目标类型组
    
# 232
* Q
    * AWS资源访问，所有账户，限制在2个region    (限制region，要靠SCP)
    * 特定一组授权服务中
    * Active Directory认证
    * 访问权限按只能组织
* A
    * 建立 **服务控制策略**（service control policy  AWS Service Control Policy (**SCP**)）限制权限
    * 使用 **CloudFormation StackSets 配置角色**
    
# 233
* Q
    * 检测到 账户 异常登录
    * 解决：**当多次登录失败** 尝试 发通知
* A
    * CloudTrail 发 **管理事件** CloudWatch Logs。 建 **度量过滤器（metric filter）**
        * 不是 数据事件
    
# 234
* Q
    * API Gateway 只能从 **特定VPC访问**
* A
    * **资源策略**， 只允许 **VPC ID**
        * 需要从VPC访问，配置IAM是没有用的
    
# 235
* Q
    * ECS ALB 服务稳定
    * **更新**容器镜像，更新任务。服务**经常停止并重启**
* A
    * 增加 ALB **健康检查宽限期**
    
# 236
* Q
    * 需要：自动化 操作系统、应用程序 补丁部署，同时 默认操作系统和自定义补丁仓库
    * 最省力
* A
    * AWS System Manager 创建**一个补丁基线**
        * 混淆答案：创建2条基线
        
# 237
* Q
    * ALB + EC2, EC2上运行**Docker**。Ec2上运行 **MySQL**
    * **无服务架构**，最少更改
* A
    * **Fargate， Aurora Serverless**
    
# 238
* Q
    * 使用 CloudFormation 在**所有账户中部署一个 IAM角色**
    * 最小操作开销
* A
    * 创建具有 **服务管理权限** SCP CloudFormation StackSet，**根OU部署**
        * 最后一个选项，多加的CloudFormation堆栈，**和题目没关系**

# 239   - 答案错 -- 同 218
* Q
    * 记录AWS资源配置，检查问题，发送通知
    * 0.0.0.0/0 22端口
    * 定期拍摄 资源配置快照
* A
    * AWS Config 定期记录 （use periodic recording）
    * ~~vpc-sg-port-restriction-check 规则~~
    * ssh-restricted
    
# 240
* Q
    * CloudFront分发
    * 需要：只对 **IP地址范围 开放**
    * **AWS WAF 网络ACL**默认 为 计数
* A
    * 创建 IP地址范围匹配 **WAF IP地址集**。优先级 0
    * **网络ACL**默认操作为**阻止**
    
# 241 - 答案错
* Q
    * 需要：收集 **应用程序指标、自定义指标**
    * S3 前转换
    * 收集命名空间的任何新指标
    * 最小操作开销
* A
    * CloudWatch 指标流，包括 **应用程序 和 命名空间** 指标
    * Data Firehose
* 应该是A
    * 其他选项，没有说这2种指标。所有指标涵盖范围又太大

# 242
* Q
    * CodeBuild ECR EKS
    * 镜像 部署到EKS **之前** **签名**
    * 定期轮换，追踪谁签的
* A
    * CodeBuild**构建**，AWS Signer 对镜像 **签名**， CloudTrail追踪
        * 只有这项，是在 **推送到ECR之前**，签名的
    
# 243
* Q
    * Python in CodeArtifact
    * 需要： **授权 EC2 访问 **CodeArtifact
* A
    * IAM 角色 的 **实例配置文件**（instance profile），与 EC2实例关联
    
# 244
* Q
    * 每天特定时间，产生EC2上文件
    * 删除数据库记录 > 60天
    * 先 文件，再 数据库记录 删除
    * **最小开发量**
* A
    * AWS Systems Manager State Manager 自动调用 Systems Manager Automation document
    * EventBridge -> **SNS**
        * Lambda有开发量。SES不合适。4个都可以
    
# 245 - 答案错
* Q
    * Org
    * **阻止** 不使用 IMDSv2 的 EC2实例 **AWS API调用**
* A
    * ~~SCP, ec2:MetadataHttpTokens 条件关键字不等于 required 值~~
    * ~~拒绝 **ec2:RunInstances**~~
    * D is right
        * ec2:MetadataHttpTokens
        * 拒绝操作（相当于 *）
    
# 246
* Q
    * CloudFormation 更新部署失败
    * 一些资源被手动修改
    * 解决：手动修改发报警
    * 最小操作努力
* A
    * **SNS**，AWS Config *CLOUDFORMATION_STACK_DRIFT_DETECTION_CHECK*
    * **Eventbridge** **NON_**COMPLIANT
        * 不走Lambda，省的写代码
* R
    * 其他选项排除
        * B。（RDKlib）
        * C。COMPLIANT
        * D。没有SNS
    
# 247
* Q
    * **自动化** AWS Control Tower guardrails **护栏**， 应用于所有OU
    * 安全团队 希望 限制护栏类型，只允许它批准
    * 最高操作效率
* A
    * CloudFormation模版
    * 为**每个OU**，在模版中创建 AWS::ControlTower::EnableControl logical resource
    * 在安全团队 中 **配置 AWS CodePipeline** pipeline
        * 不是配置项目
    
# 248
* Q
    * AWS WAF， CloudWatch Logs
    * 希望：被阻止流量 突然变化 警报
* A
    * **AWS WAF** *被组织请求* 创建 CloudWatch Logs 指标过滤器
    * **CloudWatch 异常检测**
* R
    * 4个选项都可以。只有A最灵活 简单

# 249
* Q
    * S3 MP4视频
    * CloudFront分发， URL包含授权令牌
    * 需要：CloudWatch记录，实现对前端令牌检查
* A
    * **CloudFront函数(function)**， 启用CloudWatch
        * 混淆答案：**第二个**
        * Lambda@Edge, it has higher cold start latency compared to CF Functions
        
# 250
* Q
    * 强制执行标签
    * **促进代码重用**
    * 避免部署SQS配置不同
* A
    * AWS **CDK** 标记 CloudFormation **StackSet**
    * AWS CDK 定义 SQS
    
# 251
* Q
    * 希望：特定资源 **配置 发生变更** 自动回滚
* A
    * AWS **Config** 检测变更，AWS Systems Manager Automation文档回滚
    

-----
多选题

# 252 - [授权]
* Q
    * **账户间 共享AMI 加密**
    * KMS
    * Auto scaling组 从AMI启动EC2
* A
    * A 在源账户中，AMI复制为加密，**指定KMS密钥**
    * D 源账户中，**修改 密钥策略**。目标账户中，**创建KMS授权**， **服务链接角色**(Auto Scaling service-linked role) （选详细的）
    * F 在源账户中， 共享 **加密的**AMI
    
# 253
* Q
    * **Codepipeline** 自动化部署程序
    * 应用程序打包**RPM**
    * 共同的AMI启动 （common）
* A
    * A。 AMI 带 CodeDeploy**代理**。 **IAM role**
    * D。CodeDeploy创建应用。**Auto Scaling组**为部署目标（**不是EC2**）。更新 CodePipeline使用**CodeDeploy**操作。
    
* W
    * B: no need to grants AppSpec file access to code CodeDeploy
    * C：缺更新管道
    * E：不能部署到EC2
    
# 254
* Q
    * 公司要求 ALB、 API Gateway 都于 AWS WAF Web ACL**关联**
    * 发现 **没关联的**
    * 需要：**防止**未来的**违规**
* A
    * A。**AWS Firewall Manager**委托给安全账户
    * C。创建**AWS Firewall Manager**策略，附加ACL（是自动的）
    
# 255
* Q
    * AWS Control Tower **着陆区**
    * AWS IAM Identity Center *(AWS Single Sign-On)， IdP，SAML 2.0*
    * 希望：**强大权限模型**（robust），最少授权原则
* A
    * B。创建 **权限集**（permission sets）， 附加内联策略（inline policy），aws:PrincipalTag
    * C。IdP建组，将组*分配给* **账户**和IAM Identity Center**权限集**
    * F。启用 IAM Identity Center， **IdP映射** as key-value pairs （没标签的事情）

# 256
* Q
    * 订单处理 延迟
    * SQS -> Lambda(**保留并发** reserved) -> DynamoDB（auto scaling）
* A
    * 增加 Lambda **并发限制**
    * 增加 **WCU** 最大值
    
# 257
* Q
    * 希望 使用 eu-west-1 管道 将 Lambda 部署到 us-east-1
    * 使用 aws cloudformation package AWS CLI 命令构建 Lambda函数代码.zip
* A
    * C。us-east-1 **创建S3**，允许**CodePipeline**访问
    * D。 **修改管道**，包括 us-east-1 S3，创建新的 CloudFormation
    
# 258 - [授权] 会，但背不对
* Q
    * 工作负载账户，增加运营团队，运营团队 成员  获得 工作负载账户 管理员权限
* A
    * B。每个**工作负载**账户 创建 SysAdmin **角色**。 AdministratorAccess 策略。允许 sts:AssumeRole
    * D。**运营账户** 中 创建 每个运营团队 成员 **IAM用户**
    * E。**运营账户** 创建 SysAdmin**s** **IAM用户组**。进行 sts:AssumeRole
* R
    * 先去除 Cognito选项2个
    * 剩下4个中。因为 运营账户 要获得 每个工作负载账户的管理员权限，
        * 就应该在 每个 工作负载账户 中 创建 SysAdmin角色  


# 259
* Q
    * ECS ALB，多个ECS服务
    * 需要 **收集**应用程序和访问**日志** 将日志**发送到S3** 准实时分析
* A
    * ECS上安装 CloudWatch Logs **代理**
    * **激活***ALB*上访问日志记录，指向S3
    * **Kinesis Data Firehose**
    
# 260 - [授权]
* Q
    * 私有 S3，**敏感**对象，**不同AWS区域和账户** 复制
* A
    * A。在 **源**账户 创建 **IAM 角色**
    * D。在**目标** S3 **策略** 加 允许IAM角色 复制对象 【获得 跨账户 访问目标资源的能力】（允许你来操作我）
    * E。在 **源**S3 创建启用 复制规则 【**在源的一方，发起复制**】
* R
    * 从选项**E开始解题**。复制操作是**从源开始**的
    
# 261 - [授权]
* Q
    * **管理账户** Lambda **检索** **成员**账户 EC2 **安全组** 出入站规则
* A
    * B。创建**信任**关系，允许 **管理员**账户 的 IAM 角色 （暗指是在成员账户中创建）
    * C。**成员**账户 创建**IAM角色**。访问 AmazonEC2ReadOnlyAccess **托管策略**【起点】
    * E。**管理员**账户 **创建IAM角色**，允许**对成员**账户 sts:AssumeRole 【role套role】
* R
    * 2 2一组。中间成员 给 具体策略
    
# 262
* Q
    * 一个EC2，一个NAT实例
    * 需要：高可用性
* A
    * B。**跨多可用区EC2**，应用程序负载均衡器 ALB
        * C选项，没说跨AZ，不合适
    * D。**每个 可用区** NAT网关
    
# 263
* Q
    * API Gateway **高延迟**
    * 需要 确定原因，**不引入额外**延迟
* A
    * 安装CloudWatch**代理**
    * AWS X-Ray跟踪，**守护进程**
    
# 264
* Q
    * EC2 + 本地配置（on-premise）运行应用
    * 2个环境 打补丁，非工作时间
* A
    * A。**混合激活** *物理机* (Systems Manager Hybrid Activations)
    * B。*EC2* 附加 **IAM角色**
    * F。Systems Manager Maintenance Windows **维护窗口**
* K
    * A B 都是为了让 Systems Manager 能管理

# 265
* Q
    * EC2漏洞扫描 Amazon Inspector
    * 发现： **“未扫描”**的EC2实例
* A
    * 方向集中在**通讯**上
    * A。是否安装 AWS Systems Manager **Agent**
    * B。*安全组* 允许 AWS Systems Manager service endpoint. **443**
    * E。EC2 *配置文件* 授权 AWS Systems Manager **通讯权限**
    
# 266
* Q
    * Codepipeline
    * 希望 运行 **渗透测试**工具 **手动测试**
    * 工具通过 *REST API* 调用
    * 2个选项
* A
    * **管道** 2个 步骤间 **插入**一个*手动批准*
    * **更新**管道，调用**Lambda**来调用REST API
    
# 267
* Q
    * **笔记本电脑** 测试 CloudFormation。*容易错*
    * 希望：*自动化*部署，手动批准。代码、**CloudFormation模版**到 CodeCommit
* A
    * *CodeDeploy *应用程序组 部署组，EC2**安装代理**
    * *CodePipeline*，每个程序创建 **CloudFormation更改集**，手动批准。运行 **CloudFormation更改集** 启动 CodeDeploy部署 
        * (CloudFormation change sets)
    
# 268
* Q
    * **单一**区域部署。ALB+扩展组+EC2+DynamoDB
    * 公司在不同大陆上开设 新办公室
    * 需要：**最小化响应时间**，**2个**区域可用性
* A
    * C。**新区域** 加 ALB 扩展组
    * D。route53 **延迟**路由
    * F。DB**全局表**
    
# 269 - 看不懂 - 会了
* Q
    * 集中配置 所有账户的 AWS Config
    * 资源更改 记录到中央账户
* A
    * A。AWS Config 配置 **委派管理账户**，**启用受信任** 访问
    * E。在 **委派管理账户** 创建 AWS Config **组织聚合器**。配置收集数据
* K
    * 委派管理账户
        * **delegated administrator account**。is an account in an AWS Organizations that is **granted** additional **administrative** permissions for a specified AWS service. This means that in addition to the management account, you can also use a delegated admin account to **aggregate** data from *all the member accounts* in AWS Organizations without any additional authorization.
    * AWS Config 组织聚合器
        * **AWS Config organization aggregator**。An aggregator is an AWS Config resource type that collects AWS Config configuration and compliance data *from **multiple** AWS accounts and Regions **into** a **single** account* and Region to get a centralized view of your resource inventory and compliance
    
# 270 - 看不懂 - 会了
* Q
    * 让 所有用户 使用 AWS IAM Identity Center (AWS Single Sign-On)
    * 禁用 新IAM用户凭证（new IAM user）并通知
* A
    * A。EventBridge，响应 CloudTrail 的 IAM **CreateUser** API
    * C。Lambda，**禁用** *IAM用户关联的* **密钥**，**删除** IAM用户关联的 **登录配置文件**
    * E。SNS
    
# 271
* Q
    * 日志在CloudWatch Logs
    * 希望 归档 S3。90天后很少访问，保留10年
* A
    * Kinesis Data **Firehose** 传输
    * > 90 **Glacier**
    
# 272
* Q
    * 程序用 Lambda。金丝雀部署。故障自动回滚
    * 需要： 创建IaC代码，CI/CD流水线
* A
    * B。*创建* **AWS Serverless Application Model** (AWS SAM) 模版 定义每个Lambda
    * C。CodeBuild*部署* **AWS Serverless Application Model** (AWS SAM) 模版
    * F。CloudWatch警报 （**不是** 复合警报）
    
# 273
* Q
    * CodeDeploy部署EC2，**部署失败**
    * 与特定部署ID相关的，都**跳过状态**
    * 原因
* A
    * A。**网络配置** 不允许EC2 访问互联网
    * D。**实例配置文件** 没有附加到EC2
* K
    * When EC2 instances are part of a CodeDeploy deployment group, they need to have an associated IAM **instance profile** with the necessary permissions to *interact with CodeDeploy* and download the deployment artifacts.
    
# 274
* Q
    * S3下载 **AccessDenied** 原因
* A
    * **桶策略** 有错误
    * **IAM角色** 有错误

# 275
* Q
    * 故事：本地数据 迁移 S3，1个月旧数据归档S3
    * 审查发现：脚本**没有验证** S3 数据是否复制成功
    * 改脚本，确保传输完整，MD5验证数据完整性
* A
    * B。**Conten-MD5**
    * D。检查返回 **ETag**
    
# 276
* Q
    * MySQL + EC2
    * 2小时 RPO，10分钟 RTO
    * 故障转移
* A
    * B。2个region，Aurora**全局数据库**
    * D。route53 **故障转移**路由。 2个region
    
# 277 - 不懂
* Q
    * Org AWS Control Tower**新着陆区**。证明符合 **（CIS）** 基准
    * 安全团队 AWS Security Hub 查看合规性
    * 所有账户创建后，必须注册Security Hub
* A
    * A。组织的管理账户中，**用AWS Control Tower创建账户**，Security Hub **提供 CIS基准** （长）
    * C。创建 AWS IAM Identity Center (AWS Single Sign-On) **权限集** （长）
    * E。Security Hub， **启用自动启用功能**（短）
    
# 278 - 图 - 答案错
* Q
    * **CodeArtifact** 存 Python库
    * VPC没有互联网
    * 无法从**存储库下载**Python库
* A
    * 关注权限， **存储库 - CodeBuild** 之间
    * ~~B。存储库**策略** Principal，包括 CodeBuild **IAM角色** ARN~~
    * A。 创建S3 网关端点
    * D。 更新CodeBuild **使用的角色**，访问 CodeArtifact **足够的权限**
* R
    * 虽然是争议题，但根据题干，去了公网，那应该是在考网络
 

# 279
* Q
    * 100GB S3 .csv
    * 希望：查询 图表 自动化csv元数据
    * 最少努力
* A
    * Amazon **QuickSight**
    * **Athena**
    * AWS **Glue**
    
# 280
* Q
    * AWS **IoT Greengrass**、公司**本地**服务器、**EC2**
    * 希望：*AWS Systems Manager* 自动化管理、**补丁**
* A
    * C。Systems Manager 为**3种**打补丁，**维护窗口**任务
    * E。为**EC2** 附加 **实例配置文件**。为**IoT和本地服务器** 创建 **IAM角色** (给3种，访问权限)
    * F。本地服务器 安装 代理。更新 IoT **令牌交换**。所有IoT设备上 部署SSM代理
    
# 281
* Q
    * CodePipeline。KMS S3。**CloudFormation操作因访问被拒绝** 错误
* A
    * 围绕权限，**围绕CloudFormation 的权限**
    * B。客户端管理的密钥。**配置KMS密钥策略**，允许 *CloudFormation* 使用 IAM角色。**加密工件**
    * E。创建 Codepipeline IAM角色。**+** 修改 **S3存储桶策略** 允许角色访问。
    
# 282
* Q
    * 应用重写为 Lambda
    * **账户A EFS**
    * 要求 **Lambda**都部署到 **账户B**。允许继续使用现有EFS访问点
* A
    * 更新**EFS文件系统 策略**，为账户B访问授权
    * 更新**Lambda执行 角色**，获得 VPC EFS 权限
    * 创建 账户 A B **VPC对等连接**
    
# 283
* Q
    * 由于CloudFormation模版错误。更新错误，自动**回滚失败**
    * **UPDATE_ROLLBACK_FAILED**
    * 要求：完成回滚
* A
    * AWS CLI **ContinueUpdateRollback** 命令
    * **手动调整** 资源 匹配堆栈 **期望**
    
# 284
* Q
    * 手动 *本地构建*
    * *本地缓存*，每次部署 必须清除
    * 想要 迁移到CI/CD
* A
    * 核心：围绕自动构建服务 （**另外2项，没有相关服务**）
    * B。自定义脚本 **清缓存。BeforeInstall** 钩子
    * D。CodePipeline 部署
    * E。CodeBuild构建
    
# 285 - 图
* Q
    * 安全审查**未通过**
    * buildspec.yaml文件 （里面有key、db pwd、ssh命令）
* A
    * B。环境变量 **删除AWS凭据**
    * C。环境变量 **删除DB_PASSWORD**
    * E。不使用**scp ssh命令**
    
# 286
* Q
    * AWS Secrets Manager 存储 敏感的API密钥
    * AWS Secrets Manager + Lambda, KMS
    * 需要：只有 Lambda 才能访问 AWS Secrets Manager 值，最少授权原则
* A
    * 围绕 Lambda 访问 密钥服务 权限
    * B。创建 **KMS** 客户管理密钥，**信任** AWS Secrets Manager 并**允许Lambda 解密**
    * D。确保 Lambda **执行角色** 在 **资源级别**上有 KMS 权限。**加密** Secrets Manager 密钥
    
# 287
* Q
    * SNS -> Lambda -> RDS Multi-AZ 
    * 不小心**关闭 DB**，期间**SNS消息丢失**
* A
    * C。SNS配置SQS**死信队列**
    * D。**订阅** SNS -> SQS，SQS -> Lambda
    
# 288
* Q
    * 自动缩放组 EC2，都没有响应，HTTP健康检查不通过
    * EC2实例 没有进程运行，日志 内存不足
    * **应对 内存泄漏**
* A
    * A。改 自动缩放 配置，**不健康 替换**
    * E。 CloudWatch**代理**，收集EC2内存利用率。报警
    
# 289
* Q
    * 工作负载OU下: 开发、生产OU
    * 授予完全访问权限
    * 只允许 **特定** 管理IAM角色 仅在生产OU中，管理IAM角色和策略
* A
    * 在 根 应用 **FullAWSAccess** SCP
    * 创建 条件 SCP，拒绝 IAM 操作。排除  管理IAM 角色。 附加 **生产OU**
    
# 290 - 不懂
* Q
    * 集中化 CloudFormation 和 AWS Service Catalog
    * 通过AWS Control Tower提供一系列定制
* A
    * B。创建 IAM角色 AWSControlTower**Blueprint**Access
    * C。为每个 CloudFormation 创建 **服务目录产品**(Service Catalog)
    * F。创建 包含**每个定制** CloudFormation **模版**
    
# 291
* Q
    * 故事：AMI不小心被删除了，出问题了，怎么恢复
    * 自动伸缩组 EC2 自定义AMI
    * 自动伸缩组 **横向扩展 失败**
    * AMI最近被删除
    * AMI **ID不存在**
* A
    * 围绕 **新AMI** 创建和启动
    * A。创建新 **AMI 启动模版**
    * B。更新 自动伸缩组 **使用新模版**
    * E。自动伸缩组 **创建ec2**实例 from **新AMI**
    
# 292
* Q
    * 之前部署：CodeArtifact， Lambda触发CodeBuild，AWS Manager Run Command部署到 EC2
    * 以前部署**导致缺陷**。EC2没有最新代码，实例间**不一致**
* A
    * B。CodePipeline， 创建**单独****管道阶段**
    * C。创建 Codedeploy应用程序 **一个****部署组**
    
# 293
* Q
    * 创建许多S3， 目的： 应用程序存储、CloudTrail日志存储
    * 优化Macie成本
* A
    * A。排除 **CloudTrail 日志**
    * D。**最后修改** 发现作业
    
# 294
* Q
    * *收购*一家公司，*整合2家的账户* 管理，**保留对账户完全管理权**
    * 需要 收集和组织账户 发现 **维护 安全**
* A
    * B。被收购 账户加入 组织。创建 **OrganizationAccountAccessRole** IAM角色
    * C。AWS **Security Hub**
    
# 295
* Q
    * API Gateway 相互 TLS 认证层
* A
    * A。**AWS证书管理器**，签发 客户端证书
    * E。**根**私有证书 上传 （不是客户端证书）
    
# 296
* Q
    * Ruby MySQL。数据库应不受应用堆栈影响，保持持久
    * 需要：自动化部署，自动回滚，提醒
* A
    * Beanstalk 应用。**RDS单独**
    * Beanstalk **配置电子邮件**
    * **不**可变部署方法
* K
    * 不可变部署执行不可变更新，在单独的自动扩展组中启动一整套运行新版本应用程序的新实例，同时启动运行旧版本的实例。不可变部署可以**防止部分完成**的滚动部署**造成的问题**。如果新实例未通过健康检查，Elastic Beanstalk 就会终止它们，而原有实例则保持不变
    
# 297
* Q
    * Codepipeline。*部署生产前签名*。批准必须*记录*
* A
    * AWS **CloudTrail**，S3
        * 批准是个用户行为
    * 创建 **Codepipeline 手动批准**
        * 手动批准步骤是Codepipeline原生支持的
    
# 298
* Q
    * 限制 必须位于美国境内
    * 超出范围，发报警
* A
    * A。SCP，**拒绝** 所有非全球服务 **非美国 region**，附加 组织根
    * B。所有 region 配 **CloudTrail**
    
# 299 - 图
* Q
    * CodeCommit，使用AWS IAM Identity Center (AWS Single Sign-On) 外部 IdP
    * 允许使用Git
    * 只允许 程序团队 修改他们的 存储库 主分支
* A
    * A。**SAML断言** 团队名称 标签
    * D。每个 **CodeCommit** **添加** 访问团队**标签**
    * E。附加**SCP** - 图 （其中比较ResourceTag）
    
# 300 - 图 - 不会
* Q
    * 策略 被标记 **过于宽松**
    * **周末** env NonProduction EC2停止
* A
    * B。**Resource** * 改为 *arn:aws:ec2...*
    * E。**Action** 改 *ec2:StopInstances*
    * F。图。aws:DateTime:Friday （时间）
    
# 301
* Q
    * 每1/10秒，包含*5个不同指标*。大量数据
    * 最快查询**性能**
* A
    * A。**批处理**写入
    * D。**多指标**记录
    * F。内存 **短于** 磁盘 （暗指 支持内存缓存）
    
# 302
* Q
    * CloudFormation EC2。通过包含用户数据的**shell脚本，部署EC2**
    * EC2实例配置文件：AmazonSSMManagedInstanceCore 托管策略
    * 问题 应用程序在EC2上**未更新**
* A
    * B。`cfn-init` 辅助脚本
    * E。Systems Manager State Manager在SSM文档 和 EC2，**创建关联**
    
# 303
* Q
    * 希望 **用现有身份提供者 IdP**。仅支持 OpenID Connect （OIDC）
    * **auth.company.com**
* A
    * B。提供者 **URL**
    * D。IAM角色，**auth.company.com**
    * E。AssumeRoleWith**Web**Identity API 检索临时凭据
    
# 304
* Q
    * 本地数据中心 迁移 AWS
    * 自定义本地CI/CD
    * **CodeArtifact** *软件包和依赖*
    * 最小运营开销
* A
    * AWS Identity and Access Management **Roles Anywhere** 信任锚点，**IAM 角色**
    * **每个**公共存储库，配置**外部连接**的 CodeArtifact
    
# 305
* Q
    * Lambda满足高峰期请求量
    * 但，**最初爆发**，请求延迟。大量时间 建立数据库连接
* A
    * B。Lambda 高 **预配置并发**
    * D。重构Lambda，移动**数据库连接代码**
    * E。RDS **Proxy**
    
# [306]
* Q
    * EKS 工作负载报警 SNS
    * Amazon Managed Service for Prometheus for monitoring （监控）
* A
    * B。创建 **每个容器** *报警规则*
    * C。SNS 创建 **警报管理器配置** （alert manager）
    * E。**IAM 角色**。Amazon Managed Service for Prometheus
    
# 307
* Q
    * *AWS Systems Manager Automation* 跨EC2
    * 打补丁，**磁盘空间不足**
* A
    * EC2 安装 CloudWatch **代理**
    * 磁盘空间**警报** 作为安全控制 **添加到 AWS Systems Manager Automation**
    
# 308
* Q
    * EventBridge -> Lambda -> MySQL 读取
    * 每秒 事件数量增加，数据库 延迟 吞吐量降低
* A
    * RDS **Proxy** 创建**代理** （proxy）
    * Lambda事件处理代码 之外 **数据库连接** **打开**
    * Lambda**连接**到**代理** （proxy）
    
# 309
* Q
    * CloudWatch Logs -> Kinesis -> 单一Lambda -> S3
    * 处理和摄取日志，高延迟
* A
    * **增强型扇出**，Lambda设置为消费者
    * Lambda **ParallelizationFactor设置**
    * 增加Kinesis **分片**
    
# 310
* Q
    * 现：2 region
    * 主region S3
    * 要求：2个region都 高可用。对象存在2个S3中，故障转移
    * **最具操作效率**
* A
    * 新建 IAM 角色，允许 **S3和S3 批量操作**
    * S3 **CRR** （不要双向，双向增加复杂度了）
    * S3 **批量操作**中创建一个操作 复制S3
    
# 311
* Q
    * 自动化 **隔离** 标记为 受损的EC2
* A
    * 围绕隔离**网络流量**。SCP的问题，只能阻止创建
    * A。部署 **CloudFormation** StackSets 堆栈
    * E。配置 安全组 没有**出入站规则**。Lambda替换安全组（**不用管ACL**）
    
# 312
* Q
    * 几百个EC2实例，**自动伸缩组**
    * S3 拉取-> EC2 放回-> S3
    * 必须最少权限，临时安全凭证
* A
    * 创建**IAM角色** *S3权限*
    * 更新**启动模版** 包括*IAM实例配置文件*
    
# 313
* Q
    * **根用户登录** 收警报
    * 仪表板 显示根用户 日志
* A
    * C。 CloudWatch Logs **指标**过滤器
    * E。 **CloudTrail** -> CloudWatch Logs
    * F。CloudWatch Logs **Insights**查询的 **CloudWatch仪表板**（不用 QuickSight)
    
# 314
* Q
    * 必须 通过 IdP 进行身份验证
* A
    * IAM Identity Center， **SAML 2.0**
    * IAM **权限边界**，拒绝密码登录
    
# 315
* Q
    * 故事：手动部署Docker，每天超500用户，困难
    * Docker镜像 手动部署 难以管理
    * Docker 扫描 漏洞 HIGH 或 CRITICAL，通知
* A
    * 2个方向：管线 + 扫描
    * CodeCommit，Codepipeline。 **EventBridge** 调用流水线
    * ECR **增强型**扫描。**严重程度计数** 超过0

# 316 - 图 - 答案错
* Q
    * API Gateway，没有现有身份验证机制，只有特定OU有权限
    * 应该策略：拒绝 不通过 VPC端点的调用
    * 出现错误："User: anonymous is not authorized."
* A
    * A。**AWS IAM** 启用API方法的IAM身份验证
    * E。AWS 凭据 签名版本4 (Signature Version 4)
    
# 317
* Q
    * JavaScript 应用 解耦。发布、消费、路由事件
    * 测试。**事件没有传递**到Eventbridge 指定的目标
    * 不重新部署，**查看、排除**故障，防止丢事件
* A
    * ~~B。CloudTrail -> S3~~
    * C。Eventbridge -> **SQS 死信**
        * 未成功投递的事件将被发送到DLQ，便于后续排查和处理
    * **E。CloudWatch** Logs
        * 投递情况记录到CloudWatch Logs中，便于实时监控和排查问题
    * F。AWS **X-Ray**
        * 实现对事件处理流程的端到端追踪
    
# 318
* Q
    * 部署容器化 工作负载
    * 共享工作负载 共享服务账号
    * 部署之前必须安全扫描
    * 预扫描 后扫描 隔离。部署 不用 预扫描
* A
    * A。**共享**服务账号中。ECR 扫描。为 后扫描 建库。为预扫描授权。**预扫描库 写权限 + 后扫描库 读权限**。
    * D。为扫描库，建**CodePipeline**流水线。在预扫描库 CodeBuild，将 **无漏洞 推送到 后扫描**库  buildspec.xml
    
# 319
* Q
    * PII S3 KMS CloudFormation
    * 每周 生产 -> 开发 环境 S3更新，匿名化 PII
* A
    * A。**生产账户** 存储桶上**激活** Amazon **Macie**。 Step Function （**不要先**设置S3复制）
    * D。**Eventbridge**， 每周 Step Function
        * 上下两组选项，各选一个
    
# 320
* Q
    * API 用于检索 工作负载指标
    * 需要 审计、分析、可视化，大规模检测
* A
    * 指标存 **S3**
    * **Glue** S3 编目
    * Athena + **QuickSight**
    
# 321
* Q
    * EKS EC2 EFS
    * 应用启动时，EC2**没有挂载**EFS
* A
    * B。**入站规则**，允许EKS集群的NFS流量
    * C。**IAM角色**
    * E。EKS节点 子网中 创建 EFS **挂载目标**
    
# 322
* Q
    * 本地数据中心 + AWS
    * Direct Connect， *从本地设备 到 EFS* 所有**流量加密**
    * **撤销**单个设备不影响
    * 最少权限
* A
    * 引用 AWS **Private CA**， 创建 IAM 角色
    * **amazon-efs-utils**包挂载EFS
    
# 323
* Q
    * 容器镜像安全
    * 集成 操作系统 编程语言包  漏洞扫描
    * ECR 流水线
    * 需要：**向流水线 添加镜像扫描**。仅部署 没有CRITICAL HIGH 
* A
    * ECR **增强**扫描
    * EventBridge -> Lambda -> **Amazon Inspector**
    
# 324
* Q
    * 故障转移 灾难恢复
    * MySQL EC2
    * RPO 2小时，RTO 10分钟
* A
    * B。Aurora **全局**
    * D。Route53 **故障转移**
    
# 325
* Q
    * AWS SAM, Lambda
    * S3 CodeUri
    * package.zip Lambda 代码
    * sam deploy 命令失败。返回 **没有更改**可部署错误
* A
    * *更新 *CodeUri属性。**sam deploy**命令
    * *更新 *CodeUri属性。 aws cloudformation **package** 和 aws cloudformation **deploy** 命令
    
# 326 - 图
* Q
    * 必须 用 CloudFormation 进行环境所有更改
    * **开发人员 IAM 角色** 有 AdministratorAccess
    * 创建**新**的 IAM 角色 **CloudFormationDeployment**
    * *只有 CloudFormation 使用新角色。**开发团队**不能手动更改*
* A
    * A。**移除** AdministratorAccess。 **使用** CloudFormation**Deployment**角色**作为服务角色** （**不是 假定** 角色）
    * D。更新CloudFormation**Deployment** **信任策略**。执行 `iam:AssumeRole`
    * F。CloudFormation**Deployment**角色 添加一个IAM策略，允许 `cloudformation:*` 上的操作。`iam:PassRole`
    * D和F，也是**2个有 cloudformation.amazonaws.com的选项** (这个就是CloudFormation服务本事的代表啊！！)
    
# 327
* Q
    * 基于CodeArtifact策略
    * 每个*应用团队*有 账户，有限权限访问 *共享服务账户*
    * 公共包  必须与 整个组织共享
    * 最少管理开销
* A
    * 故事：这里的目标，就在讨论怎么设定 **共享库** 部分，不关其他每个账户的事情
    * B。在**共享服务账号**中创建 **一个域**
    * D。在**共享服务账号**中创建 **一个存储库**
    * E。需要共享包的团队，创建 **基于资源策略**
    
# 328
* Q
    * CodeArtifact存储库 和 公共上游存储库
    * 内网 存储库 **开源依赖项**
    * 软件包最新版本 严重漏洞
    * 防止 易受攻击 版本被下载
* A
    * 软件包版本 **已删除**
    * 源代码 控制设置，允许直接发布 并 **阻止上游操作**
    
# 329
* Q
    * EC2 + ALB + 自动伸缩组 分析数据
    * *关键数据*，**不容忍中断**；*非关键数据*，**可以中断**，涉及内存消耗
* A
    * 关键数据。**warm pool**，**按需实例**
    * 非关键数据。创建 使用 **启动模版** 第二个伸缩组。CloudWatch**代理**，内存使用率指标。**Spot实例**
        * 不要那个跟踪策略的选项
    
# 330
* Q
    * EKS + EC2，根据CPU率 扩展
    * 高负载时，内存错误，**无法扩展**
    * 随时间**收集、分析** 程序**内存指标**
* A
    * **托管策略**， **IAM实例配置文件**
    * CloudWatch**代理**部署，收集性能指标 （**不要选** Distro for OpenTelemetry）
    * **Service维度**分析 **pod_memory_utilization**
* R
    * 2 2分组的选
    
# 331
* Q
    * IAM 团队 ， 希望实施 AWS IAM Identity Center (AWS Single Sign-On)
    * 为现有和新成员账户配置
    * （此题，要理解成 账户 各管个的IAM，才会引导到 新账户中 操作）
* A
    * A。创建 新的AWS账户，在**新账户中**，启用 AWS IAM Identity Center
        * 没说2个账户间如何复权，所以只能选同一个账户中开启，也就是都在新账户中
    * D。AWSSSO**Member**AccountAdministrator 附加到组
    * F。权限集分配给 **新的AWS账户**
    
# 332
* Q
    * 主账户中 AWS Backup -> **新账户中** AWS Backup **备份库**
    * 但，并**未**复制到新账户
* A
    * A。编辑 **新账户** *备份库策略* 以允许访问主账户
    * D。编辑**主账户**中的 KMS *密钥策略*
    
# 333
* Q
    * S3存储图像
    * 多区域策略，故障转移
    * 15分钟内 复制
* A
    * A。启用 复制时间控制 **S3 RTC**
    * B。创建 *多区域访问点*。 **主动-被动模式**
    * C。AWS API SubmitMultiRegion**AccessPointRoutes**，当需要故障转移时
    
# 334
* Q
    * CodePipeline CodeBuild 部署 CDK应用
    * 希望加入单元测试。如 没有失败，部署继续
* A
    * A。CodeBuild运行测试。**OnFailure ABORT**
    * D。CDK**断言模块**创建测试。template.hasResourceProperties
    
# 335
* Q
    * ECS ECR CI/CD实现**集成测试**
    * 测试要求：确保最小版本可访问，**各种API返回成功响应数据**
* A
    * 添加部署阶段，**ECS**为动作提供者
    * 更新构建管道阶段，输出 `imagedefinitions.json`， 引用 新 镜像标签
    * **Lambda** 对 *服务连通性* 检查 和 API 调用

# 336
* Q
    * 多可用区 Windows、 Linux EC2实例
    * SMB、NFS。亚毫秒延迟
* A
    * B。FSx for NetApp ONTAP
    * D。更新**启动模版**，挂载文件系统
    * E。每个 *Auto Scaling组* **实例刷新**
    
# 337
* Q
    * Java应用 ECS + Fargate
    * **JVM 线程数**决定扩展 指标
    * **9404**提供JVM指标
    * 操作开销最小
* A
    * A。CloudWatch 代理。**从端口9404检索**JVM指标。逐步扩展
    * D。Prometheus。**从端口9404检索**JVM指标。目标跟踪策略 （不会主动发布）
    
# 338 - 类似331，但答案有出入。应该是338错了
* Q
    * IAM 团队 实施 AWS IAM Identity Center
    * 不能获得 不必要访问权限
    * 能为新和现有账户 提供 AWS IAM Identity Center 权限集
* A
    * A。创建新账户，在新账户中，启用 AWS IAM Identity Center
    * D。在 AWS IAM Identity Center 中，创建IAM 用户和组， AWSSSOMemberAccountAdministrator
    * F。权限集 分配给 新账户
    
# 339 - 可能根本没正确答案。pdf选项都对不上
* Q
    * **EKS** + Fargate
    * 正在经历稳定性问题，响应时间变长
    * 需要： CloudWatch配置可观测性
* A
    * B。AWS Distro for **OpenTelemetry** Collector  (排除A 不知道 StatefulSet是个什么玩意)
    * C。Kubernetes服务账户 与 IAM角色**关联**
    * E。EKS 启用 **控制平面日志**
* K
    * AWS Distro for OpenTelemetry Collector
        * The ADOT Collector is based on OpenTelemetry, which is an **open-source** set of APIs, libraries, agents, and instrumentation to enable observability across applications and systems.

# 340 - 不懂
* Q
    * AWS Control Tower
    * 现有 AWS **Step Function工作流**，用于创建新账户
    * 希望： 新账户自动注册到 **AWS Control Tower**， 添加到工作流中
* A
    * D。AWS Service **Catalog ProvisionProduct** API
        * 这可以帮助自动将新账户注册到AWS Control Tower
    * E。调用 Organizations **EnableAWSServiceAccess** API
        * 是自动注册AWS Control Tower所必需的
    
# 341
* Q
    * EKS EC2。pod自动水平扩展
    * 需要： 收集EKS 指标和日志，建立性能基线。发邮件通知
* A
    * **Fluent Bit**
    * 创建 CloudWatch警报（**不是 复合警报**），监控集群 CPU 内存 节点故障
    * 创建 CloudWatch警报 自动扩展器 部署错误 **度量日志**过滤器
* K
    * Fluent Bit
        * is an open-source, lightweight, and high-performance **log processor and forwarder**. It's often used in logging pipelines to collect, process, and forward logs to various destinations, such as Elasticsearch, Kafka, or other storage systems. Fluent Bit is part of the Fluentd ecosystem but is designed to be more lightweight and optimized for environments with limited resources, such as containers or edge devices.

# 342 - 答案错
* Q
    * **eu region** CodePipeline 部署 Lambda
    * 需要 **部署到 us region**
* A
    * ~~A。**修改 CloudFormation 模版**，*参数* 位置~~
    * C。us-east-1 **创建S3**，允许**CodePipeline**访问
    * E。**修改管道**，us S3作为存储

# 343
* Q
    * **Eventbridge 匹配特定事件**，调用 工作流
    * 发现： 一些重要事件 **没有按预期调用 工作流**
    * 确定根本原因
* A
    * B。为 **Eventbridge 和 Step Function 指标** 配置 CloudWatch监控
    * D。Step Function **执行历史**
    * E。Eventbridge**失败调用指标**。有足够权限
    
# 344
* Q
    * VPC EC2， RDP访问
    * 希望：收集每天启动多少会话 指标
* A
    * **核心：流日志**
    * C。**VPC Flow Logs** 流日志
    * D。CloudWatch Logs，**流日志 -> 日志组**
    * E。日志组**度量**过滤器
    
# 345 - 部分不懂
* Q
    * ECS EC2 ECR。实例可连接公网
    * 新要求：不能直接访问公网，移除 VPC中到NAT和互联网网关。仍可以更新ECR
* A
    * C。ECR 拉取通过 **缓存规则**。确保移除公网前，**至少一次**
    * E。**私有**ECR interface VPC endpoint
    * F。**S3 gateway** endpoint
    
# 346
* Q
    * ECR 检查和修复 漏洞 太多时间
    * 希望：快速 且 通知
* A
    * A。**激活**ECR到 Amazon **Inspector增强扫描**
    * B。为 （**Amazon Inspector **findings）创建一个**Eventbridge**规则，目标SNS
    
# 347 - 答案错误
* Q
    * Redshift
    * 希望： 仪表板 查看 用户执行的查询
* A
    * C。配置 Redshift **审计日志记录**， **CloudWatch**
    * D。**日志小部件** **CloudWatch**仪表板
    
# 348
* Q
    * Codepipeline
    * S3 单页web应用
    * 需要 **使用CloudFormation模版中 S3**
* A
    * B。CloudFormation 添加 BucketName，命名空间，设为**管道的StackVariables**
    * D。构建操作 **CodePipeline**， 使用 StackVariables.BucketName 变量
    
# [349]
* Q
    * AWS Control Tower 管理 AWS Config配置记录器 在公司每个账户中
    * 公司需要讲定制应用与任何新账户。理解成是Lambda在每个新账户中调用
* A
    * C。AWS Control Tower， **Eventbridg**， 在注册或重新注册时，调用Lambda，**重新注册根OU**
    * D。*AWSControlTowerExecution* **IAM角色**
    * E。**假定***AWSControlTowerExecution* IAM角色的权限
    
# 350
* Q
    * S3 写入**大量对象**，**同时执行**几个任务
    * AWS Step Function
* A
    * A。**异步** Express 工作流
    * D。Eventbridge规则 **启动工作流** （**不用**Lambda）
        * Use an EventBridge rule to directly trigger the Step Functions workflow (Option D).
    
# 351
* Q
    * ECS ALB, CodeDeploy 蓝/绿 全量 部署
    * 新版本 请求 响应时间 显著增加，回滚
    * 需要：**所有**流量转移前，监控。如果增加**响应时间**，立即回滚
* A
    * A。CodeDeployDefault.ECS**Canary**10Percent5Minutes
    * D。监控 ALB **TargetResponseTime**
    
# [352]
* Q
    * 大量EC2。大量请求返回错误。健康检查为健康
    * 收集应用日志。如果**超过一半错误**，收警报
    * 最小操作开销
* A
    * A。CloudWatch **Contributor Insights**。日志分组
        * 肯定比配 CloudFormation省事
    * D。**INSIGHT_RULE_METRIC**。超过一半 SNS
* K
    * CloudWatch Contributor Insights
        * is a feature of Amazon CloudWatch that helps you analyze and gain **deeper insights** into your log data. It automatically analyzes log data from services like AWS CloudTrail, VPC Flow Logs, and custom logs. Contributor Insights processes and categorizes this log data to identify key contributors to patterns or trends
    
# 353
* Q
    * Org，开发人员 能 创建 IAM用户，不能授权过多
    * **开发人员** 每账户有 **CreateAndManageUsers** 角色
    * 必须**防止其他用户** 创建IAM 用户
* A
    * A。**SCP** **否认** 创建IAM能力. 附加 CreateAndManageUsers 给 开发人员
    * C。每账户 **PermissionBoundaries** 策略，**指定开发人员** *可以*授予的**最大权限** (短)