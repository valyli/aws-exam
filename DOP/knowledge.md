# OpsWorks
## **AWS OpsWorks vs. Other AWS Tools**

|     **Feature**     |          **OpsWorks**           |         **CloudFormation**         |      **Elastic Beanstalk**      |
| ------------------- | ------------------------------- | ---------------------------------- | ------------------------------- |
| **Focus**           | Configuration management        | Infrastructure as code             | Application deployment          |
| **Management**      | Chef/Puppet for configuration   | Templates to define infrastructure | Managed deployment environments |
| **Target Audience** | Users familiar with Chef/Puppet | DevOps/Infrastructure engineers    | Developers needing a PaaS       |

* OpsWorks Chef Automate
* OpsWorks Puppet Enterprise

## **与 CloudFormation 和 OpsWorks 的关系**

|      **工具**       |                **主要用途**                 |                               **与 CodePipeline 的关系**                                |
| ------------------ | ------------------------------------------ | -------------------------------------------------------------------------------------- |
| **CloudFormation** | 管理基础设施即代码（IaC），创建和更新 AWS 资源。 | CodePipeline 可调用 CloudFormation 模板，自动部署基础设施。                                 |
| **OpsWorks**       | 自动化服务器配置和多层应用栈的部署。             | CodePipeline 可集成 OpsWorks，将应用部署到配置好的服务器。                                   |
| **CodePipeline**   | 自动化应用交付过程，包括构建、测试和部署。        | CodePipeline 是管理交付流程的工具，与 CloudFormation 和 OpsWorks 搭配使用时实现更完整的自动化。 |

# CodeDeploy
* 用于自动化代码部署到各种计算服务上
* 支持 蓝绿部署 滚动更新 分阶段更新 自动回滚
* AppSpec 文件。包括：要部署的文件、生命周期事件钩子

## **EC2/On-Premises 部署生命周期钩子**

下表列出了主要的生命周期事件和其执行的顺序：

|      生命周期事件      |                     描述                     |
| -------------------- | -------------------------------------------- |
| `BeforeBlockTraffic` | 停止流量前的自定义任务，例如通知或日志记录。        |
| `BlockTraffic`       | 停止流量。                                    |
| `AfterBlockTraffic`  | 停止流量后的自定义任务，例如验证服务已停止接收请求。 |
| `ApplicationStop`    | 停止当前运行的应用程序实例。                     |
| `DownloadBundle`     | 下载新的应用程序包。                            |
| `BeforeInstall`      | 安装新的应用程序包前执行的自定义任务。             |
| `Install`            | 安装应用程序包。                               |
| `AfterInstall`       | 安装后的任务，例如修改配置文件或初始化数据库。      |
| `ApplicationStart`   | 启动新的应用程序实例。                          |
| `ValidateService`    | 验证部署是否成功，例如运行健康检查或集成测试。      |
| `BeforeAllowTraffic` | 开放流量前的任务，例如运行额外检查或通知。          |
| `AllowTraffic`       | 恢复流量。                                    |
| `AfterAllowTraffic`  | 恢复流量后的任务，例如执行最后的日志记录或通知。     |


# AWS Health
* 提供云服务运行状态和事件通知的平台，它帮助用户了解 AWS 服务的运行状况以及如何应对可能影响其资源的事件

# AWS Systems Manager State Manager
* is a feature of AWS Systems Manager that automates the process of **keeping** your managed instances in a *consistent and desired state.* It enables you to define and enforce configurations, software updates, or any desired **settings** on your **EC2** instances or on-premises servers without manual intervention.
    * Patching and Updates
    * Configuration Management
    * Software Installation
    * Compliance Reportin
    
# Amazon Detective
* helps you quickly *analyze and investigate security events* across one or more AWS accounts by generating data visualizations that represent the ways your resources behave and interact over time. Detective creates visualizations of GuardDuty findings.

# AWS CDK (Cloud Development Kit) 
* is an open-source software development framework used to *define cloud infrastructure* in **code** and provision AWS resources using familiar programming languages. It allows developers to define cloud resources using high-level constructs (such as EC2 instances, S3 buckets, Lambda functions, etc.) in languages like TypeScript, Python, Java, and C#.

