## 《分布式任务调度平台XXL-JOB》

[![Actions Status](https://github.com/xuxueli/xxl-job/workflows/Java%20CI/badge.svg)](https://github.com/xuxueli/xxl-job/actions)
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.xuxueli/xxl-job/badge.svg)](https://maven-badges.herokuapp.com/maven-central/com.xuxueli/xxl-job/)
[![GitHub release](https://img.shields.io/github/release/xuxueli/xxl-job.svg)](https://github.com/xuxueli/xxl-job/releases)
[![GitHub stars](https://img.shields.io/github/stars/xuxueli/xxl-job)](https://github.com/xuxueli/xxl-job/)
[![Docker Status](https://img.shields.io/docker/pulls/xuxueli/xxl-job-admin)](https://hub.docker.com/r/xuxueli/xxl-job-admin/)
[![License](https://img.shields.io/badge/license-GPLv3-blue.svg)](http://www.gnu.org/licenses/gpl-3.0.html)
[![donate](https://img.shields.io/badge/%24-donate-ff69b4.svg?style=flat)](https://www.xuxueli.com/page/donate.html)

[TOCM]

[TOC]

## 一、简介

### 1.1 概述
XXL-JOB是一个分布式任务调度平台，其核心设计目标是开发迅速、学习简单、轻量级、易扩展。现已开放源代码并接入多家公司线上产品线，开箱即用。
### 1.2 社区交流    
### 1.3 特性
- 1、简单：支持通过Web页面对任务进行CRUD操作，操作简单，一分钟上手；
- 2、动态：支持动态修改任务状态、启动/停止任务，以及终止运行中任务，即时生效；
- 3、调度中心HA（中心式）：调度采用中心式设计，“调度中心”自研调度组件并支持集群部署，可保证调度中心HA；
- 4、执行器HA（分布式）：任务分布式执行，任务"执行器"支持集群部署，可保证任务执行HA；
- 5、注册中心: 执行器会周期性自动注册任务, 调度中心将会自动发现注册的任务并触发执行。同时，也支持手动录入执行器地址；
- 6、弹性扩容缩容：一旦有新执行器机器上线或者下线，下次调度时将会重新分配任务；
- 7、路由策略：执行器集群部署时提供丰富的路由策略，包括：第一个、最后一个、轮询、随机、一致性HASH、最不经常使用、最近最久未使用、故障转移、忙碌转移等；
- 8、故障转移：任务路由策略选择"故障转移"情况下，如果执行器集群中某一台机器故障，将会自动Failover切换到一台正常的执行器发送调度请求。
- 9、阻塞处理策略：调度过于密集执行器来不及处理时的处理策略，策略包括：单机串行（默认）、丢弃后续调度、覆盖之前调度；
- 10、任务超时控制：支持自定义任务超时时间，任务运行超时将会主动中断任务；
- 11、任务失败重试：支持自定义任务失败重试次数，当任务失败时将会按照预设的失败重试次数主动进行重试；其中分片任务支持分片粒度的失败重试；
- 12、任务失败告警；默认提供邮件方式失败告警，同时预留扩展接口，可方便的扩展短信、钉钉等告警方式；
- 13、分片广播任务：执行器集群部署时，任务路由策略选择"分片广播"情况下，一次任务调度将会广播触发集群中所有执行器执行一次任务，可根据分片参数开发分片任务；
- 14、动态分片：分片广播任务以执行器为维度进行分片，支持动态扩容执行器集群从而动态增加分片数量，协同进行业务处理；在进行大数据量业务操作时可显著提升任务处理能力和速度。
- 15、事件触发：除了"Cron方式"和"任务依赖方式"触发任务执行之外，支持基于事件的触发任务方式。调度中心提供触发任务单次执行的API服务，可根据业务事件灵活触发。
- 16、任务进度监控：支持实时监控任务进度；
- 17、Rolling实时日志：支持在线查看调度结果，并且支持以Rolling方式实时查看执行器输出的完整的执行日志；
- 18、GLUE：提供Web IDE，支持在线开发任务逻辑代码，动态发布，实时编译生效，省略部署上线的过程。支持30个版本的历史版本回溯。
- 19、脚本任务：支持以GLUE模式开发和运行脚本任务，包括Shell、Python、NodeJS、PHP、PowerShell等类型脚本;
- 20、命令行任务：原生提供通用命令行任务Handler（Bean任务，"CommandJobHandler"）；业务方只需要提供命令行即可；
- 21、任务依赖：支持配置子任务依赖，当父任务执行结束且执行成功后将会主动触发一次子任务的执行, 多个子任务用逗号分隔；
- 22、一致性：“调度中心”通过DB锁保证集群分布式调度的一致性, 一次任务调度只会触发一次执行；
- 23、自定义任务参数：支持在线配置调度任务入参，即时生效；
- 24、调度线程池：调度系统多线程触发调度运行，确保调度精确执行，不被堵塞；
- 25、数据加密：调度中心和执行器之间的通讯进行数据加密，提升调度信息安全性；
- 26、邮件报警：任务失败时支持邮件报警，支持配置多邮件地址群发报警邮件；
- 27、推送maven中央仓库: 将会把最新稳定版推送到maven中央仓库, 方便用户接入和使用;
- 28、运行报表：支持实时查看运行数据，如任务数量、调度次数、执行器数量等；以及调度报表，如调度日期分布图，调度成功分布图等；
- 29、全异步：任务调度流程全异步化设计实现，如异步调度、异步运行、异步回调等，有效对密集调度进行流量削峰，理论上支持任意时长任务的运行；
- 30、跨语言：调度中心与执行器提供语言无关的 RESTful API 服务，第三方任意语言可据此对接调度中心或者实现执行器。除此之外，还提供了 “多任务模式”和“httpJobHandler”等其他跨语言方案；
- 31、国际化：调度中心支持国际化设置，提供中文、英文两种可选语言，默认为中文；
- 32、容器化：提供官方docker镜像，并实时更新推送dockerhub，进一步实现产品开箱即用；
- 33、线程池隔离：调度线程池进行隔离拆分，慢任务自动降级进入"Slow"线程池，避免耗尽调度线程，提高系统稳定性；
- 34、用户管理：支持在线管理系统用户，存在管理员、普通用户两种角色；
- 35、权限控制：执行器维度进行权限控制，管理员拥有全量权限，普通用户需要分配执行器权限后才允许相关操作；

至今，XXL-JOB已接入多家公司的线上产品线，接入场景如电商业务，O2O业务和大数据作业等，截止最新统计时间为止。
### 1.5 下载
源码仓库地址 | Release Download
--- | ---
[https://github.com/xuxueli/xxl-job](https://github.com/xuxueli/xxl-job) | [Download](https://github.com/xuxueli/xxl-job/releases)  
[http://gitee.com/xuxueli0323/xxl-job](http://gitee.com/xuxueli0323/xxl-job) | [Download](http://gitee.com/xuxueli0323/xxl-job/releases)


#### 中央仓库地址

```
<!-- http://repo1.maven.org/maven2/com/xuxueli/xxl-job-core/ -->
<dependency>
    <groupId>com.xuxueli</groupId>
    <artifactId>xxl-job-core</artifactId>
    <version>${最新稳定版本}</version>
</dependency>
```


### 1.6 环境
- Maven3+
- Jdk1.8+
- Mysql5.7+


## 二、快速入门

### 2.1 初始化“调度数据库”
请下载项目源码并解压，获取 "调度数据库初始化SQL脚本" 并执行即可。

"调度数据库初始化SQL脚本" 位置为:

    /xxl-job/doc/db/tables_xxl_job.sql

调度中心支持集群部署，集群情况下各节点务必连接同一个mysql实例;

如果mysql做主从,调度中心集群节点务必强制走主库;

### 2.2 编译源码
解压源码,按照maven格式将源码导入IDE, 使用maven进行编译即可，源码结构如下：

    xxl-job-admin：调度中心
    xxl-job-core：公共依赖
    xxl-job-executor-samples：执行器Sample示例（选择合适的版本执行器，可直接使用，也可以参考其并将现有项目改造成执行器）
        ：xxl-job-executor-sample-springboot：Springboot版本，通过Springboot管理执行器，推荐这种方式；
        ：xxl-job-executor-sample-spring：Spring版本，通过Spring容器管理执行器，比较通用；
        ：xxl-job-executor-sample-frameless：无框架版本；
        ：xxl-job-executor-sample-jfinal：JFinal版本，通过JFinal管理执行器；
        ：xxl-job-executor-sample-nutz：Nutz版本，通过Nutz管理执行器；
        ：xxl-job-executor-sample-jboot：jboot版本，通过jboot管理执行器；
        

### 2.3 配置部署“调度中心”

    调度中心项目：xxl-job-admin
    作用：统一管理任务调度平台上调度任务，负责触发调度执行，并且提供任务管理平台。

#### 步骤一：调度中心配置：
调度中心配置文件地址：

    /xxl-job/xxl-job-admin/src/main/resources/application.properties


调度中心配置内容说明：

    ### 调度中心JDBC链接：链接地址请保持和 2.1章节 所创建的调度数据库的地址一致
    spring.datasource.url=jdbc:mysql://127.0.0.1:3306/xxl_job?useUnicode=true&characterEncoding=UTF-8&autoReconnect=true&serverTimezone=Asia/Shanghai
    spring.datasource.username=root
    spring.datasource.password=root_pwd
    spring.datasource.driver-class-name=com.mysql.jdbc.Driver
    
    ### 报警邮箱
    spring.mail.host=smtp.qq.com
    spring.mail.port=25
    spring.mail.username=xxx@qq.com
    spring.mail.password=xxx
    spring.mail.properties.mail.smtp.auth=true
    spring.mail.properties.mail.smtp.starttls.enable=true
    spring.mail.properties.mail.smtp.starttls.required=true
    spring.mail.properties.mail.smtp.socketFactory.class=javax.net.ssl.SSLSocketFactory
    
    ### 调度中心通讯TOKEN [选填]：非空时启用；
    xxl.job.accessToken=
    
    ### 调度中心国际化配置 [必填]： 默认为 "zh_CN"/中文简体, 可选范围为 "zh_CN"/中文简体, "zh_TC"/中文繁体 and "en"/英文；
    xxl.job.i18n=zh_CN
    
    ## 调度线程池最大线程配置【必填】
    xxl.job.triggerpool.fast.max=200
    xxl.job.triggerpool.slow.max=100
    
    ### 调度中心日志表数据保存天数 [必填]：过期日志自动清理；限制大于等于7时生效，否则, 如-1，关闭自动清理功能；
    xxl.job.logretentiondays=30
    
    

#### 步骤二：部署项目：
如果已经正确进行上述配置，可将项目编译打包部署。

调度中心访问地址：http://localhost:8080/xxl-job-admin (该地址执行器将会使用到，作为回调地址)

默认登录账号 "admin/123456", 登录后运行界面如下图所示。

![输入图片说明](https://www.xuxueli.com/doc/static/xxl-job/images/img_6yC0.png "在这里输入图片标题")

至此“调度中心”项目已经部署成功。

#### 步骤三：调度中心集群（可选）：
调度中心支持集群部署，提升调度系统容灾和可用性。

调度中心集群部署时，几点要求和建议：
- DB配置保持一致；
- 集群机器时钟保持一致（单机集群忽视）；
- 建议：推荐通过nginx为调度中心集群做负载均衡，分配域名。调度中心访问、执行器回调配置、调用API服务等操作均通过该域名进行。


#### 其他：Docker 镜像方式搭建调度中心：

- 下载镜像

```
// Docker地址：https://hub.docker.com/r/xuxueli/xxl-job-admin/     (建议指定版本号)
docker pull xuxueli/xxl-job-admin
```

- 创建容器并运行

```
docker run -p 8080:8080 -v /tmp:/data/applogs --name xxl-job-admin  -d xuxueli/xxl-job-admin:{指定版本}

/**
* 如需自定义 mysql 等配置，可通过 "-e PARAMS" 指定，参数格式 PARAMS="--key=value  --key2=value2" ；
* 配置项参考文件：/xxl-job/xxl-job-admin/src/main/resources/application.properties
* 如需自定义 JVM内存参数 等配置，可通过 "-e JAVA_OPTS" 指定，参数格式 JAVA_OPTS="-Xmx512m" ；
*/
docker run -e PARAMS="--spring.datasource.url=jdbc:mysql://127.0.0.1:3306/xxl_job?useUnicode=true&characterEncoding=UTF-8&autoReconnect=true&serverTimezone=Asia/Shanghai" -p 8080:8080 -v /tmp:/data/applogs --name xxl-job-admin  -d xuxueli/xxl-job-admin:{指定版本}
```


### 2.4 配置部署“执行器项目”

    “执行器”项目：xxl-job-executor-sample-springboot (提供多种版本执行器供选择，现以 springboot 版本为例，可直接使用，也可以参考其并将现有项目改造成执行器)
    作用：负责接收“调度中心”的调度并执行；可直接部署执行器，也可以将执行器集成到现有业务项目中。
    
#### 步骤一：maven依赖
确认pom文件中引入了 "xxl-job-core" 的maven依赖；
    
#### 步骤二：执行器配置
执行器配置，配置文件地址：

    /xxl-job/xxl-job-executor-samples/xxl-job-executor-sample-springboot/src/main/resources/application.properties

执行器配置，配置内容说明：

    ### 调度中心部署跟地址 [选填]：如调度中心集群部署存在多个地址则用逗号分隔。执行器将会使用该地址进行"执行器心跳注册"和"任务结果回调"；为空则关闭自动注册；
    xxl.job.admin.addresses=http://127.0.0.1:8080/xxl-job-admin
    
    ### 执行器通讯TOKEN [选填]：非空时启用；
    xxl.job.accessToken=
    
    ### 执行器AppName [选填]：执行器心跳注册分组依据；为空则关闭自动注册
    xxl.job.executor.appname=xxl-job-executor-sample
    ### 执行器注册 [选填]：优先使用该配置作为注册地址，为空时使用内嵌服务 ”IP:PORT“ 作为注册地址。从而更灵活的支持容器类型执行器动态IP和动态映射端口问题。
    xxl.job.executor.address=
    ### 执行器IP [选填]：默认为空表示自动获取IP，多网卡时可手动设置指定IP，该IP不会绑定Host仅作为通讯实用；地址信息用于 "执行器注册" 和 "调度中心请求并触发任务"；
    xxl.job.executor.ip=
    ### 执行器端口号 [选填]：小于等于0则自动获取；默认端口为9999，单机部署多个执行器时，注意要配置不同执行器端口；
    xxl.job.executor.port=9999
    ### 执行器运行日志文件存储磁盘路径 [选填] ：需要对该路径拥有读写权限；为空则使用默认路径；
    xxl.job.executor.logpath=/data/applogs/xxl-job/jobhandler
    ### 执行器日志文件保存天数 [选填] ： 过期日志自动清理, 限制值大于等于3时生效; 否则, 如-1, 关闭自动清理功能；
    xxl.job.executor.logretentiondays=30
    

#### 步骤三：执行器组件配置

执行器组件，配置文件地址：

    /xxl-job/xxl-job-executor-samples/xxl-job-executor-sample-springboot/src/main/java/com/xxl/job/executor/core/config/XxlJobConfig.java

执行器组件，配置内容说明：

```
@Bean
public XxlJobSpringExecutor xxlJobExecutor() {
    logger.info(">>>>>>>>>>> xxl-job config init.");
    XxlJobSpringExecutor xxlJobSpringExecutor = new XxlJobSpringExecutor();
    xxlJobSpringExecutor.setAdminAddresses(adminAddresses);
    xxlJobSpringExecutor.setAppname(appname);
    xxlJobSpringExecutor.setIp(ip);
    xxlJobSpringExecutor.setPort(port);
    xxlJobSpringExecutor.setAccessToken(accessToken);
    xxlJobSpringExecutor.setLogPath(logPath);
    xxlJobSpringExecutor.setLogRetentionDays(logRetentionDays);

    return xxlJobSpringExecutor;
}
```

#### 步骤四：部署执行器项目：
如果已经正确进行上述配置，可将执行器项目编译打部署，系统提供多种执行器Sample示例项目，选择其中一个即可，各自的部署方式如下。

    xxl-job-executor-sample-springboot：项目编译打包成springboot类型的可执行JAR包，命令启动即可；
    xxl-job-executor-sample-spring：项目编译打包成WAR包，并部署到tomcat中。
    xxl-job-executor-sample-jfinal：同上
    xxl-job-executor-sample-nutz：同上
    xxl-job-executor-sample-jboot：同上
    

至此“执行器”项目已经部署结束。

#### 步骤五：执行器集群（可选）：
执行器支持集群部署，提升调度系统可用性，同时提升任务处理能力。

执行器集群部署时，几点要求和建议：
- 执行器回调地址（xxl.job.admin.addresses）需要保持一致；执行器根据该配置进行执行器自动注册等操作。 
- 同一个执行器集群内AppName（xxl.job.executor.appname）需要保持一致；调度中心根据该配置动态发现不同集群的在线执行器列表。


### 2.5 开发第一个任务“Hello World”       
本示例以新建一个 “GLUE模式(Java)” 运行模式的任务为例。更多有关任务的详细配置，请查看“章节三：任务详解”。
（ “GLUE模式(Java)”的执行代码托管到调度中心在线维护，相比“Bean模式任务”需要在执行器项目开发部署上线，更加简便轻量）

> 前提：请确认“调度中心”和“执行器”项目已经成功部署并启动；

#### 步骤一：新建任务：
登录调度中心，点击下图所示“新建任务”按钮，新建示例任务。然后，参考下面截图中任务的参数配置，点击保存。

![输入图片说明](https://www.xuxueli.com/doc/static/xxl-job/images/img_o8HQ.png "在这里输入图片标题")

![输入图片说明](https://www.xuxueli.com/doc/static/xxl-job/images/img_ZAsz.png "在这里输入图片标题")


#### 步骤二：“GLUE模式(Java)” 任务开发：
请点击任务右侧 “GLUE” 按钮，进入 “GLUE编辑器开发界面” ，见下图。“GLUE模式(Java)” 运行模式的任务默认已经初始化了示例任务代码，即打印Hello World。
（ “GLUE模式(Java)” 运行模式的任务实际上是一段继承自IJobHandler的Java类代码，它在执行器项目中运行，可使用@Resource/@Autowire注入执行器里中的其他服务，详细介绍请查看第三章节）

![输入图片说明](https://www.xuxueli.com/doc/static/xxl-job/images/img_Fgql.png "在这里输入图片标题")

![输入图片说明](https://www.xuxueli.com/doc/static/xxl-job/images/img_dNUJ.png "在这里输入图片标题")

#### 步骤三：触发执行：
请点击任务右侧 “执行” 按钮，可手动触发一次任务执行（通常情况下，通过配置Cron表达式进行任务调度触发）。

#### 步骤四：查看日志： 
请点击任务右侧 “日志” 按钮，可前往任务日志界面查看任务日志。
在任务日志界面中，可查看该任务的历史调度记录以及每一次调度的任务调度信息、执行参数和执行信息。运行中的任务点击右侧的“执行日志”按钮，可进入日志控制台查看实时执行日志。

![输入图片说明](https://www.xuxueli.com/doc/static/xxl-job/images/img_inc8.png "在这里输入图片标题")

在日志控制台，可以Rolling方式实时查看任务在执行器一侧运行输出的日志信息，实时监控任务进度；

![输入图片说明](https://www.xuxueli.com/doc/static/xxl-job/images/img_eYrv.png "在这里输入图片标题")

## 三、任务详解

### 配置属性详细说明：

    - 执行器：任务的绑定的执行器，任务触发调度时将会自动发现注册成功的执行器, 实现任务自动发现功能; 另一方面也可以方便的进行任务分组。每个任务必须绑定一个执行器, 可在 "执行器管理" 进行设置;
    - 任务描述：任务的描述信息，便于任务管理；
    - 路由策略：当执行器集群部署时，提供丰富的路由策略，包括；
        FIRST（第一个）：固定选择第一个机器；
        LAST（最后一个）：固定选择最后一个机器；
        ROUND（轮询）：；
        RANDOM（随机）：随机选择在线的机器；
        CONSISTENT_HASH（一致性HASH）：每个任务按照Hash算法固定选择某一台机器，且所有任务均匀散列在不同机器上。
        LEAST_FREQUENTLY_USED（最不经常使用）：使用频率最低的机器优先被选举；
        LEAST_RECENTLY_USED（最近最久未使用）：最久未使用的机器优先被选举；
        FAILOVER（故障转移）：按照顺序依次进行心跳检测，第一个心跳检测成功的机器选定为目标执行器并发起调度；
        BUSYOVER（忙碌转移）：按照顺序依次进行空闲检测，第一个空闲检测成功的机器选定为目标执行器并发起调度；
        SHARDING_BROADCAST(分片广播)：广播触发对应集群中所有机器执行一次任务，同时系统自动传递分片参数；可根据分片参数开发分片任务；
        
    - Cron：触发任务执行的Cron表达式；
    - 运行模式：
        BEAN模式：任务以JobHandler方式维护在执行器端；需要结合 "JobHandler" 属性匹配执行器中任务；
        GLUE模式(Java)：任务以源码方式维护在调度中心；该模式的任务实际上是一段继承自IJobHandler的Java类代码并 "groovy" 源码方式维护，它在执行器项目中运行，可使用@Resource/@Autowire注入执行器里中的其他服务；
        GLUE模式(Shell)：任务以源码方式维护在调度中心；该模式的任务实际上是一段 "shell" 脚本；
        GLUE模式(Python)：任务以源码方式维护在调度中心；该模式的任务实际上是一段 "python" 脚本；
        GLUE模式(PHP)：任务以源码方式维护在调度中心；该模式的任务实际上是一段 "php" 脚本；
        GLUE模式(NodeJS)：任务以源码方式维护在调度中心；该模式的任务实际上是一段 "nodejs" 脚本；
        GLUE模式(PowerShell)：任务以源码方式维护在调度中心；该模式的任务实际上是一段 "PowerShell" 脚本；
    - JobHandler：运行模式为 "BEAN模式" 时生效，对应执行器中新开发的JobHandler类“@JobHandler”注解自定义的value值；
    - 阻塞处理策略：调度过于密集执行器来不及处理时的处理策略；
        单机串行（默认）：调度请求进入单机执行器后，调度请求进入FIFO队列并以串行方式运行；
        丢弃后续调度：调度请求进入单机执行器后，发现执行器存在运行的调度任务，本次请求将会被丢弃并标记为失败；
        覆盖之前调度：调度请求进入单机执行器后，发现执行器存在运行的调度任务，将会终止运行中的调度任务并清空队列，然后运行本地调度任务；
    - 子任务：每个任务都拥有一个唯一的任务ID(任务ID可以从任务列表获取)，当本任务执行结束并且执行成功时，将会触发子任务ID所对应的任务的一次主动调度。
    - 任务超时时间：支持自定义任务超时时间，任务运行超时将会主动中断任务；
    - 失败重试次数；支持自定义任务失败重试次数，当任务失败时将会按照预设的失败重试次数主动进行重试；
    - 报警邮件：任务调度失败时邮件通知的邮箱地址，支持配置多邮箱地址，配置多个邮箱地址时用逗号分隔；
    - 负责人：任务的负责人；
    - 执行参数：任务执行所需的参数；

    
### 3.1 BEAN模式（类形式）

Bean模式任务，支持基于类的开发方式，每个任务对应一个Java类。

- 优点：不限制项目环境，兼容性好。即使是无框架项目，如main方法直接启动的项目也可以提供支持，可以参考示例项目 "xxl-job-executor-sample-frameless"；
- 缺点：
    - 每个任务需要占用一个Java类，造成类的浪费；
    - 不支持自动扫描任务并注入到执行器容器，需要手动注入。

#### 步骤一：执行器项目中，开发Job类：

    1、开发一个继承自"com.xxl.job.core.handler.IJobHandler"的JobHandler类，实现其中任务方法。
    2、手动通过如下方式注入到执行器容器。
    ```
    XxlJobExecutor.registJobHandler("demoJobHandler", new DemoJobHandler());
    ```

#### 步骤二：调度中心，新建调度任务
后续步骤和 "3.2 BEAN模式（方法形式）"一致，可以前往参考。


### 3.2 BEAN模式（方法形式）

Bean模式任务，支持基于方法的开发方式，每个任务对应一个方法。

- 优点：
    - 每个任务只需要开发一个方法，并添加"@XxlJob"注解即可，更加方便、快速。
    - 支持自动扫描任务并注入到执行器容器。
- 缺点：要求Spring容器环境；

>基于方法开发的任务，底层会生成JobHandler代理，和基于类的方式一样，任务也会以JobHandler的形式存在于执行器任务容器中。

#### 步骤一：执行器项目中，开发Job方法：

    1、在Spring Bean实例中，开发Job方法，方式格式要求为 "public ReturnT<String> execute(String param)"
    2、为Job方法添加注解 "@XxlJob(value="自定义jobhandler名称", init = "JobHandler初始化方法", destroy = "JobHandler销毁方法")"，注解value值对应的是调度中心新建任务的JobHandler属性的值。
    3、执行日志：需要通过 "XxlJobLogger.log" 打印执行日志；
    
```
// 可参考Sample示例执行器中的 "com.xxl.job.executor.service.jobhandler.SampleXxlJob" ，如下：
@XxlJob("demoJobHandler")
public ReturnT<String> execute(String param) {

    XxlJobLogger.log("hello world.");
    return ReturnT.SUCCESS;
}
```

#### 步骤二：调度中心，新建调度任务
参考上文“配置属性详细说明”对新建的任务进行参数配置，运行模式选中 "BEAN模式"，JobHandler属性填写任务注解“@XxlJob”中定义的值；

![输入图片说明](https://www.xuxueli.com/doc/static/xxl-job/images/img_ZAsz.png "在这里输入图片标题")

#### 原生内置Bean模式任务
为方便用户参考与快速实用，示例执行器内原生提供多个Bean模式任务Handler，可以直接配置实用，如下：

- demoJobHandler：简单示例任务，任务内部模拟耗时任务逻辑，用户可在线体验Rolling Log等功能；
- shardingJobHandler：分片示例任务，任务内部模拟处理分片参数，可参考熟悉分片任务；
- httpJobHandler：通用HTTP任务Handler；业务方只需要提供HTTP链接等信息即可，不限制语言、平台。示例任务入参如下：
    ```
    url: http://www.xxx.com
    method: get 或 post
    data: post-data
    ```
- commandJobHandler：通用命令行任务Handler；业务方只需要提供命令行即可；如 “pwd”命令；


### 3.3 GLUE模式(Java)
任务以源码方式维护在调度中心，支持通过Web IDE在线更新，实时编译和生效，因此不需要指定JobHandler。开发流程如下：

#### 步骤一：调度中心，新建调度任务：
参考上文“配置属性详细说明”对新建的任务进行参数配置，运行模式选中 "GLUE模式(Java)"；

![输入图片说明](https://www.xuxueli.com/doc/static/xxl-job/images/img_tJOq.png "在这里输入图片标题")

#### 步骤二：开发任务代码：
选中指定任务，点击该任务右侧“GLUE”按钮，将会前往GLUE任务的Web IDE界面，在该界面支持对任务代码进行开发（也可以在IDE中开发完成后，复制粘贴到编辑中）。

版本回溯功能（支持30个版本的版本回溯）：在GLUE任务的Web IDE界面，选择右上角下拉框“版本回溯”，会列出该GLUE的更新历史，选择相应版本即可显示该版本代码，保存后GLUE代码即回退到对应的历史版本；

![输入图片说明](https://www.xuxueli.com/doc/static/xxl-job/images/img_dNUJ.png "在这里输入图片标题")

### 3.4 GLUE模式(Shell)

#### 步骤一：调度中心，新建调度任务   
参考上文“配置属性详细说明”对新建的任务进行参数配置，运行模式选中 "GLUE模式(Shell)"；

#### 步骤二：开发任务代码：
选中指定任务，点击该任务右侧“GLUE”按钮，将会前往GLUE任务的Web IDE界面，在该界面支持对任务代码进行开发（也可以在IDE中开发完成后，复制粘贴到编辑中）。

该模式的任务实际上是一段 "shell" 脚本；

![输入图片说明](https://www.xuxueli.com/doc/static/xxl-job/images/img_iUw0.png "在这里输入图片标题")

### 3.4 GLUE模式(Python)

#### 步骤一：调度中心，新建调度任务   
参考上文“配置属性详细说明”对新建的任务进行参数配置，运行模式选中 "GLUE模式(Python)"；

#### 步骤二：开发任务代码：
选中指定任务，点击该任务右侧“GLUE”按钮，将会前往GLUE任务的Web IDE界面，在该界面支持对任务代码进行开发（也可以在IDE中开发完成后，复制粘贴到编辑中）。

该模式的任务实际上是一段 "python" 脚本；

![输入图片说明](https://www.xuxueli.com/doc/static/xxl-job/images/img_BPLG.png "在这里输入图片标题")

### 3.5 GLUE模式(NodeJS)

#### 步骤一：调度中心，新建调度任务   
参考上文“配置属性详细说明”对新建的任务进行参数配置，运行模式选中 "GLUE模式(NodeJS)"；

#### 步骤二：开发任务代码：
选中指定任务，点击该任务右侧“GLUE”按钮，将会前往GLUE任务的Web IDE界面，在该界面支持对任务代码进行开发（也可以在IDE中开发完成后，复制粘贴到编辑中）。

该模式的任务实际上是一段 "nodeJS" 脚本；

### 3.6 GLUE模式(PHP)
同上

### 3.7 GLUE模式(PowerShell)
同上



## 四、操作指南

### 4.1 配置执行器
点击进入"执行器管理"界面, 如下图:
![输入图片说明](https://www.xuxueli.com/doc/static/xxl-job/images/img_Hr2T.png "在这里输入图片标题")

    1、"调度中心OnLine:"右侧显示在线的"调度中心"列表, 任务执行结束后, 将会以failover的模式进行回调调度中心通知执行结果, 避免回调的单点风险;
    2、"执行器列表" 中显示在线的执行器列表, 可通过"OnLine 机器"查看对应执行器的集群机器。

点击按钮 "+新增执行器" 弹框如下图, 可新增执行器配置:

![输入图片说明](https://www.xuxueli.com/doc/static/xxl-job/images/img_V3vF.png "在这里输入图片标题")

执行器属性说明

    AppName: 是每个执行器集群的唯一标示AppName, 执行器会周期性以AppName为对象进行自动注册。可通过该配置自动发现注册成功的执行器, 供任务调度时使用;
    名称: 执行器的名称, 因为AppName限制字母数字等组成,可读性不强, 名称为了提高执行器的可读性;
    排序: 执行器的排序, 系统中需要执行器的地方,如任务新增, 将会按照该排序读取可用的执行器列表;
    注册方式：调度中心获取执行器地址的方式；
        自动注册：执行器自动进行执行器注册，调度中心通过底层注册表可以动态发现执行器机器地址；
        手动录入：人工手动录入执行器的地址信息，多地址逗号分隔，供调度中心使用；
    机器地址："注册方式"为"手动录入"时有效，支持人工维护执行器的地址信息；

### 4.2 新建任务
进入任务管理界面，点击“新增任务”按钮，在弹出的“新增任务”界面配置任务属性后保存即可。详情页参考章节 "三、任务详解"。

### 4.3 编辑任务
进入任务管理界面，选中指定任务。点击该任务右侧“编辑”按钮，在弹出的“编辑任务”界面更新任务属性后保存即可，可以修改设置的任务属性信息：

### 4.4 编辑GLUE代码

该操作仅针对GLUE任务。

选中指定任务，点击该任务右侧“GLUE”按钮，将会前往GLUE任务的Web IDE界面，在该界面支持对任务代码进行开发。可参考章节 "3.3 GLUE模式(Java)"。

### 4.5 启动/停止任务
可对任务进行“启动”和“停止”操作。
需要注意的是，此处的启动/停止仅针对任务的后续调度触发行为，不会影响到已经触发的调度任务，如需终止已经触发的调度任务，可查看“4.9 终止运行中的任务”

![输入图片说明](https://www.xuxueli.com/doc/static/xxl-job/images/img_ZAhX.png "在这里输入图片标题")

### 4.6 手动触发一次调度
点击“执行”按钮，可手动触发一次任务调度，不影响原有调度规则。

![输入图片说明](https://www.xuxueli.com/doc/static/xxl-job/images/img_ZAhX.png "在这里输入图片标题")

### 4.7 查看调度日志
点击“日志”按钮，可以查看任务历史调度日志。在历史调入日志界面可查看每次任务调度的调度结果、执行结果等，点击“执行日志”按钮可查看执行器完整日志。

![输入图片说明](https://www.xuxueli.com/doc/static/xxl-job/images/img_ZAhX.png "在这里输入图片标题")

![输入图片说明](https://www.xuxueli.com/doc/static/xxl-job/images/img_UDSo.png "在这里输入图片标题")

    调度时间："调度中心"触发本次调度并向"执行器"发送任务执行信号的时间；
    调度结果："调度中心"触发本次调度的结果，200表示成功，500或其他表示失败；
    调度备注："调度中心"触发本次调度的日志信息；
    执行器地址：本次任务执行的机器地址
    运行模式：触发调度时任务的运行模式，运行模式可参考章节 "三、任务详解"；
    任务参数：本地任务执行的入参
    执行时间："执行器"中本次任务执行结束后回调的时间；
    执行结果："执行器"中本次任务执行的结果，200表示成功，500或其他表示失败；
    执行备注："执行器"中本次任务执行的日志信息；
    操作：
        "执行日志"按钮：点击可查看本地任务执行的详细日志信息；详见“4.8 查看执行日志”；
        "终止任务"按钮：点击可终止本地调度对应执行器上本任务的执行线程，包括未执行的阻塞任务一并被终止；

### 4.8 查看执行日志
点击执行日志右侧的 “执行日志” 按钮，可跳转至执行日志界面，可以查看业务代码中打印的完整日志，如下图；

![输入图片说明](https://www.xuxueli.com/doc/static/xxl-job/images/img_tvGI.png "在这里输入图片标题")

### 4.9 终止运行中的任务
仅针对执行中的任务。
在任务日志界面，点击右侧的“终止任务”按钮，将会向本次任务对应的执行器发送任务终止请求，将会终止掉本次任务，同时会清空掉整个任务执行队列。

![输入图片说明](https://www.xuxueli.com/doc/static/xxl-job/images/img_hIci.png "在这里输入图片标题")

任务终止时通过 "interrupt" 执行线程的方式实现, 将会触发 "InterruptedException" 异常。因此如果JobHandler内部catch到了该异常并消化掉的话, 任务终止功能将不可用。

因此, 如果遇到上述任务终止不可用的情况, 需要在JobHandler中应该针对 "InterruptedException" 异常进行特殊处理 (向上抛出) , 正确逻辑如下:
```
try{
    // do something
} catch (Exception e) {
    if (e instanceof InterruptedException) {
        throw e;
    }
    logger.warn("{}", e);
}
```

而且，在JobHandler中开启子线程时，子线程也不可catch处理"InterruptedException"，应该主动向上抛出。

任务终止时会执行对应JobHandler的"destroy()"方法，可以借助该方法处理一些资源回收的逻辑。


### 4.10 删除执行日志
在任务日志界面，选中执行器和任务之后，点击右侧的"删除"按钮将会出现"日志清理"弹框，弹框中支持选择不同类型的日志清理策略，选中后点击"确定"按钮即可进行日志清理操作；
![输入图片说明](https://www.xuxueli.com/doc/static/xxl-job/images/img_Ypik.png "在这里输入图片标题")

![输入图片说明](https://www.xuxueli.com/doc/static/xxl-job/images/img_EB65.png "在这里输入图片标题")

### 4.11 删除任务
点击删除按钮，可以删除对应任务。

![输入图片说明](https://www.xuxueli.com/doc/static/xxl-job/images/img_Z9Qr.png "在这里输入图片标题")

### 4.12 用户管理
进入 "用户管理" 界面，可查看和管理用户信息；

目前用户分为两种角色：
- 管理员：拥有全量权限，支持在线管理用户信息，为用户分配权限，权限分配粒度为执行器；
- 普通用户：仅拥有被分配权限的执行器，及相关任务的操作权限；

![输入图片说明](https://www.xuxueli.com/doc/static/xxl-job/images/img_1001.png "在这里输入图片标题")

![输入图片说明](https://www.xuxueli.com/doc/static/xxl-job/images/img_1002.png "在这里输入图片标题")


## 五、总体设计
### 5.1 源码目录介绍
    - /doc :文档资料
    - /db :“调度数据库”建表脚本
    - /xxl-job-admin :调度中心，项目源码
    - /xxl-job-core :公共Jar依赖
    - /xxl-job-executor-samples :执行器，Sample示例项目（大家可以在该项目上进行开发，也可以将现有项目改造生成执行器项目）

### 5.2 “调度数据库”配置
XXL-JOB调度模块基于自研调度组件并支持集群部署，调度数据库表说明如下：

    - xxl_job_lock：任务调度锁表；
    - xxl_job_group：执行器信息表，维护任务执行器信息；
    - xxl_job_info：调度扩展信息表： 用于保存XXL-JOB调度任务的扩展信息，如任务分组、任务名、机器地址、执行器、执行入参和报警邮件等等；
    - xxl_job_log：调度日志表： 用于保存XXL-JOB任务调度的历史信息，如调度结果、执行结果、调度入参、调度机器和执行器等等；
    - xxl_job_log_report：调度日志报表：用户存储XXL-JOB任务调度日志的报表，调度中心报表功能页面会用到；
    - xxl_job_logglue：任务GLUE日志：用于保存GLUE更新历史，用于支持GLUE的版本回溯功能；
    - xxl_job_registry：执行器注册表，维护在线的执行器和调度中心机器地址信息；
    - xxl_job_user：系统用户表；


### 5.3 架构设计
#### 5.3.1 设计思想
将调度行为抽象形成“调度中心”公共平台，而平台自身并不承担业务逻辑，“调度中心”负责发起调度请求。

将任务抽象成分散的JobHandler，交由“执行器”统一管理，“执行器”负责接收调度请求并执行对应的JobHandler中业务逻辑。

因此，“调度”和“任务”两部分可以相互解耦，提高系统整体稳定性和扩展性；

#### 5.3.2 系统组成
- **调度模块（调度中心）**：
    负责管理调度信息，按照调度配置发出调度请求，自身不承担业务代码。调度系统与任务解耦，提高了系统可用性和稳定性，同时调度系统性能不再受限于任务模块；
    支持可视化、简单且动态的管理调度信息，包括任务新建，更新，删除，GLUE开发和任务报警等，所有上述操作都会实时生效，同时支持监控调度结果以及执行日志，支持执行器Failover。
- **执行模块（执行器）**：
    负责接收调度请求并执行任务逻辑。任务模块专注于任务的执行等操作，开发和维护更加简单和高效；
    接收“调度中心”的执行请求、终止请求和日志请求等。

#### 5.3.3 架构图

![输入图片说明](https://www.xuxueli.com/doc/static/xxl-job/images/img_Qohm.png "在这里输入图片标题")

### 5.4 调度模块剖析
#### 5.4.1 quartz的不足
Quartz作为开源作业调度中的佼佼者，是作业调度的首选。但是集群环境中Quartz采用API的方式对任务进行管理，从而可以避免上述问题，但是同样存在以下问题：
   
- 问题一：调用API的的方式操作任务，不人性化；
- 问题二：需要持久化业务QuartzJobBean到底层数据表中，系统侵入性相当严重。
- 问题三：调度逻辑和QuartzJobBean耦合在同一个项目中，这将导致一个问题，在调度任务数量逐渐增多，同时调度任务逻辑逐渐加重的情况下，此时调度系统的性能将大大受限于业务；
- 问题四：quartz底层以“抢占式”获取DB锁并由抢占成功节点负责运行任务，会导致节点负载悬殊非常大；而XXL-JOB通过执行器实现“协同分配式”运行任务，充分发挥集群优势，负载各节点均衡。

XXL-JOB弥补了quartz的上述不足之处。

#### 5.4.2 自研调度模块
XXL-JOB最终选择自研调度组件（早期调度组件基于Quartz）；一方面是为了精简系统降低冗余依赖，另一方面是为了提供系统的可控度与稳定性；

XXL-JOB中“调度模块”和“任务模块”完全解耦，调度模块进行任务调度时，将会解析不同的任务参数发起远程调用，调用各自的远程执行器服务。这种调用模型类似RPC调用，调度中心提供调用代理的功能，而执行器提供远程服务的功能。

#### 5.4.3 调度中心HA（集群）
基于数据库的集群方案，数据库选用Mysql；集群分布式并发环境中进行定时任务调度时，会在各个节点会上报任务，存到数据库中，执行时会从数据库中取出触发器来执行，如果触发器的名称和执行时间相同，则只有一个节点去执行此任务。

#### 5.4.4 调度线程池
调度采用线程池方式实现，避免单线程因阻塞而引起任务调度延迟。

#### 5.4.5 并行调度
XXL-JOB调度模块默认采用并行机制，在多线程调度的情况下，调度模块被阻塞的几率很低，大大提高了调度系统的承载量。

XXL-JOB的每个调度任务虽然在调度模块是并行调度执行的，但是任务调度传递到任务模块的“执行器”确实串行执行的，同时支持任务终止。

#### 5.4.6 过期处理策略
任务调度错过触发时间时的处理策略：
- 可能原因：服务重启；调度线程被阻塞，线程被耗尽；上次调度持续阻塞，下次调度被错过；
- 处理策略：
    - 过期超5s：本次忽略，当前时间开始计算下次触发时间
    - 过期5s内：立即触发一次，当前时间开始计算下次触发时间


#### 5.4.7 日志回调服务
调度模块的“调度中心”作为Web服务部署时，一方面承担调度中心功能，另一方面也为执行器提供API服务。

调度中心提供的"日志回调服务API服务"代码位置如下：
```
xxl-job-admin#com.xxl.job.admin.controller.JobApiController.callback
```

“执行器”在接收到任务执行请求后，执行任务，在执行结束之后会将执行结果回调通知“调度中心”：

#### 5.4.8 任务HA（Failover）
执行器如若集群部署，调度中心将会感知到在线的所有执行器，如“127.0.0.1:9997, 127.0.0.1:9998, 127.0.0.1:9999”。

当任务"路由策略"选择"故障转移(FAILOVER)"时，当调度中心每次发起调度请求时，会按照顺序对执行器发出心跳检测请求，第一个检测为存活状态的执行器将会被选定并发送调度请求。

调度成功后，可在日志监控界面查看“调度备注”，如下；
![输入图片说明](https://www.xuxueli.com/doc/static/xxl-job/images/img_jrdI.png "在这里输入图片标题")

“调度备注”可以看出本地调度运行轨迹，执行器的"注册方式"、"地址列表"和任务的"路由策略"。"故障转移(FAILOVER)"路由策略下，调度中心首先对第一个地址进行心跳检测，心跳失败因此自动跳过，第二个依然心跳检测失败……
直至心跳检测第三个地址“127.0.0.1:9999”成功，选定为“目标执行器”；然后对“目标执行器”发送调度请求，调度流程结束，等待执行器回调执行结果。

#### 5.4.9 调度日志
调度中心每次进行任务调度，都会记录一条任务日志，任务日志主要包括以下三部分内容：

- 任务信息：包括“执行器地址”、“JobHandler”和“执行参数”等属性，点击任务ID按钮可查看，根据这些参数，可以精确的定位任务执行的具体机器和任务代码；
- 调度信息：包括“调度时间”、“调度结果”和“调度日志”等，根据这些参数，可以了解“调度中心”发起调度请求时具体情况。
- 执行信息：包括“执行时间”、“执行结果”和“执行日志”等，根据这些参数，可以了解在“执行器”端任务执行的具体情况；

调度日志，针对单次调度，属性说明如下：
- 执行器地址：任务执行的机器地址；
- JobHandler：Bean模式表示任务执行的JobHandler名称；
- 任务参数：任务执行的入参；
- 调度时间：调度中心，发起调度的时间；
- 调度结果：调度中心，发起调度的结果，SUCCESS或FAIL；
- 调度备注：调度中心，发起调度的备注信息，如地址心跳检测日志等；
- 执行时间：执行器，任务执行结束后回调的时间；
- 执行结果：执行器，任务执行的结果，SUCCESS或FAIL；
- 执行备注：执行器，任务执行的备注信息，如异常日志等；
- 执行日志：任务执行过程中，业务代码中打印的完整执行日志，见“4.8 查看执行日志”；

#### 5.4.10 任务依赖
原理：XXL-JOB中每个任务都对应有一个任务ID，同时，每个任务支持设置属性“子任务ID”，因此，通过“任务ID”可以匹配任务依赖关系。

当父任务执行结束并且执行成功时，将会根据“子任务ID”匹配子任务依赖，如果匹配到子任务，将会主动触发一次子任务的执行。

在任务日志界面，点击任务的“执行备注”的“查看”按钮，可以看到匹配子任务以及触发子任务执行的日志信息，如无信息则表示未触发子任务执行，可参考下图。

![输入图片说明](https://www.xuxueli.com/doc/static/xxl-job/images/img_Wb2o.png "在这里输入图片标题")

![输入图片说明](https://www.xuxueli.com/doc/static/xxl-job/images/img_jOAU.png "在这里输入图片标题")

#### 5.4.11  全异步化 & 轻量级

- 全异步化设计：XXL-JOB系统中业务逻辑在远程执行器执行，触发流程全异步化设计。相比直接在调度中心内部执行业务逻辑，极大的降低了调度线程占用时间；
    - 异步调度：调度中心每次任务触发时仅发送一次调度请求，该调度请求首先推送“异步调度队列”，然后异步推送给远程执行器
    - 异步执行：执行器会将请求存入“异步执行队列”并且立即响应调度中心，异步运行。
- 轻量级设计：XXL-JOB调度中心中每个JOB逻辑非常 “轻”，在全异步化的基础上，单个JOB一次运行平均耗时基本在 "10ms" 之内（基本为一次请求的网络开销）；因此，可以保证使用有限的线程支撑大量的JOB并发运行；

得益于上述两点优化，理论上默认配置下的调度中心，单机能够支撑 5000 任务并发运行稳定运行；

实际场景中，由于调度中心与执行器网络ping延迟不同、DB读写耗时不同、任务调度密集程度不同，会导致任务量上限会上下波动。

如若需要支撑更多的任务量，可以通过 "调大调度线程数" 、"降低调度中心与执行器ping延迟" 和 "提升机器配置" 几种方式优化。

#### 5.4.12 均衡调度    
调度中心在集群部署时会自动进行任务平均分配，触发组件每次获取与线程池数量（调度中心支持自定义调度线程池大小）相关数量的任务，避免大量任务集中在单个调度中心集群节点；

### 5.5 任务 "运行模式" 剖析
#### 5.5.1 "Bean模式" 任务
开发步骤：可参考 "章节三" ；
原理：每个Bean模式任务都是一个Spring的Bean类实例，它被维护在“执行器”项目的Spring容器中。任务类需要加“@JobHandler(value="名称")”注解，因为“执行器”会根据该注解识别Spring容器中的任务。任务类需要继承统一接口“IJobHandler”，任务逻辑在execute方法中开发，因为“执行器”在接收到调度中心的调度请求时，将会调用“IJobHandler”的execute方法，执行任务逻辑。

#### 5.5.2 "GLUE模式(Java)" 任务
开发步骤：可参考 "章节三" ；
原理：每个 "GLUE模式(Java)" 任务的代码，实际上是“一个继承自“IJobHandler”的实现类的类代码”，“执行器”接收到“调度中心”的调度请求时，会通过Groovy类加载器加载此代码，实例化成Java对象，同时注入此代码中声明的Spring服务（请确保Glue代码中的服务和类引用在“执行器”项目中存在），然后调用该对象的execute方法，执行任务逻辑。

#### 5.5.3 GLUE模式(Shell) + GLUE模式(Python) + GLUE模式(PHP) + GLUE模式(NodeJS) + GLUE模式(Powershell)
开发步骤：可参考 "章节三" ；
原理：脚本任务的源码托管在调度中心，脚本逻辑在执行器运行。当触发脚本任务时，执行器会加载脚本源码在执行器机器上生成一份脚本文件，然后通过Java代码调用该脚本；并且实时将脚本输出日志写到任务日志文件中，从而在调度中心可以实时监控脚本运行情况；

目前支持的脚本类型如下：

    - shell脚本：任务运行模式选择为 "GLUE模式(Shell)"时支持 "Shell" 脚本任务；
    - python脚本：任务运行模式选择为 "GLUE模式(Python)"时支持 "Python" 脚本任务；
    - php脚本：任务运行模式选择为 "GLUE模式(PHP)"时支持 "PHP" 脚本任务；
    - nodejs脚本：任务运行模式选择为 "GLUE模式(NodeJS)"时支持 "NodeJS" 脚本任务；
    - powershell：任务运行模式选择为 "GLUE模式(PowerShell)"时支持 "PowerShell" 脚本任务；

脚本任务通过 Exit Code 判断任务执行结果，状态码可参考章节 "5.15 任务执行结果说明"；

#### 5.5.4 执行器
执行器实际上是一个内嵌的Server，默认端口9999（配置项：xxl.job.executor.port）。

在项目启动时，执行器会通过“@JobHandler”识别Spring容器中“Bean模式任务”，以注解的value属性为key管理起来。

“执行器”接收到“调度中心”的调度请求时，如果任务类型为“Bean模式”，将会匹配Spring容器中的“Bean模式任务”，然后调用其execute方法，执行任务逻辑。如果任务类型为“GLUE模式”，将会加载GLue代码，实例化Java对象，注入依赖的Spring服务（注意：Glue代码中注入的Spring服务，必须存在与该“执行器”项目的Spring容器中），然后调用execute方法，执行任务逻辑。

#### 5.5.5 任务日志
XXL-JOB会为每次调度请求生成一个单独的日志文件，需要通过 "XxlJobLogger.log" 打印执行日志，“调度中心”查看执行日志时将会加载对应的日志文件。

(历史版本通过重写LOG4J的Appender实现，存在依赖限制，该方式在新版本已经被抛弃)

日志文件存放的位置可在“执行器”配置文件进行自定义，默认目录格式为：/data/applogs/xxl-job/jobhandler/“格式化日期”/“数据库调度日志记录的主键ID.log”。

在JobHandler中开启子线程时，子线程将会将会把日志打印在父线程即JobHandler的执行日志中，方便日志追踪。

### 5.6 通讯模块剖析

#### 5.6.1 一次完整的任务调度通讯流程 
    - 1、“调度中心”向“执行器”发送http调度请求: “执行器”中接收请求的服务，实际上是一台内嵌Server，默认端口9999;
    - 2、“执行器”执行任务逻辑；
    - 3、“执行器”http回调“调度中心”调度结果: “调度中心”中接收回调的服务，是针对执行器开放一套API服务;

#### 5.6.2 通讯数据加密
调度中心向执行器发送的调度请求时使用RequestModel和ResponseModel两个对象封装调度请求参数和响应数据, 在进行通讯之前底层会将上述两个对象对象序列化，并进行数据协议以及时间戳检验,从而达到数据加密的功能;

### 5.7 任务注册, 任务自动发现   
自v1.5版本之后, 任务取消了"任务执行机器"属性, 改为通过任务注册和自动发现的方式, 动态获取远程执行器地址并执行。

    AppName: 每个执行器机器集群的唯一标示, 任务注册以 "执行器" 为最小粒度进行注册; 每个任务通过其绑定的执行器可感知对应的执行器机器列表;
    注册表: 见"xxl_job_registry"表, "执行器" 在进行任务注册时将会周期性维护一条注册记录，即机器地址和AppName的绑定关系; "调度中心" 从而可以动态感知每个AppName在线的机器列表;
    执行器注册: 任务注册Beat周期默认30s; 执行器以一倍Beat进行执行器注册, 调度中心以一倍Beat进行动态任务发现; 注册信息的失效时间为三倍Beat; 
    执行器注册摘除：执行器销毁时，将会主动上报调度中心并摘除对应的执行器机器信息，提高心跳注册的实时性；
    

为保证系统"轻量级"并且降低学习部署成本，没有采用Zookeeper作为注册中心，采用DB方式进行任务注册发现；

### 5.8 任务执行结果
自v1.6.2之后，任务执行结果通过 "IJobHandler" 的返回值 "ReturnT" 进行判断；
当返回值符合 "ReturnT.code == ReturnT.SUCCESS_CODE" 时表示任务执行成功，否则表示任务执行失败，而且可以通过 "ReturnT.msg" 回调错误信息给调度中心；
从而，在任务逻辑中可以方便的控制任务执行结果；

### 5.9 分片广播 & 动态分片   
执行器集群部署时，任务路由策略选择"分片广播"情况下，一次任务调度将会广播触发对应集群中所有执行器执行一次任务，同时系统自动传递分片参数；可根据分片参数开发分片任务；

"分片广播" 以执行器为维度进行分片，支持动态扩容执行器集群从而动态增加分片数量，协同进行业务处理；在进行大数据量业务操作时可显著提升任务处理能力和速度。

"分片广播" 和普通任务开发流程一致，不同之处在于可以获取分片参数，获取分片参数进行分片业务处理。

- Java语言任务获取分片参数方式：BEAN、GLUE模式(Java)
```
// 可参考Sample示例执行器中的示例任务"ShardingJobHandler"了解试用 
int shardIndex = XxlJobContext.getXxlJobContext().getShardIndex();
int shardTotal = XxlJobContext.getXxlJobContext().getShardTotal();
```
- 脚本语言任务获取分片参数方式：GLUE模式(Shell)、GLUE模式(Python)、GLUE模式(Nodejs)
```
// 脚本任务入参固定为三个，依次为：任务传参、分片序号、分片总数。以Shell模式任务为例，获取分片参数代码如下
echo "分片序号 index = $2"
echo "分片总数 total = $3"
```  
    
分片参数属性说明：

    index：当前分片序号(从0开始)，执行器集群列表中当前执行器的序号；
    total：总分片数，执行器集群的总机器数量；

该特性适用场景如：
- 1、分片任务场景：10个执行器的集群来处理10w条数据，每台机器只需要处理1w条数据，耗时降低10倍；
- 2、广播任务场景：广播执行器机器运行shell脚本、广播集群节点进行缓存更新等

### 5.10 访问令牌（AccessToken）
为提升系统安全性，调度中心和执行器进行安全性校验，双方AccessToken匹配才允许通讯；

调度中心和执行器，可通过配置项 "xxl.job.accessToken" 进行AccessToken的设置。

调度中心和执行器，如果需要正常通讯，只有两种设置；

- 设置一：调度中心和执行器，均不设置AccessToken；关闭安全性校验；
- 设置二：调度中心和执行器，设置了相同的AccessToken；

### 5.11 故障转移 & 失败重试
一次完整任务流程包括"调度（调度中心） + 执行（执行器）"两个阶段。
    
- "故障转移"发生在调度阶段，在执行器集群部署时，如果某一台执行器发生故障，该策略支持自动进行Failover切换到一台正常的执行器机器并且完成调度请求流程。
- "失败重试"发生在"调度 + 执行"两个阶段，支持通过自定义任务失败重试次数，当任务失败时将会按照预设的失败重试次数主动进行重试；

### 5.12 执行器灰度上线
调度中心与业务解耦，只需部署一次后常年不需要维护。但是，执行器中托管运行着业务作业，作业上线和变更需要重启执行器，尤其是Bean模式任务。
执行器重启可能会中断运行中的任务。但是，XXL-JOB得益于自建执行器与自建注册中心，可以通过灰度上线的方式，避免因重启导致的任务中断的问题。

步骤如下：
- 1、执行器改为手动注册，下线一半机器列表（A组），线上运行另一半机器列表（B组）；
- 2、等待A组机器任务运行结束并编译上线；执行器注册地址替换为A组；
- 3、等待B组机器任务运行结束并编译上线；执行器注册地址替换为A组+B组；
操作结束；

### 5.13 任务执行结果说明
系统根据以下标准判断任务执行结果，可参考之。

-- | Bean/Glue(Java) | Glue(Shell) 等脚本任务
--- | --- | ---
成功 | IJobHandler.SUCCESS | 0
失败 | IJobHandler.FAIL | -1（非0状态码）

### 5.14 任务超时控制
支持设置任务超时时间，任务运行超时的情况下，将会主动中断任务；

需要注意的是，任务超时中断时与任务终止机制（可查看“4.9 终止运行中的任务”）类似，也是通过 "interrupt" 中断任务，因此业务代码需要将 "InterruptedException" 外抛，否则功能不可用。

### 5.15 跨语言
XXL-JOB是一个跨语言的任务调度平台，主要体现在如下几个方面：
- 1、RESTful API：调度中心与执行器提供语言无关的 RESTful API 服务，第三方任意语言可据此对接调度中心或者实现执行器。（可参考章节 “调度中心/执行器 RESTful API” ）
- 2、多任务模式：提供Java、Python、PHP……等十来种任务模式，可参考章节 “5.5 任务 "运行模式" ”；理论上可扩展任意语言任务模式；
- 2、提供基于HTTP的任务Handler（Bean任务，JobHandler="httpJobHandler"）；业务方只需要提供HTTP链接等相关信息即可，不限制语言、平台；（可参考章节 “原生内置Bean模式任务” ）

### 5.16 任务失败告警
默认提供邮件失败告警，可扩展短信、钉钉等方式。如果需要新增一种告警方式，只需要新增一个实现 "com.xxl.job.admin.core.alarm.JobAlarm" 接口的告警实现即可。可以参考默认提供邮箱告警实现 "EmailJobAlarm"。

### 5.17 调度中心Docker镜像构建
可以通过以下命令快速构建调度中心，并启动运行；
```
mvn clean package
docker build -t xuxueli/xxl-job-admin ./xxl-job-admin
docker run --name xxl-job-admin -p 8080:8080 -d xuxueli/xxl-job-admin
```

### 5.20 避免任务重复执行   
调度密集或者耗时任务可能会导致任务阻塞，集群情况下调度组件小概率情况下会重复触发；
针对上述情况，可以通过结合 "单机路由策略（如：第一台、一致性哈希）" + "阻塞策略（如：单机串行、丢弃后续调度）" 来规避，最终避免任务重复执行。 

### 5.21 命令行任务   
原生提供通用命令行任务Handler（Bean任务，"CommandJobHandler"）；业务方只需要提供命令行即可；
如任务参数 "pwd" 将会执行命令并输出数据；

### 5.22 日志自动清理
XXL-JOB日志主要包含如下两部分，均支持日志自动清理，说明如下：
- 调度中心日志表数据：可借助配置项 "xxl.job.logretentiondays" 设置日志表数据保存天数，过期日志自动清理；详情可查看上文配置说明；
- 执行器日志文件数据：可借助配置项 "xxl.job.executor.logretentiondays" 设置日志文件数据保存天数，过期日志自动清理；详情可查看上文配置说明；

### 5.23 调度结果丢失处理
执行器因网络抖动回调失败或宕机等异常情况，会导致任务调度结果丢失。由于调度中心依赖执行器回调来感知调度结果，因此会导致调度日志永远处于 "运行中" 状态。

针对该问题，调度中心提供内置组件进行处理，逻辑为：调度记录停留在 "运行中" 状态超过10min，且对应执行器心跳注册失败不在线，则将本地调度主动标记失败；


## 六、调度中心/执行器 RESTful API
XXL-JOB 目标是一种跨平台、跨语言的任务调度规范和协议。

针对Java应用，可以直接通过官方提供的调度中心与执行器，方便快速的接入和使用调度中心，可以参考上文 “快速入门” 章节。

针对非Java应用，可借助 XXL-JOB 的标准 RESTful API 方便的实现多语言支持。

- 调度中心 RESTful API：
    - 说明：调度中心提供给执行器使用的API；不局限于官方执行器使用，第三方可使用该API来实现执行器；
    - API列表：执行器注册、任务结果回调等；
- 执行器 RESTful API ：
    - 说明：执行器提供给调度中心使用的API；官方执行器默认已实现，第三方执行器需要实现并对接提供给调度中心；
    - API列表：任务触发、任务终止、任务日志查询……等；

此处 RESTful API 主要用于非Java语言定制个性化执行器使用，实现跨语言。除此之外，如果有需要通过API操作调度中心，可以个性化扩展 “调度中心 RESTful API” 并使用。

### 6.1 调度中心 RESTful API

API服务位置：com.xxl.job.core.biz.AdminBiz （ com.xxl.job.admin.controller.JobApiController ）
API服务请求参考代码：com.xxl.job.adminbiz.AdminBizTest

#### a、任务回调
```
说明：执行器执行完任务后，回调任务结果时使用

------

地址格式：{调度中心跟地址}/callback

Header：
    XXL-JOB-ACCESS-TOKEN : {请求令牌}
 
请求数据格式如下，放置在 RequestBody 中，JSON格式：
    [{
        "logId":1,              // 本次调度日志ID
        "logDateTim":0,         // 本次调度日志时间
        "executeResult":{
            "code": 200,        // 200 表示任务执行正常，500表示失败
            "msg": null
        }
    }]

响应数据格式：
    {
      "code": 200,      // 200 表示正常、其他失败
      "msg": null      // 错误提示消息
    }
```
    
#### b、执行器注册
```
说明：执行器注册时使用，调度中心会实时感知注册成功的执行器并发起任务调度

------

地址格式：{调度中心跟地址}/registry

Header：
    XXL-JOB-ACCESS-TOKEN : {请求令牌}
 
请求数据格式如下，放置在 RequestBody 中，JSON格式：
    {
        "registryGroup":"EXECUTOR",                     // 固定值
        "registryKey":"xxl-job-executor-example",       // 执行器AppName
        "registryValue":"http://127.0.0.1:9999/"        // 执行器地址，内置服务跟地址
    }

响应数据格式：
    {
      "code": 200,      // 200 表示正常、其他失败
      "msg": null      // 错误提示消息
    }
```

#### c、执行器注册摘除
```
说明：执行器注册摘除时使用，注册摘除后的执行器不参与任务调度与执行

------

地址格式：{调度中心跟地址}/registryRemove

Header：
    XXL-JOB-ACCESS-TOKEN : {请求令牌}
 
请求数据格式如下，放置在 RequestBody 中，JSON格式：
    {
        "registryGroup":"EXECUTOR",                     // 固定值
        "registryKey":"xxl-job-executor-example",       // 执行器AppName
        "registryValue":"http://127.0.0.1:9999/"        // 执行器地址，内置服务跟地址
    }

响应数据格式：
    {
      "code": 200,      // 200 表示正常、其他失败
      "msg": null      // 错误提示消息
    }
```

### 6.2 执行器 RESTful API

API服务位置：com.xxl.job.core.biz.ExecutorBiz
API服务请求参考代码：com.xxl.job.executorbiz.ExecutorBizTest

#### a、心跳检测
```
说明：调度中心检测执行器是否在线时使用

------

地址格式：{执行器内嵌服务跟地址}/beat

Header：
    XXL-JOB-ACCESS-TOKEN : {请求令牌}
 
请求数据格式如下，放置在 RequestBody 中，JSON格式：

响应数据格式：
    {
      "code": 200,      // 200 表示正常、其他失败
      "msg": null       // 错误提示消息
    }
```

#### b、忙碌检测
```
说明：调度中心检测指定执行器上指定任务是否忙碌（运行中）时使用

------

地址格式：{执行器内嵌服务跟地址}/idleBeat

Header：
    XXL-JOB-ACCESS-TOKEN : {请求令牌}
 
请求数据格式如下，放置在 RequestBody 中，JSON格式：
    {
        "jobId":1       // 任务ID
    }

响应数据格式：
    {
      "code": 200,      // 200 表示正常、其他失败
      "msg": null       // 错误提示消息
    }
```

#### c、触发任务
```
说明：触发任务执行

------

地址格式：{执行器内嵌服务跟地址}/run

Header：
    XXL-JOB-ACCESS-TOKEN : {请求令牌}
 
请求数据格式如下，放置在 RequestBody 中，JSON格式：
    {
        "jobId":1,                                  // 任务ID
        "executorHandler":"demoJobHandler",         // 任务标识
        "executorParams":"demoJobHandler",          // 任务参数
        "executorBlockStrategy":"COVER_EARLY",      // 任务阻塞策略，可选值参考 com.xxl.job.core.enums.ExecutorBlockStrategyEnum
        "executorTimeout":0,                        // 任务超时时间，单位秒，大于零时生效
        "logId":1,                                  // 本次调度日志ID
        "logDateTime":1586629003729,                // 本次调度日志时间
        "glueType":"BEAN",                          // 任务模式，可选值参考 com.xxl.job.core.glue.GlueTypeEnum
        "glueSource":"xxx",                         // GLUE脚本代码
        "glueUpdatetime":1586629003727,             // GLUE脚本更新时间，用于判定脚本是否变更以及是否需要刷新
        "broadcastIndex":0,                         // 分片参数：当前分片
        "broadcastTotal":0                          // 分片参数：总分片
    }

响应数据格式：
    {
      "code": 200,      // 200 表示正常、其他失败
      "msg": null       // 错误提示消息
    }
```

#### f、终止任务
```
说明：终止任务

------

地址格式：{执行器内嵌服务跟地址}/kill

Header：
    XXL-JOB-ACCESS-TOKEN : {请求令牌}
 
请求数据格式如下，放置在 RequestBody 中，JSON格式：
    {
        "jobId":1       // 任务ID
    }
    

响应数据格式：
    {
      "code": 200,      // 200 表示正常、其他失败
      "msg": null       // 错误提示消息
    }
```

#### d、查看执行日志
```
说明：终止任务，滚动方式加载

------

地址格式：{执行器内嵌服务跟地址}/log

Header：
    XXL-JOB-ACCESS-TOKEN : {请求令牌}
 
请求数据格式如下，放置在 RequestBody 中，JSON格式：
    {
        "logDateTim":0,     // 本次调度日志时间
        "logId":0,          // 本次调度日志ID
        "fromLineNum":0     // 日志开始行号，滚动加载日志
    }

响应数据格式：
    {
        "code":200,         // 200 表示正常、其他失败
        "msg": null         // 错误提示消息
        "content":{
            "fromLineNum":0,        // 本次请求，日志开始行数
            "toLineNum":100,        // 本次请求，日志结束行号
            "logContent":"xxx",     // 本次请求日志内容
            "isEnd":true            // 日志是否全部加载完
        }
    }
```



## 七、版本更新日志
### 7.1 版本 V1.1.x，新特性[2015-12-05]
**【于V1.1.x版本，XXL-JOB正式应用于我司，内部定制别名为 “Ferrari”，新接入应用推荐使用最新版本】**
- 1、简单：支持通过Web页面对任务进行CRUD操作，操作简单，一分钟上手；
- 2、动态：支持动态修改任务状态，动态暂停/恢复任务，即时生效；
- 3、服务HA：任务信息持久化到mysql中，Job服务天然支持集群，保证服务HA；
- 4、任务HA：某台Job服务挂掉，任务会平滑分配给其他的某一台存活服务，即使所有服务挂掉，重启时或补偿执行丢失任务；
- 5、一个任务只会在其中一台服务器上执行；
- 6、任务串行执行；
- 7、支持自定义参数；
- 8、支持远程任务执行终止；

### 7.2 版本 V1.2.x，新特性[2016-01-17]
- 1、支持任务分组；
- 2、支持“本地任务”、“远程任务”；
- 3、底层通讯支持两种方式，Servlet方式 + JETTY方式；
- 4、支持“任务日志”；
- 5、支持“串行执行”，并行执行；
	
	说明：V1.2版本将系统架构按功能拆分为：
	
		- 调度模块（调度中心）：负责管理调度信息，按照调度配置发出调度请求；
		- 执行模块（执行器）：负责接收调度请求并执行任务逻辑；
		- 通讯模块：负责调度模块和任务模块之间的信息通讯；
	优点：
	
		- 解耦：任务模块提供任务接口，调度模块维护调度信息，业务相互独立；
		- 高扩展性；
		- 稳定性；

### 7.3 版本 V1.3.0，新特性[2016-05-19]
- 1、遗弃“本地任务”模式，推荐使用“远程任务”，易于系统解耦，任务对应的JobHandler统称为“执行器”；
- 2、遗弃“servlet”方式底层系统通讯，推荐使用JETTY方式，调度+回调双向通讯，重构通讯逻辑；
- 3、UI交互优化：左侧菜单展开状态优化，菜单项选中状态优化，任务列表打开表格有压缩优化；
- 4、【重要】“执行器”细分为：BEAN、GLUE两种开发模式，简介见下文：
	
	“执行器” 模式简介：
		- BEAN模式执行器：每个执行器都是Spring的一个Bean实例，XXL-JOB通过注解@JobHandler识别和调度执行器；
		 -GLUE模式执行器：每个执行器对应一段代码，在线Web编辑和维护，动态编译生效，执行器负责加载GLUE代码和执行；

### 7.4 版本 V1.3.1，新特性[2016-05-23]
- 1、更新项目目录结构：
	- /xxl-job-admin -------------------- 【调度中心】：负责管理调度信息，按照调度配置发出调度请求；
	- /xxl-job-core ----------------------- 公共依赖
	- /xxl-job-executor-example ------ 【执行器】：负责接收调度请求并执行任务逻辑；
	- /db ---------------------------------- 建表脚本
	- /doc --------------------------------- 用户手册
- 2、在新的目录结构上，升级了用户手册；
- 3、优化了一些交互和UI；

### 7.5 版本 V1.3.2，新特性[2016-05-28]
- 1、调度逻辑进行事务包裹；
- 2、执行器异步回调执行日志；
- 3、【重要】在 “调度中心” 支持HA的基础上，扩展执行器的Failover支持，支持配置多执行期地址；

### 7.6 版本 V1.4.0 新特性[2016-07-24]
- 1、任务依赖: 通过事件触发方式实现, 任务执行成功并回调时会主动触发一次子任务的调度, 多个子任务用逗号分隔;
- 2、执行器底层实现代码进行重度重构, 优化底层建表脚本;
- 3、执行器中任务线程分组逻辑优化: 之前根据执行器JobHandler进行线程分组,当多个任务复用Jobhanlder会导致相互阻塞。现改为根据调度中心任务进行任务线程分组,任务与任务执行相互隔离;
- 4、执行器调度通讯方案优化, 通过Hex + HC实现建议RPC通讯协议, 优化了通讯参数的维护和解析流程;
- 5、调度中心, 新建/编辑任务, 界面属性调整: 
    - 5.1、任务新增/编辑界面中去除 "任务名JobName"属性 ,该属性改为系统自动生成: 该字段之前主要用于在 "调度中心" 唯一标示一个任务, 现实意义不大, 因此计划淡化掉该字段,改为系统生成UUID,从而简化任务新建的操作;
    - 5.2、任务新增/编辑界面中去除 "GLUE模式" 复选框位置调整, 改为贴近"JobHandler"输入框右侧;
    - 5.3、任务新增/编辑界面中去除 "报警阈值" 属性;
    - 5.4、任务新增/编辑界面中去除 "子任务Key" 属性, 每个任务全局任务Key可以从任务列表获取, 当本任务执行结束且成功后, 将会根据子任务Key匹配子任务并主动触发一次子任务执行;
- 6、问题修复:
    - 6.1、执行器jetty关闭优化,解决一处可能导致jetty无法关闭的问题;
    - 6.2、执行器任务终止时,执行队列回调优化,解决一处导致任务无法回调的问题；
    - 6.3、调度中心中列表分页参数优化,解决一处因服务器限制post长度而引起的问题;
    - 6.4、执行器Jobhandler注解优化,解决一处因事务代理导致的容器无法加载JobHandler的问题;
    - 6.5、远程调度优化,禁用retry策略,解决一处可能导致重复调用的问题;

Tips: 历史版本(V1.3.x)目前已经Release至稳定版本, 进入维护阶段, 地址见分支 [V1.3](https://github.com/xuxueli/xxl-job/tree/v1.3) 。新特性将会在master分支持续更新。

### 7.7 版本 V1.4.1 新特性[2016-09-06]
- 1、项目成功推送maven中央仓库, 中央仓库地址以及依赖如下: 
    ```
    <!-- http://repo1.maven.org/maven2/com/xuxueli/xxl-job-core/ -->
    <dependency>
        <groupId>com.xuxueli</groupId>
        <artifactId>xxl-job-core</artifactId>
        <version>${最新稳定版}</version>
    </dependency>
    ```
- 2、为适配中央仓库规则, 项目groupId从com.xxl改为com.xuxueli。
- 3、系统版本不在维护在项目跟pom中,各个子模块单独配置版本配置,解决子模块无法单独编译的问题;
- 4、底层RPC通讯,传输数据的字节长度统计规则优化,可节省50%数据传输量;
- 5、IJobHandler取消任务返回值,原通过返回值判断执行状态,逻辑改为:默认任务执行成功,仅在捕获异常时认定任务执行失败。
- 6、系统公共弹框功能,插件化;
- 7、底层表结构,表明统一大写;
- 8、调度中心,异常处理器JSON响应的ContentType修改,修复浏览器不识别的问题;

### 7.8 版本 V1.4.2 新特性[2016-09-29]
- 1、推送新版本 V1.4.2 至中央仓库, 大版本 V1.4 进入维护阶段;
- 2、任务新增时,任务列表偏移问题修复;
- 3、修复一处因bootstrap不支持模态框重叠而导致的样式错乱的问题, 在任务编辑时会出现该问题;
- 4、调度超时和Handler匹配不到时,调度状态优化;
- 5、因catch异常,导致任务不可终止的问题,给出解决方案, 见文档;

### 7.9 版本 V1.5.0 特性[2016-11-13]
- 1、任务注册: 执行器会周期性自动注册任务, 调度中心将会自动发现注册的任务并触发执行。
- 2、"执行器" 新增参数 "AppName" : 是每个执行器集群的唯一标示AppName, 并周期性以AppName为对象进行自动注册。
- 3、调度中心新增栏目 "执行器管理" : 管理在线的执行器, 通过属性AppName自动发现注册的执行器。只有被管理的执行器才允许被使用;
- 4、"任务组"属性改为"执行器": 每个任务需要绑定指定的执行器, 调度地址通过绑定的执行器获取;
- 5、抛弃"任务机器"属性: 通过任务绑定的执行器, 自动发现注册的远程执行器地址并触发调度请求。
- 6、"公共依赖"中新增DBGlueLoader,基于原生jdbc实现GLUE源码的加载器,减少第三方依赖(mybatis,spring-orm等);精简和优化执行器测配置(针对GLUE任务),降低上手难度;
- 7、表结构调整,底层重构优化;
- 8、"调度中心"自动注册和发现,failover: 调度中心周期性自动注册, 任务回调时可以感知在线的所有调度中心地址, 通过failover的方式进行任务回调,避免回调单点风险。

### 7.10 版本 V1.5.1 特性[2016-11-13]
- 1、底层代码重构和逻辑优化，POM清理以及CleanCode；
- 2、Servlet/JSP Spec设定为3.0/2.2
- 3、Spring升级至3.2.17.RELEASE版本；
- 4、Jetty升级版本至8.2.0.v20160908；
- 5、已推送V1.5.0和V1.5.1至Maven中央仓库；

### 7.11 版本 V1.5.2 特性[2017-02-28]
- 1、IP工具类获取IP逻辑优化，IP静态缓存；
- 2、执行器、调度中心，均支持自定义注册IP地址；解决机器多网卡时错误网卡注册的情况；
- 3、任务跨天执行时生成多份日志文件的问题修复；
- 4、底层日志底层日志调整，非敏感日志level调整为debug；
- 5、升级数据库连接池c3p0版本；
- 6、执行器log4j配置优化，去除无效属性；
- 7、底层代码重构和逻辑优化以及CleanCode；
- 8、GLUE依赖注入逻辑优化，支持别名注入；

### 7.12 版本 V1.6.0 特性[2017-03-13]
- 1、通讯方案升级，原基于HEX的通讯模型调整为基于HTTP的B-RPC的通讯模型；
- 2、执行器支持手动设置执行地址列表，提供开关切换使用注册地址还是手动设置的地址；
- 3、执行器路由规则：第一个、最后一个、轮询、随机、一致性HASH、最不经常使用、最近最久未使用、故障转移；
- 4、规范线程模型统一，统一线程销毁方案(通过listener或stop方法，容器销毁时销毁线程；Daemon方式有时不太理想)；
- 5、规范系统配置数据，通过配置文件统一管理；
- 6、CleanCode，清理无效的历史参数；
- 7、底层扩展数据结构以及相关表结构调整；
- 8、新建任务默认为非运行状态；
- 9、GLUE模式任务实例更新逻辑优化，原根据超时时间更新改为根据版本号更新，源码变动版本号加一；

### 7.13 版本 V1.6.1 特性[2017-03-25]
- 1、Rolling日志；
- 2、WebIDE交互重构；
- 3、通讯增强校验，有效过滤非正常请求；
- 4、权限增强校验，采用动态登录TOKEN（推荐接入内部SSO）；
- 5、数据库配置优化，解决乱码问题；

### 7.14 版本 V1.6.2 特性[2017-04-25]
- 1、运行报表：支持实时查看运行数据，如任务数量、调度次数、执行器数量等；以及调度报表，如调度日期分布图，调度成功分布图等；
- 2、JobHandler支持设置任务返回值，在任务逻辑中可以方便的控制任务执行结果；
- 3、资源路径包含空格或中文时资源文件无法加载时，无法准确查看异常信息的问题处理。
- 4、路由策越优化：循环和LFU路由策略计数器自增无上限问题和首次路由压力集中在首台机器的问题修复；

### 7.15 版本 V1.7.0 特性[2017-05-02]
- 1、脚本任务：支持以GLUE模式开发和运行脚本任务，包括Shell、Python和Groovy等类型脚本;
- 2、新增spring-boot类型执行器example项目；
- 3、升级jetty版本至9.2；
- 4、任务运行日志移除log4j组件依赖，改为底层自主实现，从而取消了对日志组件的依赖限制；
- 5、执行器移除GlueLoader依赖，改为推送方式实现，从而GLUE源码加载不再依赖JDBC；
- 6、登录拦截Redirect时获取项目名，解决非根据目录发布时跳转404问题；

### 7.16 版本 V1.7.1 特性[2017-05-08]
- 1、运行日志读写编码统一为UTF-8，解决windows环境下日志乱码问题；
- 2、通讯超时时间限定为10s，避免异常情况下调度线程占用；
- 3、执行器，server启动、销毁和注册逻辑调整；
- 4、JettyServer关闭逻辑优化，修复执行器无法正常关闭导致端口占用和频繁打印c3p0日志的问题；
- 5、JobHandler中开启子线程时，支持子线程输出执行日志并通过Rolling查看。
- 6、任务日志清理功能；
- 7、弹框组件统一替换为layer；
- 8、升级quartz版本至2.3.0；

### 7.17 版本 V1.7.2 特性[2017-05-17]
- 1、阻塞处理策略：调度过于密集执行器来不及处理时的处理策略，策略包括：单机串行（默认）、丢弃后续调度、覆盖之前调度；
- 2、失败处理策略；调度失败时的处理策略，策略包括：失败告警（默认）、失败重试；
- 3、通讯时间戳超时时间调整为180s；
- 4、执行器与数据库彻底解耦，但是执行器需要配置调度中心集群地址。调度中心提供API供执行器回调和心跳注册服务，取消调度中心内部jetty，心跳周期调整为30s，心跳失效为三倍心跳；
- 5、执行参数编辑时丢失问题修复；
- 6、新增任务测试Demo，方便在开发时进行任务逻辑测试；

### 7.18 版本 V1.8.0 特性[2017-07-17]
- 1、任务Cron更新逻辑优化，改为rescheduleJob，同时防止cron重复设置；
- 2、API回调服务失败状态码优化，方便问题排查；
- 3、XxlJobLogger的日志多参数支持；
- 4、路由策略新增 "忙碌转移" 模式：按照顺序依次进行空闲检测，第一个空闲检测成功的机器选定为目标执行器并发起调度；
- 5、路由策略代码重构；
- 6、执行器重复注册问题修复；
- 7、任务线程轮空30次后自动销毁，降低低频任务的无效线程消耗。
- 8、执行器任务执行结果批量回调，降低回调频率提升执行器性能；
- 9、springboot版本执行器，取消XML配置，改为类配置方式；
- 10、执行日志，支持根据运行 "状态" 筛选日志；
- 11、调度中心任务注册检测逻辑优化；

### 7.19 版本 V1.8.1 特性[2017-07-30]
- 1、分片广播任务：执行器集群部署时，任务路由策略选择"分片广播"情况下，一次任务调度将会广播触发集群中所有执行器执行一次任务，可根据分片参数处理分片任务；
- 2、动态分片：分片广播任务以执行器为维度进行分片，支持动态扩容执行器集群从而动态增加分片数量，协同进行业务处理；在进行大数据量业务操作时可显著提升任务处理能力和速度。
- 3、执行器JobHandler禁止命名冲突；
- 4、执行器集群地址列表进行自然排序；
- 5、调度中心，DAO层代码精简优化并且新增测试用例覆盖；
- 6、调度中心API服务改为自研RPC形式，统一底层通讯模型；
- 7、新增调度中心API服务测试Demo，方便在调度中心API扩展和测试；
- 8、任务列表页交互优化，更换执行器分组时自动刷新任务列表，新建任务时默认定位在当前执行器位置；
- 9、访问令牌（accessToken）：为提升系统安全性，调度中心和执行器进行安全性校验，双方AccessToken匹配才允许通讯；
- 10、springboot版本执行器，升级至1.5.6.RELEASE版本；
- 11、统一maven依赖版本管理；

### 7.20 版本 V1.8.2 特性[2017-09-04]
- 1、项目主页搭建：提供中英文文档：https://www.xuxueli.com/xxl-job 
- 2、JFinal执行器Sample示例项目；
- 3、事件触发：除了"Cron方式"和"任务依赖方式"触发任务执行之外，支持基于事件的触发任务方式。调度中心提供触发任务单次执行的API服务，可根据业务事件灵活触发。
- 4、执行器摘除：执行器销毁时，主动通知调度中心并摘除对应执行器节点，提高执行器状态感知的时效性。
- 5、执行器手动设置IP时将会绑定Host；
- 6、规范项目目录，方便扩展多执行器；
- 7、解决执行器回调URL不支持配置HTTPS时问题；
- 8、执行器回调线程销毁前, 批量回调队列中数据，防止任务结果丢失；
- 9、调度中心任务监控线程销毁时，批量对失败任务告警，防止告警信息丢失；
- 10、任务日志文件路径时间戳格式化时SimpleDateFormat并发问题解决；

### 7.21 版本 V1.9.0 特性[2017-12-29]
- 1、新增Nutz执行器Sample示例项目；
- 2、新增任务运行模式 "GLUE模式(NodeJS) "，支持NodeJS脚本任务；
- 3、脚本任务Shell、Python和Nodejs等支持获取分片参数；
- 4、失败重试，完整支持：调度中心调度失败且启用"失败重试"策略时，将会自动重试一次；执行器执行失败且回调失败重试状态（新增失败重试状态返回值）时，也将会自动重试一次；
- 5、失败告警策略扩展：默认提供邮件失败告警，可扩展短信等，扩展代码位置为 "JobFailMonitorHelper.failAlarm"；
- 6、执行器端口支持自动生成(小于等于0时)，避免端口定义冲突；
- 7、调度报表优化，支持时间区间筛选；
- 8、Log组件支持输出异常栈信息，底层实现优化；
- 9、告警邮件样式优化，调整为表格形式，邮件组件调整为commons-email简化邮件操作；
- 10、项目依赖全量升级至较新稳定版本，如spring、jackson等等；
- 11、任务日志，记录发起调度的机器信息；
- 12、交互优化，如登录注销；
- 13、任务Cron长度扩展支持至128位，支持负责类型Cron设置；
- 14、执行器地址录入交互优化，地址长度扩展支持至512位，支持大规模执行器集群配置；
- 15、任务参数“IJobHandler.execute”入参改为“String params”，增强入参通用性。
- 16、IJobHandler提供init/destroy方法，支持在相应任务线程初始化和销毁时进行附加操作；
- 17、任务注解调整为 “@JobHandler”，与任务抽象接口统一；
- 18、修复任务监控线程被耗时任务阻塞的问题；
- 19、修复任务监控线程无法监控任务触发和执行状态均未0的问题；
- 20、执行器动态代理对象，拦截非业务方法的执行；
- 21、修复JobThread捕获Error错误不更新JobLog的问题；
- 22、修复任务列表界面左侧菜单合并时样式错乱问题；
- 23、调度中心项目日志配置改为xml文件格式；
- 24、Log地址格式兼容，支持非"/"结尾路径配置；
- 25、底层系统日志级别规范调整，清理遗留代码；
- 26、建表SQL优化，支持同步创建制定编码的库和表；
- 27、系统安全性优化，登录Token写Cookie时进行MD5加密，同时Cookie启用HttpOnly；
- 28、新增"任务ID"属性，移除"JobKey"属性，前者承担所有功能，方便后续增强任务依赖功能。
- 29、任务循环依赖问题修复，避免子任务与父任务重复导致的调度死循环；
- 30、任务列表新增筛选条件 "任务描述"，快速检索任务；
- 31、执行器Log文件定期清理功能：执行器新增配置项（"xxl.job.executor.logretentiondays"）日志保存天数，日志文件过期自动删除。

### 7.22 版本 V1.9.1 特性[2018-02-22]
- 1、国际化：调度中心实现国际化，支持中文、英文两种语言，默认为中文。
- 2、调度报表新增"运行中"中状态项；
- 3、调度报表优化，报表SQL调优并且新增LocalCache缓存（缓存时间60s），提高大数据量下报表加载速度；
- 4、修复打包部署时资源文件乱码问题；
- 5、修复新版本chrome滚动到顶部失效问题；
- 6、调度中心配置加载优化，取消对配置文件名的强依赖，支持加载磁盘配置；
- 7、修复脚本任务Log文件未正常close的问题；
- 8、项目依赖全量升级至较新稳定版本，如spring、jackson等等；

### 7.23 版本 V1.9.2 特性[2018-10-05]
- 1、任务超时控制：新增任务属性 "任务超时时间"，并支持自定义，任务运行超时将会主动中断任务；
- 2、任务失败重试次数：新增任务属性 "失败重试次数"，并支持自定义，当任务失败时将会按照预设的失败重试次数主动进行重试；同时收敛废弃其他失败重试策略，如调度失败、执行失败、状态码失败等；
- 3、新增任务运行模式 "GLUE模式(PHP) "，支持php脚本任务；
- 4、新增任务运行模式 "GLUE模式(PowerShell) "，支持PowerShell脚本任务；
- 5、调度全异步处理：任务触发之后，推送到调度队列，多线程并发处理调度请求，提高任务调度速率的同时，避免因网络问题导致quartz调度线程阻塞的问题；
- 6、执行器任务结果落盘优化：执行器回调失败时将任务结果写磁盘，待重启或网络恢复时重试回调任务结果，防止任务执行结果丢失；
- 7、任务日志查询速度大幅提升：百万级别数据量搜索速度提升1000倍；
- 8、调度中心提供API服务，支持通过API服务对任务进行查询、新增、更新、启停等操作；
- 9、底层自研Log组件参数占位符改为"{}"，并修复打印有参日志时参数不匹配导致报错的问题；
- 10、任务回调结果优化，支持展示在Rolling log中，方便问题排查；
- 11、底层LocalCache组件兼容性优化，支持jdk9、jdk10及以上版本编译部署；
- 12、告警邮件固定使用 UTF-8 编码格式，修复由机器编码导致的邮件乱码问题；
- 13、告警邮件中展示失败告警信息；
- 14、告警邮箱支持SSL配置；
- 15、Window机器下File.separator不兼容问题修复；
- 16、脚本任务异常Log输出优化；
- 17、任务线程停止变量修饰符优化；
- 18、脚本任务Log文件流关闭优化；
- 19、任务报表成功、失败和进行中统计问题修复；
- 20、核心依赖Core内部国际化处理；
- 21、默认Quartz线程数调整为50；
- 22、新增左侧菜单"运行报表"；
- 23、执行器手动设置IP时取消绑定Host的操作，该IP仅供执行器注册使用；修复指定外网IP时无法绑定执行器Host的问题；
- 24、取消父子任务不可重复的限制，支持循环任务触发等特殊场景；
- 25、任务调度备注中标注任务触发类型，如Cron触发、父任务触发、API触发等等，方便排查调度日志；
- 26、底层日志组件SimpleDateFormat线程安全问题修复；
- 27、执行器通讯线程优化，corePoolSize从256降低至32；
- 28、任务日志表状态字段类型优化；
- 29、GLUE脚本文件自动清理功能，及时清理过期脚本文件；
- 30、执行器注册方式切换优化，切换自动注册时主动同步在线机器，避免执行器为空的问题；
- 31、跨平台：除了提供Java、Python、PHP等十来种任务模式之外，新增提供基于HTTP的任务模式；
- 32、底层RPC序列化协议调整为hessian2；
- 33、修复表字段 “t.order”与数据库关键字冲突查询失败的问题，
- 34、任务属性枚举 "任务模式、阻塞策略" 国际化优化；
- 35、分片任务失败重试优化，仅重试当前失败的分片；
- 36、任务触发时支持动态传参，调度中心与API服务均提供提供动态参数功能；
- 37、任务执行日志、调度日志字段类型调整，改为text类型并取消字数限制；
- 38、GLUE任务脚本字段类型调整，改为mediumtext类型，提高GLUE长度上限；
- 39、任务监控线程Log输出优化，运行中任务的监控Log改为debug级别，减少非核心日志量；
- 40、项目依赖全量升级至较新稳定版本，如spring、Jackson、groovy等等；
- 41、docker支持：调度中心提供 Dockerfile 方便快速构建docker镜像； 

### 7.24 版本 V2.0.0 Release Notes[2018-11-04]
- 1、调度中心迁移到 springboot；
- 2、底层通讯组件迁移至 xxl-rpc；
- 3、容器化：提供官方docker镜像，并实时更新推送dockerhub（docker pull xuxueli/xxl-job-admin），进一步实现产品开箱即用；
- 4、新增无框架执行器Sample示例项目 "xxl-job-executor-sample-frameless"。不依赖第三方框架，只需main方法即可启动运行执行器；
- 5、命令行任务：原生提供通用命令行任务Handler（Bean任务，"CommandJobHandler"）；业务方只需要提供命令行即可；
- 6、任务状态优化，仅运行状态"NORMAL"任务关联至quartz，降低quartz底层数据存储与调度压力；
- 7、任务状态规范：新增任务默认停止状态，任务更新时保持任务状态不变；
- 8、IP获取逻辑优化，优先遍历网卡来获取可用IP；
- 9、任务新增的API服务接口返回任务ID，方便调用方实用；
- 10、组件化优化，移除对 spring 的依赖：非spring应用选用 "XxlJobExecutor" 、spring应用选用 "XxlJobSpringExecutor" 作为执行器组件； 
- 11、任务RollingLog展示逻辑优化，修复超时任务无法查看的问题；
- 12、多项UI组件升级到最新版本，如：CodeMirror、Echarts、Jquery 等；
- 13、项目依赖升级 groovy 至较新稳定版本；pom清理；
- 14、子任务失败重试重试逻辑优化，子任务失败时将会按照其预设的失败重试次数主动进行重试

### 7.25 版本 v2.0.1 Release Notes[2018-11-09]
- 1、左侧菜单折叠动画问题修复；
- 2、调度报表日期分布图默认值统一；
- 3、freemarker对数字默认加千分位问题修复，解决日志ID被分隔导致查看日志失败问题；
- 4、底层通讯组件升级，修复通讯异常时无效等待的问题；
- 5、执行器启动之后jetty停止的问题修复；

### 7.26 版本 v2.0.2 Release Notes[2019-04-20]
- 1、底层通讯方案优化：升级较新版本xxl-rpc，由"JETTY"方案调整为"NETTY_HTTP"方案，执行器内嵌netty-http-server提供服务，调度中心复用容器端口提供服务；
- 2、任务告警逻辑调整，改为通过扫描失败日志方式触发。一方面精确扫描失败任务，降低扫描范围；另一方面取消内存队列，降低线程内存消耗；
- 3、Quartz触发线程池废弃并替换为 "XxlJobThreadPool"，降低线程切换、内存占用带来的消耗，提高调度性能；
- 4、调度线程池隔离，拆分为"Fast"和"Slow"两个线程池，1分钟窗口期内任务耗时达500ms超过10次，该窗口期内判定为慢任务，慢任务自动降级进入"Slow"线程池，避免耗尽调度线程，提高系统稳定性；
- 5、执行器热部署时JobHandler重新初始化，修复由此导致的 "jobhandler naming conflicts." 问题；
- 6、新增Class的加载缓存，解决频繁加载Class会使jvm的方法区空间不足导致OOM的问题；
- 7、任务支持更换绑定执行器，方便任务分组转移和管理；
- 8、调度中心告警邮件发送组件改为 “spring-boot-starter-mail”；
- 9、记住密码功能优化，选中时永久记住；非选中时关闭浏览器即登出；
- 10、项目依赖升级至较新稳定版本，如quartz、spring、jackson、groovy、xxl-rpc等等；
- 11、精简项目，取消第三方依赖，如 commons-collections4、commons-lang3 ;
- 12、执行器回调日志落盘方案复用RPC序列化方案，并移除Jackson依赖；
- 13、底层Log调优，应用正常终止取消异常栈信息打印；
- 14、交互优化，尽量避免新开页面窗口；仅WebIDE支持新开页，并提供窗口快速关闭按钮；任务启、停、删除、触发等轻操作提示改为toast方式，
- 15、任务暂停、删除优化，避免quartz delete不完整导致任务脏数据；
- 16、任务回调、心跳注册成功日志优化，非核心常规日志调整为debug级别，降低冗余日志输出；
- 17、调整首页报表默认区间为本周，避免日志量太大查询缓慢；
- 18、LRU路由更新不及时问题修复；
- 19、任务失败告警邮件发送逻辑优化；
- 20、调度日志排序逻辑调整为按照调度时间倒序，兼容TIDB等主键不连续日志存储组件；
- 21、执行器优雅停机优化；
- 22、连接池配置优化，增强连接有效性验证；
- 23、JobHandler#msg长度限制，修复异常情况下日志超长导致内存溢出的问题；
- 24、升级xxl-rpc至较新版本，修复springboot 2.x版本兼容性问题；

### 7.27 版本 v2.1.0 Release Notes[2019-07-07]
- 1、自研调度组件，移除quartz依赖：一方面是为了精简系统降低冗余依赖，另一方面是为了提供系统的可控度与稳定性；
    - 触发：单节点周期性触发，运行事件如delayqueue；
    - 调度：集群竞争，负载方式协同处理，锁竞争-更新触发信息-推送时间轮-锁释放-锁竞争；
- 2、底层表结构重构：移除11张quartz相关表，并对现有表结构优化梳理；
- 3、任务日志主键调整为long数据类型，防止海量日志情况下数据溢出；
- 4、底层线程模型重构：移除Quartz线程池，降低系统线程与内存开销；
- 5、用户管理：支持在线管理系统用户，存在管理员、普通用户两种角色；
- 6、权限管理：执行器维度进行权限控制，管理员拥有全量权限，普通用户需要分配执行器权限后才允许相关操作；
- 7、调度线程池参数调优；
- 8、注册表索引优化，缓解锁表问题；
- 9、新增Jboot执行器Sample示例项目；
- 10、任务列表优化，支持根据 "任务状态"、"负责人" 属性筛选任务；
- 11、任务日志列表交互优化，操作按钮合并为分割按钮；
- 12、项目依赖升级至较新稳定版本，如spring、springboot、groovy、xxl-rpc等等；并清理冗余POM；
- 13、升级xxl-rpc至较新版本，修复代理服务初始化时远程服务不可用导致长连冗余创建的问题;
- 14、首页调度报表的日期排序在TIDB下乱序问题修复；
- 15、调度中心与执行器双向通讯超时时间调整为3s；
- 16、调度组件销毁流程优化，先停止调度线程，然后等待时间轮内存量任务处理完成，最终销毁时间轮线程；
- 17、执行器回调线程优化，回调地址为空时销毁问题修复；
- 18、HttpJobHandler优化，响应数据指定UTF-8格式，避免中文乱码；
- 19、代码优化，ConcurrentHashMap变量类型改为ConcurrentMap，避免因不同版本实现不同导致的兼容性问题；

### 7.28 版本 v2.1.1 Release Notes[2019-11-24]
- 1、 调度中心日志自动清理功能（至此，调度中心/执行器均支持日志自动清理，过期天数均默认设置为30天）：调度中心新增配置项（"xxl.job.logretentiondays"）日志保存天数，过期日志自动清理；解决海量日志情况下日志表慢SQL问题；限制大于等于7时生效，否则关闭清理功能，默认为30；
- 2、 调度报表优化：新增日志报表的存储表，三天内的任务日志会以每分钟一次的频率异步同步至报表中；任务报表仅读取报表数据，极大提升加载速度；
- 3、 Cron在线生成工具：任务新增、编辑框通过组件在线生成Cron表达式；
- 4、 Cron下次执行时间查询：支持通过界面在线查看后续连续5次执行时间；
- 5、 调度中心新增应用健康检查功能，借助“spring-boot-starter-actuator”，相对地址 “/actuator/health”；
- 6、 DB脚本默认编码改为utf8mb4，修复字符乱码问题(建议Mysql版本5.7+)；
- 7、 调度中心任务平均分配，触发组件每次获取与线程池数量相关数量的任务，避免大量任务集中在单个调度中心集群节点；
- 8、 任务触发组件优化，预加载频率正常1s一次，当预加载轮空时主动休眠一个加载周期，动态降低加载频率从而降低DB压力；
- 9、 调度组件优化：针对永远不会触发的Cron禁止配置和启动；任务Cron最后一次触发后再也不会触发时，比如一次性任务，主动停止相关任务；
- 10、DB重连优化，修复DB宕机重连后任务调度停止的问题，重连后自动加入调度集群触发任务调度；
- 11、注册监控线程优化，降低死锁几率；
- 12、调度中心日志删除优化，改为分页获取ID并根据ID删除的方式，避免批量删除海量日志导致死锁问题；
- 13、任务重试时参数丢失的问题修复；
- 14、调度中心移除SQL中的 "now()" 函数；集群部署时不再依赖DB时钟，仅需要保证调度中心应用节点时钟一致即可；
- 15、任务触发组件加载顺序调整，避免小概率情况下组件随机加载顺序导致的I18N的NPE问题;
- 16、JobThread自销毁优化，避免并发触发导致triggerQueue中任务丢失问题；
- 17、调度中心密码限制18位，修复修改密码超过18位无法登录的问题；
- 18、任务告警组件分页参数无效问题修复；
- 19、升级xxl-rpc版本：服务端线程优化，降低线程内存开销；IpUtil优化：增加连通性校，过滤明确非法的网卡；
- 20、调度中心回调API服务改为restful方式；
- 21、UI优化，任务列表和日志列表数据表格宽度比例调整，避免数据换行提升体验；
- 22、登录界面取消默认填写的登录账号密码；
- 23、执行器表属性调整，"顺序" 属性调整为整型，解决执行器数据较多时无法正确排序的问题；
- 24、任务列表交互优化，支持查看任务所属执行器的注册节点；
- 25、项目依赖升级至较新稳定版本，如spring、spring-boot、mybatis、slf4j、groovy等等；
- 26、日志组件优化：调度中心支持控制每次请求最大加载行数，日志量太大时分批请求，避免单次加载日志量太大阻塞页面；

### 7.29 版本 v2.1.2 Release Notes[2019-12-12]
- 1、方法任务支持：由原来基于JobHandler类任务开发方式，优化为支持基于方法的任务开发方式；因此，可以支持单个类中开发多个任务方法，进行类复用
```
@XxlJob("demoJobHandler")
public ReturnT<String> execute(String param) {
    XxlJobLogger.log("hello world");
    return ReturnT.SUCCESS;
}
```
- 2、移除commons-exec，采用原生方式实现，降低第三方依赖；
- 3、执行器回调乱码问题修复；
- 4、调度中心dispatcher servlet加载顺序优化；
- 5、执行器回调地址https兼容支持；
- 6、多个项目依赖升级至较新稳定版本；
- 注意：最新版本 "XxlJobSpringExecutor" 逻辑有调整，历史项目中该组件的配置方式请参考Sample示例项目进行调整，尤其注意需要移除组件的init和destroy方法；

### 7.30 版本 v2.2.0 Release Notes[2020-04-14]
- 1、RESTful API：调度中心与执行器提供语言无关的 RESTful API 服务，第三方任意语言可据此对接调度中心或者实现执行器。
- 2、任务复制功能：点击复制是弹出新建任务弹框，并初始化被复制任务信息；
- 3、任务手动执行一次的时候，支持指定本次执行的机器地址，为空则从执行器获取；
- 4、任务结果丢失处理：调度记录停留在 "运行中" 状态超过10min，且对应执行器心跳注册失败不在线，则将本地调度主动标记失败；
- 5、调度中心升级springboot2.x；因此，系统要求JDK8+；
- 6、XxlJob注解扫描方式优化，支持查找父类以及接口和基于类代理等常见情况；修复任务为空时小概率NPE问题；
- 7、移除旧类注解JobHandler，推荐使用基于方法注解 "@XxlJob" 的方式进行任务开发；(如需保留类注解JobHandler使用方式，可以参考旧版逻辑定制开发);
- 8、任务告警组件模块化：如果需要新增一种告警方式，只需要新增一个实现 "com.xxl.job.admin.core.alarm.JobAlarm" 接口的告警实现即可，更加灵活、方便定制；
- 9、调度中心国际化完善：新增 "中文繁体" 支持。默认为 "zh_CN"/中文简体, 可选范围为 "zh_CN"/中文简体, "zh_TC"/中文繁体 and "en"/英文；
- 10、执行器注册逻辑优化：新增配置项 ”注册地址 / xxl.job.executor.address“，优先使用该配置作为注册地址，为空时使用内嵌服务 ”IP:PORT“ 作为注册地址。从而更灵活的支持容器类型执行器动态IP和动态映射端口问题。
- 11、默认数据库连接池调整为hikari，移除tomcat-jdbc依赖；
- 12、多个项目依赖升级至较新稳定版本，如mybatis、groovy和mysql驱动等；
- 13、执行器优雅停机优化，修复任务线程中断未join导致回调丢失的问题；
- 14、一致性哈希路由策略优化：默认虚拟节点数量调整为100，提高路由的均衡性；
- 15、通用HTTP任务Handler（httpJobHandler）优化，扩展自定义参数信息，示例参数如下；
```
url: http://www.xxx.com
method: get 或 post
data: post-data
```
- 16、SQL脚本编码默认utf8mb4执行，避免小概率下容器环境中乱码问题；
- 17、Web IDE交互问题修复：输入源码备注之后按回车跳转error问题处理；
- 18、执行器初始化逻辑优化：修复懒加载的Bean被提前初始化问题；
- 19、执行器注册默认值优化；
- 20、修复bootstrap.min.css.map 404问题；
- 21、执行器UI交互优化,移除冗余order属性；
- 22、执行备注消息长度限制，修复数据超长无法存储导致导致回调失败的问题；
注意：XxlJobSpringExecutor组件个别字段调整：“appName” 调整为 “appname” ，升级时该组件时需要注意；

### 7.31 版本 v2.2.1 Release Notes[迭代中]
- 1、Cron编辑器增强：Cron编辑器修改cron时可实时查看最近运行时间;
- 2、Cron编辑器问题修复：修复小概率情况下cron单个字段修改时导致其他字段被重置问题；
- 3、邮箱告警配置优化：将"spring.mail.from"与"spring.mail.username"属性拆分开，更加灵活的支持一些无密码邮箱服务；
- 4、多个项目依赖升级至较新稳定版本，如netty、groovy、spring、springboot、mybatis等；
- 5、通用HTTP任务Handler（httpJobHandler）优化：修复 "setDoOutput(true)" 导致任务请求GetMethod失效问题；
- 6、新增任务属性 "XxlJobContent" ，统一维护任务上下文信息，包括任务ID、分片参数等，方便运行时存取任务相关信息；
    - 6.1、废弃 "ShardingUtil" 组件：改用 "XxlJobContext.getXxlJobContext().getShardIndex()/getShardTotal();" 获取分片参数；
    - 6.2、日志组件逻辑调整：日志组件改为通过 XxlJobContent 获取任务上下文并匹配写入对应日志文件；
- 7、页面redirect跳转后https变为http问题修复；
- 8、[规划中]任务触发参数优化：支持选择 "Cron触发"、"固定间隔时间触发"、"指定时间点触发"、"不选择" 等；

### 7.32 版本 v2.3.0 Release Notes[规划中]
- 1、[规划中]多数据库支持，DAO层通过JPA实现，不限制数据库类型；
- 2、[规划中]告警增强：邮件告警 + webhook告警；
- 3、[规划中]DAG流程任务

### TODO LIST
- 1、任务分片路由：分片采用一致性Hash算法计算出尽量稳定的分片顺序，即使注册机器存在波动也不会引起分批分片顺序大的波动；目前采用IP自然排序，可以满足需求，待定；
- 2、调度隔离：调度中心针对不同执行器，各自维护不同的调度和远程触发组件。
- 3、调度任务优先级；
- 4、多数据库支持，DAO层通过JPA实现，不限制数据库类型；
- 5、执行器Log清理功能：调度中心Log删除时同步删除执行器中的Log文件；
- 6、延时任务：API触发，支持"动态传参、延时消费"；该功能与 XXL-MQ 冲突，该场景建议用后者；
- 7、调度线程池改为协程方式实现，大幅降低系统内存消耗；
- 8、任务、执行器数据全量本地缓存；新增消息表广播通知；
- 9、忙碌转移优化，全部机器忙碌时不再直接失败；
- 10、任务触发参数优化：支持选择 "Cron触发"、"固定间隔时间触发"、"指定时间点触发"、"不选择" 等；
- 11、调度日志列表加上执行时长列，并支持排序；
- 12、DAG流程任务：
    - 替换子任务，支持参数传递，共享数据：
    - 配置并列的"a-b、b-c"路径列表，构成串行、并行、dag任务流程，"dagre-d3"绘图；任务依赖，流程图，子任务+会签任务，各节点日志；支持根据成功、失败选择分支；
    - 分片任务：全部完成后才会出发后置节点；
- 13、日期过滤：支持多个时间段排除；
- 14、告警增强：
    - 邮件告警：支持自定义标题、模板格式；
    - webhook告警：支持自定义告警URL、请求体格式；
- 15、新增任务运行模式 "GLUE模式(GO) "，支持GO任务；
- 16、GLUE 模式 Web Ide 版本对比功能；
- 17、注册中心优化，实时性注册发现：心跳注册间隔10s，refresh失败则首次注册并立即更新注册信息，心跳类似；30s过期销毁；
- 18、提供执行器Docker镜像；
- 19、脚本任务，支持数据参数，新版本仅支持单参数不支持需要兼容；
- 20、批量调度：调度请求入queue，调度线程批量获取调度请求并发起远程调度；提高线程效率；
- 21、执行器端口复用，复用容器端口提供通讯服务；
- 22、分片任务全部成功后触发子任务；
- 23、AccessToken按照执行器维度设置；控制调度、回调；
- 24、新增执行器描述属性；任务名称属性；
- 25、自定义失败重试时间间隔；