# AWS Systems Manager Automation
* allows you to automate common IT tasks and complex workflows across AWS resources. 
## Runbooks
*   **Predefined Runbooks**: AWS provides a set of predefined runbooks for common tasks, such as patching EC2 instances, creating backups, and restarting instances.
*   **Custom Runbooks**: You can create your own runbooks using either the AWS Management Console, the Systems Manager Automation API, or AWS CloudFormation templates.

## Automation Documents (Runbooks)
* Automation documents are the core of AWS Systems Manager Automation. These documents **define** the **steps** in an *automation process*.

# 通过 IAM 角色进行跨账户访问
**步骤：**

*   **在目标账户（被访问账户）中创建 IAM 角色：**【具有被访问资源的一方】
    *   选择“信任关系”中的“另一个 AWS 账户”作为信任实体。
    *   在信任策略中指定源账户的 AWS 账户 ID。
    *   分配必要的权限策略（例如，S3 读取或写入权限）。
*   **在源账户（访问账户）中创建一个 IAM 用户或角色：** 【想访问资源的一方】
    *   授予该用户或角色权限，通过 `sts:assumeRole` 操作来扮演目标账户中的角色。
    
# CloudWatch Contributor Insights
* is a feature of Amazon CloudWatch that helps you analyze and gain **deeper insights** into your log data. It automatically analyzes log data from services like AWS CloudTrail, VPC Flow Logs, and custom logs. Contributor Insights processes and categorizes this log data to identify key contributors to patterns or trends

# Amazon Guru Security

# SCP
* SCP不能附加到IAM用户或IAM角色
* 可以附加到OU和单个账户上

# CloudWatch
|      特性       | **指标过滤器（Metric Filter）** | **订阅过滤器（Subscription Filter）**  |
| -------------- | ----------------------------- | ----------------------------------- |
| **主要用途**     | 从日志生成 CloudWatch 指标      | 实时处理并传输日志数据                  |
| **工作方式**     | 被动监听                       | 主动推送                              |
| **目标**        | CloudWatch 指标                | Kinesis、Lambda、Elasticsearch 等服务 |
| **日志处理方式** | 统计匹配次数，不传输原始日志       | 传输匹配的日志条目                      |
| **实时性**      | 非实时                         | 实时                                 |
| **典型场景**     | 用于构建监控和警报               | 用于实时日志分析或转存                  |

* To SNS
    * Subscription Filter, NO
    * Metric Filter, NO
    
|       功能        |        **指标过滤器 + SNS**         | **订阅过滤器 + SNS（通过 Lambda）** |
| ---------------- | ---------------------------------- | -------------------------------- |
| **实时性**        | 延迟较高（1 分钟或更长的评估周期）      | 实时推送日志内容                    |
| **日志内容可用性** | 只提供数字指标（无法直接包含日志内容）   | 可直接传递日志内容                  |
| **复杂性**        | 较简单，只需配置过滤器、警报和 SNS 主题 | 稍复杂，需要配置 Lambda 来桥接       |
| **典型场景**      | 监控关键事件的频率，触发简短的报警通知   | 实时处理或转发日志内容到外部系统       |


# AWS CloudTrail 管理事件 和 数据事件的区别
|     特性      |  管理事件（Management Events）   |           数据事件（Data Events）            |
| ------------- | ------------------------------ | ------------------------------------------ |
| **功能**      | 记录管理操作（创建、修改、删除资源） | 记录数据访问操作（读取、修改资源数据）           |
| **默认启用**   | 是                              | 否                                         |
| **记录粒度**   | 粗粒度（控制平面操作）             | 细粒度（数据平面操作）                        |
| **示例操作**   | `RunInstances`、`CreateBucket`  | `GetObject`、`PutObject`、`InvokeFunction` |
| **配置复杂性** | 无需额外配置，默认启用             | 需要手动启用并选择具体资源                     |
| **收费**      | 免费记录（存储和传输费用另算）      | 记录事件会产生额外费用                        |
| **使用场景**   | 审计资源管理操作、跟踪用户行为      | 审计数据访问、监控敏感资源的使用情况             |
