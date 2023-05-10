# bishe
1 绪论
1.1 选题背景
在当今的信息时代，随着企业业务量的不断增长，工作流系统变得越来越重要。作为一种流程管理和自动化工具，工作流系统可以帮助企业有效地管理和优化业务流程，提高生产力和效率。因此，研究如何构建高效的工作流系统，具有重要的实际意义和理论价值。
近年来，随着微服务架构的普及，基于微服务的工作流系统也逐渐成为研究的热点。使用基于Dubbo的微服务框架来构建工作流引擎，能够有效实现工作流系统的可扩展性和灵活性。
而在流程引擎方面，Flowable是一种基于Java的流程引擎，具有开放性和灵活性，被广泛应用于工作流系统的构建。
同时，Kafka作为一款应用极广的分布式消息队列，能够为工作流系统提供良好的对外接口，使得流程变量能够通过消息的方式，被接入到外部系统中。
此外，分布式定时任务也是构建高效工作流系统的关键技术之一。XXL-Job是一款分布式任务调度平台，提供了任务调度中心、执行器和调度器等核心组件，通过该调度平台可以更加可靠完成对流程数据的管控。
在这个背景下，本文提出了一种基于Flowable、dubbo、kafka和XXL-Job的工作流系统架构，该架构具有高可靠性、高可扩展性的特点，可以满足企业在实际业务中的需求。具体来说，本文通过将Flowable作为流程引擎，基于dubbo的微服务框架进行服务治理和RPC调用，使用kafka作为消息中间件实现广播模型，以及利用XXL-Job实现分布式定时任务调度，最终实现了一套完整的工作流系统。
本文的创新点在于将流行的微服务架构应用到工作流系统的构建中，结合广播模型和分布式定时任务实现了高可扩展性和高可靠性的工作流系统，具有重要的实际应用价值。同时，本文还提供了完整的系统设计和实现，为后续相关领域的研究和应用提供了有益的参考。
综上所述，本文的主要目的是构建一套高效的工作流系统，通过结合微服务架构和分布式调度技术实现系统的可扩展性和高可靠。

1.2 国内外研究现状
工作流技术是20世纪八九十年代发展起来的一项新兴技术,它为实现企业经营过程重组、经营过程自动化和过程优化和管理提供了方法和软件支持[2,3].
工作流技术主要是利用计算机科学技术.并结合企业具体产品开发过程与经营管理进行信息化软件系统的应用开发,达到企业经营和生产过程全自动或半自动化的执行和管理[4].工作流技术不仅仅是一门计算机应用技术,它还要与管理科学等其它学科紧密结合起来,是一个涉及多学科交叉的研究领域。
当前在国内外的工作流技术中已经存在了许多成熟的工作流引擎。其中最突出的候选者是Apache Airflow、Prefect、Camunda、Activiti和flowable。
Apache Airfiow和Prefect 是两个用Python编写的强大的工作流管理系统，在很多方面彼此非常相似。要创建工作流，用户必须编写一些Python代码来定义任务以及它们如何相互连接。整个工作流必须形成有向无环图(DAG)。虽然这些系统确实很发达，但它们不适合我们的用例。通过编写代码创建工作流这一事实具有挑战性，因为我们的工作流创建者更多是非开发人员。此外，与BPMN不同，DAG不是标准化的工作流语言。最后，这些系统不能很好地支持需要人工交互的任务。
另一方面，Flowable、Activiti和Camunda有一些相似之处，因为它们来自同一个代码库。一开始只有Activiti，然后在2013年，Camunda从Actviti项目中分离出来。几年后，另一个工具由Activiti制作而成，Flowable。这些系统代替DAG，代表使用BPMN的工作流。此外，用户可以简单地通过在他们的网络浏览器上拖放来工作流。尽管这三个系统都满足了我们的需要,选择Flowable是因为它比Activiti功能更加强大，支持的配置更多，在Git Hub上的收藏量flowable比 Camunda 多得多,能够找到更多的使用先例，能避免很多开发时的问题[5]。

1.3 主要研究内容和技术路线

1.3.1 主要研究内容
本次研究主要关注于如何构建一套基于Flowable、dubbo、kafka和XXL-Job的工作流系统架构，实现高可靠性、高可扩展性，并提供完整的系统设计和实现。具体研究内容包括需求分析、系统架构设计、流程引擎引入与调用、消息中间件引入、分布式定时任务调度。
1.	系统需求分析：首先对现有工作流系统的需求进行分析，确定系统的功能和性能要求，包括流程设计、流程运行、流程监控和流程优化等方面。
2.	系统架构设计：基于微服务架构，将系统分解为多个服务，每个服务负责不同的功能，如流程引擎、服务治理、消息中间件和分布式定时任务调度等。然后，将这些服务通过dubbo进行服务治理和RPC调用，实现服务的高效协作。
3.	流程引擎设计与实现：选择Flowable作为流程引擎，通过对Flowable的二次开发和集成，实现自定义流程节点和流程事件，实现了可扩展性和灵活性。
4.	消息中间件设计与实现：使用Kafka作为消息中间件，通过广播模型，将流程变量和事件通知到外部系统中，实现了系统的对外接口和高并发性。
5.	分布式定时任务调度设计与实现：选择XXL-Job作为分布式定时任务调度平台，实现任务调度中心、执行器和调度器等核心组件，通过XXL-Job实现流程数据的管控和调度。

1.3.2 技术路线
由于微服务架构的扩展性，可靠性和灵活性，本次基于Dubbo的微服务框架进行服务治理和RPC调用，实现了工作流系统。通过将工作流系统划分为多个微服务，能够有效地实现模块化和分布式部署，同时具有高度的可扩展性和可维护性。
其次是流程引擎的选择：经过研究，本次选择了基于Java的流程引擎Flowable作为工作流系统的核心组件。Flowable是一个开放性和灵活性较高的流程引擎，被广泛应用于工作流系统的构建。
然后是消息中间件使用：使用Kafka作为消息中间件，作为本系统中的广播模型。通过Kafka，能够将流程变量等重要信息以消息的形式传递给外部系统，实现系统的高度互联互通。
最后是分布式定时任务调度：项目使用XXL-Job作为分布式任务调度平台，实现对流程数据的管控和分布式调度。XXL-Job提供了任务调度中心、执行器和调度器等核心组件，能够更加可靠地完成对流程数据的管理和控制。
综合上述技术路线，本项目实现了一种基于Flowable、Dubbo、Kafka和XXL-Job的工作流系统架构，该架构具有高可靠性、高可扩展性和灵活性的特点。将微服务架构应用到工作流系统的构建中，结合广播模型和分布式定时任务实现了高可扩展性和高可靠性的工作流系统。

 
2  系统需求分析

2.1 业务流程需求
1. 流程模板配置和管理：
a. 系统管理员可以在后台对流程模板进行配置和管理，包括添加、编辑、删除、启用、禁用等操作；
b. 流程模板的配置包括流程名称、流程分类、流程节点、审批人、审批组、请求内容等。
2. 用户发起流程：
a. 用户在系统中选择要发起的流程类型，填写相关信息并提交申请；
b. 系统接收到申请后，将申请数据作为流程变量存储到数据库中，同时生成一个新的流程实例，之后进入第一个节点，并发送通知给用户告知用户已经提交审批申请。
c. 流程实例的状态更新为“进行中”。
3. 审批人审批：
a. 流程实例进入第一个节点后，系统自动将审批任务分配给相应的审批人；
b. 审批人登录系统后，可以查看待办审批任务，查看相关信息并进行审批；
c. 审批人对审批任务进行处理后，系统将审批结果作为流程变量存储到数据库中，同时将流程实例转移到下一个节点；
d. 流程实例的状态更新为“进行中”。
4. 流程开始、结束、流转到对应的审批人时发送消息通知：
a. 当流程实例进入某个用户任务节点、流程开始、流程结束时，系统会自动向相关人员发送消息通知，通知内容包括流程类型、发起人、流程状态等信息。；
b. 消息通知可以通过邮件、短信、微信等多种渠道发送。
5. 审批结束后发送流程变量到Kafka：
a. 当流程实例的状态更新为“已完成”时，系统将流程实例中的相关数据存储到数据库中，并将流程变量以广播的形式发送到Kafka消息队列；
b. 外部系统可以订阅该主题，以接入工作流系统，获取流程变量进行后续处理。
6. 定时任务删除过期数据：
a. 系统通过分布式定时任务的方式，定期清理过期的流程实例数据，以减少数据库存储开销，保障查询性能；
b. 定时任务的周期可以根据实际情况进行配置。

2.2  系统功能需求
1. 用户管理：作为一个面向企业使用的工作流系统，管理员可填写注册信息完成员工账户的创建。同时员工可以通过输入用户名和密码登录系统。
2. 模板管理：用户需要通过可拖拽的模块自行搭建需要的流程模板，并可以在流程模块中设置审批人、流程名称等配置信息，也可以随时编辑，保存，发布，删除流程模板。此外，模板可以关联表单，用户发起审批流程时，可以配置流程中所需要的表单信息，并通过流程模板传递。
3. 任务管理：用户需要可以在使用界面查询自己有关的流程，包括已发起，由我审批，已完成等任务信息。并可以通过该模块完成任务处理，包括提交，撤销，通过，拒绝等功能。
4. 通知管理：系统可以根据流程状态、审批结果等自动发送通知给相关人员。系统将以接入钉钉通知的方式发送通知，相关人员将在钉钉上看到关于自己的流程信息。
5. 定时任务管理：管理员可以设置定时任务的执行时间和执行频率。
任务执行：系统可以按照预定的时间和频率执行定时任务。
日志记录：系统可以记录定时任务的执行情况，包括执行结果、执行时间等。
6．回调模块: 在系统监听到已完成的流程之后，将通过消息中间件的方式对消息进行发布，到对应的主题，其它外部系统通过监听该主题，可以获得对应的流程变量信息，从而实现系统间的接入。

2.3 本章小结
本章对工作流系统做了较为详细的需求分析，通过对业务逻辑进行研究和抽象，总结了业务系统中的主要流程和功能，为后续的业务流程建模提供了重要依据;对系统总体的功能进行划分，总结出各个功能模块，并总结各个功能模块之间的依赖关联关系，为后续的系统总体设计提供了基础。
 
3 关键技术与方法
3.1  Dubbo
Dubbo是一款高性能、轻量级的Java RPC框架，由阿里巴巴开发和维护。
Dubbo框架主要由三个核心组件组成：提供方（Provider）、消费方（Consumer）和注册中心（Registry）。Provider提供服务的实现，Consumer调用服务，Registry提供服务注册和发现功能。Dubbo框架可以帮助开发者轻松构建高性能、高可用、可扩展的分布式服务应用。
本次主要用到了Dubbo框架中的服务注册，服务发现和RPC调用功能。Dubbo通过注册中心来实现服务注册与发现。RPC（Remote Procedure Call）是一种通信协议，用于在不同的进程或者不同的机器之间进行通信，其目的是让远程调用方法就像本地调用方法一样简单。
以下是Dubbo中服务注册和服务发现功能的概述图：
服务注册是指服务提供方将自己注册到注册中心，以便消费方可以通过注册中心来获取服务提供方的提供的服务。在Dubbo框架中，服务提供方在启动时会将自己的地址、服务名、版本号等信息注册到注册中心。Dubbo支持多种注册中心。本次采用的是Zookeeper，作为Dubbo官方推荐的注册中心，它具有高可用性和一致性特点，同时Dubbo框架对注册中心进行了本地缓存，防止Zookeeper Leader节点宕机之后，集群重新选举，不能对外提供服务。
服务发现是指消费者通过注册中心获取服务提供方的信息列表，以便调用服务提供方的服务。Dubbo框架中，消费方启动时会从注册中心订阅所需的服务，注册中心则会返回服务提供方的地址列表，并在服务提供方列表信息变动时发布变动信息。消费者可以通过负载均衡算法选择提供方来调用。Dubbo框架支持多种负载均衡算法，包括随机、轮询等。
Dubbo的服务注册与发现功能具有如下优点：
去中心化：Dubbo的服务注册与发现采用去中心化的方式，注册中心对本身可做对等集群，可动态增减节点，并且任意一台宕掉后，将自动切换到另一台。
高可用性：由于Dubbo中的任意下游服务都是无状态且多副本的，所以任意一个服务的提供者宕机仍不影响系统的正常运行。
可扩展性：Dubbo支持多种注册中心和负载均衡算法，也可以根据业务需求进行自定义扩展和定制。
总之，Dubbo的服务注册与发现功能可以帮助开发者构建高可用、可扩展的分布式服务应用，提高系统的稳定性和可靠性。
3.2  Kafka
Kafka是一个分布式消息系统，也是一个分布式流处理平台[41]（[41]王云龙.基于 Word2Vec新词识别的评论情感分析系统的研究与实现[D].哈尔滨:哈
滨工业大学,2018.）。Kafka的主要编写语言是scala，具有支持分区(partition)以及多副本(replica)的特点。Kafka作为消息中间件，在两个应用程序之间建立实时流处理管道，数据持久化存储在，最初由LinkedIn公司开发。具有高性能、可靠性、可扩展性等特点，广泛应用于大规模数据处理、削峰限流、系统解耦等场景。
本次主要使用了Kafka中间件提供的发布订阅模型。
Kafka会把消息发送到主题(Topic)中，主题就是生产者(producer)和消费者(consumer)用来订阅发布的对象[45]（ [45]庞洁.基于流计算的集群日志实时分析系统的设计与实现[D].哈尔滨:哈尔滨工业大学。）
Kafka的发布订阅模型是基于主题（Topic）的。每个主题可以有多个生产者向其发送消息，也可以有多个消费者从其中读取消息。生产者将消息发送到主题的分区（Partition）中，消费者从分区中消费消息，各个分区中的消息是有序的，并且在分区内保证了消息的顺序性。
Kafka的消费者组是一组共同消费同一主题下的消息的消费者。消费者组中的每个消费者可以独立消费一个或多个分区中的消息。在同一消费者组中，每个分区只能由一个消费者消费。消费者组内的消费者可以分摊消费压力，提高消息的处理效率。
订阅了同一个主题的不同消费者组，他们对于消息的消费是相互隔离的。当生产者把消息发布到主题后，会被投递给每一个订阅它的消费者组中的一个消费者。
下图描述了两个消费者组对于同一主题下的消息进行相互隔离的消费：
 
Kafka的发布订阅模型和消费者组的概念是Kafka的核心特性，本次使用Kafka中间件进行消息发布，可以支持其它系统通过不同的消费者组对分别消息进行处理，并且相互隔离，互不影响。
3.3  Xxl-job
Xxl-job是一个分布式任务调度平台，最初由许雪里开发。它主要用于定时调度和执行任务，并提供了任务调度、任务执行、任务监控、报警等功能，支持多种任务类型的调度和执行。
跟老牌的分布式任务调度框架Quartz 相比，xxl-job的功能更加强大。
1、性能提升：可以同时调度更多任务；
2、可靠性提升：提供了任务超时、失败、故障转移的处理。
3、可视化的运维：提供操作界面、用户管理、日志、通知配置、自动生成报表等等。
Xxl-job采用了分布式架构，可以实现多台机器共同管理和执行任务，提高了任务的可靠性和稳定性。同时，它还提供了丰富的任务调度策略，可以根据任务的类型、优先级、时间等因素进行灵活的调度和管理。同时Xxl-job是一款开源的任务调度平台，使用和修改都是免费的。
如下是Xxl-job的架构图：
Xxl-job经过调度器线程，通过http请求的方式，请求执行器服务，执行器服务再对对应的任务线程进行调用。
Xxl-job的应用场景非常广泛，可以用于定时任务、数据同步、数据清洗、业务流程等方面的任务调度和执行。它的开源性和易用性，也使得它在国内被广泛使用和推广。
3.4  Zookeeper
zooKeeper是一个开源的分布式协调服务，它主要用于分布式系统的协调管理和配置维护。ZooKeeper提供了一个简单的树形命名空间（类似于文件系统），用于存储和维护分布式应用程序的各种信息。
ZooKeeper的主要特点包括：
1.	分布式：ZooKeeper是一款分布式系统，可以将多个ZooKeeper服务器组成一个集群，实现数据的高可用和容错性。
2.	高可靠性：ZooKeeper采用了多数选举机制，可以在单个服务器宕机的情况下继续提供服务。
3.	简单易用：ZooKeeper提供了简单的API，可以方便地实现数据的读写、监听和通知等功能。
4.	高性能：ZooKeeper采用了内存数据库和快照技术，可以实现快速的读写操作。
5.	可扩展性：ZooKeeper支持多节点部署和分布式事务处理，可以扩展到海量数据存储。
ZooKeeper是一款非常重要的分布式协调服务，可以为分布式系统的开发和部署提供强有力的支持。它通过简单易用的API和高可靠性、高性能的特性，成为了众多分布式应用程序的核心组件。
本次使用ZooKeeper主要应用于命名服务，用于实现分布式命名服务，服务发现、负载均衡等功能。

3.5  Flowable
Flowable是一个使用Java编写的轻量级业务流程引擎。Flowable流程引擎可用于部署BPMN 2.0流程定义（用于定义流程的行业XML标准）， 创建这些流程定义的流程实例，进行查询，访问运行中或历史的流程实例与相关数据。
Flowable可以十分灵活地加入应用/服务/构架中。可以将JAR形式发布的Flowable库加入应用或服务，来嵌入引擎。 以JAR形式发布使Flowable可以轻易加入任何Java环境：Java SE；Tomcat、Jetty或Spring之类的servlet容器；JBoss或WebSphere之类的Java EE服务器，等等。 另外，也可以使用Flowable REST API进行HTTP调用。也有许多Flowable应用（Flowable Modeler, Flowable Admin, Flowable IDM 与 Flowable Task），提供了直接可用的UI示例，可以使用流程与任务。[6]
下图显示了使用Flowable的 Modeler组件开发的示例BPMN工作流。该工作流包含两个用户任务，一个脚本任务和一个决策任务。感谢表单和内容引擎，工作流开发人员可以创建表单，然后链接它们到用户任务以获取用户的输入。脚本任务以所选的编程语言执行自定义脚本，而决策任务可以根据预定义的DMN模型做出决策。
一些常用的工作流节点
名称	符号	含义
开始	 	流程开始节点
网关	 	控制流转路径
任务	 	需要人为控制的节点
结束	 	工作流的结束位置
序列	 	任务之间的流转关系
工作流流程的总入口是ProcessEngine，并派生出一系列引擎API来与Flowable进行交互，如RuntimeService,RepositoryService,ManagementService等，可基于查询需要的流程变量或者推进流程进行。下图是他们之间的派生关系：

通过编码的方式: ProcessEngines.getDefaultProcessEngine()将初始化并构建流程引擎，之后的重复调用都会返回同一个流程引擎。可以通过ProcessEngines.init()创建流程引擎，并由ProcessEngines.destroy()关闭流程引擎。
所有的服务都是无状态的。所以可以很容易的在集群环境的多个节点上运行Flowable，使用同一个数据库，而不用担心上一次调用实际在哪台机器上执行。不论在哪个节点执行，对任何服务的任何调用都是幂等（idempotent）的。
以下是对各个Flowable提供的服务的简要说明：
1.	RepositoryService：这个服务提供了管理与控制部署(deployments)与流程定义(process definitions)的操作。此外，这个服务还可以：查询引擎现有的部署与流程定义。暂停或激活部署中的某些流程，或整个部署。暂停意味着不能再对它进行操作，激活刚好相反，重新使它可以操作。获取各种资源，比如部署中保存的文件，或者引擎自动生成的流程图。获取POJO版本的流程定义。它可以用Java而不是XML的方式查看流程。
2.	 RuntimeService：它用于启动流程定义的新流程实例(是流程定义的实际执行过程)。同一时刻，一个流程定义通常有多个运行中的实例。RuntimeService也用于读取与存储流程变量(流程实例中的数据)，可以在流程的许多地方使用（例如排他网关经常使用流程变量判断流程下一步要走的路径）。RuntimeService还可以用于查询流程实例与执行(Execution)。通常执行是指向流程实例当前位置的指针。最后，还可以在流程实例等待外部触发时使用RuntimeService，使流程可以继续运行。
3.	TaskService：它主要提供人类用户操作的API。例如：查询分派给用户或组的任务；创建独立运行(standalone)任务。这是一种没有关联到流程实例的任务。决定任务的执行用户(assignee)，或者将用户通过某种方式与任务关联，如认领(claim)与完成(complete)任务。
4.	IdentityService：它用于管理（创建，更新，删除，查询……）组与用户。Flowable实际上在运行时并不做任何用户检查。例如任务可以分派给任何用户，而引擎并不会验证系统中是否存在该用户。
5.	FormService：这个服务引入了开始表单(start form)与任务表单(task form)的概念。 开始表单是在流程实例启动前显示的表单，而任务表单是用户完成任务时显示的表单。
6.	HistoryService：暴露Flowable引擎收集的所有历史数据。当执行流程时，引擎会保存许多数据（可配置），例如流程实例启动时间、谁在执行哪个任务、完成任务花费的事件、每个流程实例的执行路径，等等。
7.	ManagementService通常在用Flowable编写用户应用时不需要使用。它可以读取数据库表与表原始数据的信息，也提供了对作业(job)的查询与管理操作。
8.	DynamicBpmnService可用于修改流程定义中的部分内容，而不需要重新部署它。例如可以修改流程定义中一个用户任务的办理人设置，或者修改一个服务任务中的类名。

此外，Flowable引擎中的事件机制可以让你在引擎中发生多种事件的时候得到通知。所有被分发的事件都是org.flowable.engine.common.api.delegate.event.FlowableEvent的子类。事件（在可用时）提供type, executionId, processInstanceId与processDefinitionId。通过实现org.flowable.engine.delegate.event.FlowableEventListener接口可以实现对事件的监听，本次主要使用到的事件类型是：
事件名称	说明	事件类
ENTITY_CREATED	新的实体已经创建。该实体包含在本事件里。	org.flowable…FlowableEntityEvent
PROCESS_CREATED	流程实例已经创建。已经设置所有的基础参数，但还未设置变量。	org.flowable…FlowableEntityEvent
PROCESS_COMPLETED	流程实例已经完成。在最后一个节点的 ACTIVITY_COMPLETED 事件后分发。当流程实例没有任何路径可以继续时，流程结束。	org.flowable…FlowableEntityEvent
综上，通过Flowable提供的API和事件入口，可以在分布式环境下完成对流程的管控。
4  总体设计
4.1 总体设计方案
本章主要写了该系统的总体设计方案，意在从需求到设计实现之间架起桥梁。总体设计需要从一定的设计原理出发，对实现框架，开发环境等进行合理选择。统筹用户体验和实现方案，做出恰当的选择，本章主要介绍了系统开发环境的搭建及相关的构建工具。
首先完成系统的总体架构，规划系统的模块构成和主要技术。其次做数据库表设计，说明数据库中关键结构的设计方法和内容，最后是系统运行和测试。	设计思路围绕着工作流引擎进行，总体上将任务管理、微服务、消息队列、定时任务、消息通知等结合，建立起以工作流引擎为核心的过程模型，并通过流程引擎的动态控制机制，使得流程执行过程自动化。 
4.2 系统开发环境
4.2.1开发环境搭建
本系统使用Java语言开发，使用Flowable作为工作流引擎。开发工具选用IntelliJ IDEA，使用Zookeeper作为Kafka消息队列和Dubbo框架的注册中心，使用Kafka作为消息队列，使用Xxl-job作为分布式任务调度中心。以下是工作流系统的开发环境搭建和配置过程。
Windows10平台为主要程序的开发环境，同时以Ubuntu 20.04.2作为程序中间件的运行环境。需要在Ubuntu中运行的已经在下方标明
首先需要下载相关软件:
1. 开发工具:IntelliJ IDEA 2021
2. JDK8:JDK1.8.0_151
3. ZooKeeper: zookeeper-3.4.12 Ubuntu 20.04.2
4.  Kafka：kafka_2.11-2.0.0 Ubuntu 20.04.2
5．Xxl-job: Xxl-job-2.2.0

4.2.2 流程设计器
Flowable引擎中的流程定义文件需要通过标准的bpmn20.xml格式文件生成，可以使用标准的Flowable Designer生成，也可以通过一些开源的vue组件接入，实现可拖拽式生成。本文使用后者的方式实现，可以更加方便地实现在前端部分进行接入，可以使用以下命令在前端进行接入：
yarn add workflow-bpmn-modeler
	由前端生成的bpmn20.xml文件最后可以通过前面第三章提到的流程引擎API:RepositoryService进行部署，主要代码如下图所示。
repositoryService
.createDeployment()
.addInputStream(name, bpmn.xml)
.deploy();
4.3 系统的总体架构
根据之前的流程分析和设计方案，可以得到如下的系统总体架构图。
	基础组件部分主要是SpringBoot，Mysql，Dubbo，Kafka，Xxl-Job，Redis，Zookeeper。其中SpringBoot+Dubbo做为后台的主要调用框架，由Mysql，Redis，Zookeeper，Kafka作为主要的消息和存储中间件，由Xxl-job作为分布式定时任务的调度器。
系统的主要模块为：
1.	用户模块：同于管理用户信息。
2.	模板管理模块：用于存储需要的模板数据和表单数据。
3.	流程处理模块：用于流程管控，基于此模块可以推进流程。
4.	分布式定时任务模块：用于对过期数据进行定时删除。
5.	消息通知模块：在用户请求或者接收到流程时进行消息通知。
6.	流程数据发布模块：在流程结束后，使用广播的方式发布流程变量。
7.	流程事件处理：在用户任务开始，流程结束时进行定制化处理。

4.4数据库表设计和消息设计
4.4.1数据库表设计
	流程引擎需要用到的相关数据库表，可以在Flowable发布的文件夹中找到。说明如下：
Flowable的所有数据库表都以ACT_开头。
1.	ACT_RE_开头：'RE’代表repository。带有这个前缀的表包含“静态”信息，例如流程定义与流程资源（图片、规则等）。
2.	ACT_RU_开头：'RU’代表runtime。这些表存储运行时信息，例如流程实例（process instance）、用户任务（user task）、变量（variable）、作业（job）等。
3.	ACT_HI_开头: 'HI’代表history。这些表存储历史数据，例如已完成的流程实例、变量、任务等。

4.	ACT_GE_开头: 通用数据。在多处使用。

以下是本项目中使用到的数据库表：
act_ge_bytearray：二进制表，用于存储流程定义、流程资源文件、脚本文件等二进制数据。act_ge_property：属性配置表，存储Flowable引擎的属性配置信息，例如数据库类型、版本等。
act_hi_actinst：活动实例表，记录所有流程实例中的活动节点信息，包括活动的开始时间、结束时间、耗时等。
act_hi_comment：注释信息表，存储与流程实例或任务相关的所有注释信息。act_hi_identitylink：历史用户信息表，记录所有与流程实例或任务相关的用户和用户组信息。
act_hi_procinst：历史流程实例表，记录所有已完成的流程实例信息，包括流程实例的开始时间、结束时间、状态等。
act_hi_taskinst：历史任务信息表，记录所有已完成的任务信息，包括任务的开始时间、结束时间、处理人等。
act_hi_varinst：变量信息表，记录所有流程实例和任务中的变量信息。act_re_deployment：部署信息表，记录所有流程部署的信息，包括部署时间、版本号等。
act_re_procdef：流程定义表，记录所有流程定义的信息，包括流程定义ID、版本号等。
act_re_actinst：活动实例表，记录所有活动节点的信息，包括节点ID、名称、流程定义ID等。
act_ru_execution：执行记录表，记录所有流程实例的运行时信息，包括流程实例ID、当前活动节点、业务键等。
act_ru_identitylink：运行时用户信息表，记录所有与流程实例或任务相关的用户和用户组信息。
act_ru_task：运行时任务信息表，记录所有运行中的任务信息，包括任务ID、任务名称、流程定义ID等。
act_ru_variable：运行时实例变量表，记录所有流程实例和任务中的变量信息。department: 部门信息表,记录所有的部门信息。
form: 表单表，记录流程定义中的表单信息。
role: 角色表，记录角色的详细信息。
user: 用户表，记录用户的详细信息。
user_post: 用户职位表，记录用户对应的职位信息。
user_role: 用户角色表，记录用户对应的角色信息。

4.4.2消息设计
对于已经完成的流程，需要将流程定义的key作为消息的key，以及将流程变量的值作为消息的value，之后通过Kafka消息队列进行广播。
主题（Topic）名称定义为：flowableCallBack。
消息键（Key）序列化方式为：org.apache.kafka.common.serialization.StringSerializer。
消息值（Value）序列化方式为：org.apache.kafka.common.serialization.StringSerializer。

4.5系统关键设计
4.5.1 Dubbo接口设计
对本系统的各个模块整合之后，将用户模块和通知模块这两个模块作为服务的提供者。在各个模块中引入如下依赖。
<dependency>
                <groupId>org.apache.dubbo</groupId>
                <artifactId>dubbo-bom</artifactId>
                <version>${dubbo.version}</version>
                <type>pom</type>
                <scope>import</scope>
</dependency>

<dependency>
           	<groupId>org.apache.dubbo</groupId>
                <artifactId>dubbo-dependencies-zookeeper-curator5</artifactId>
                <version>${dubbo.version}</version>
                <type>pom</type>
</dependency>
根据实际的业务需求，定义出接口模块，并将selectUserById（int id）, sendLinkMsg(String userIdList, MsgType msgType, String content, String url, String title)等方法声明到接口模块中，之后分别在生产者模块，消费者模块中实现和调用。

4.5.2 Flowable流程引擎引入
首先在flowable模块的pom文件中导入以下的Maven依赖。
<dependency>
			<groupId>org.flowable</groupId>
			<artifactId>flowable-spring-boot-starter</artifactId>
</dependency>
将该依赖导入成功之后就可以在项目中通过自动注入的方式(@Resource
RuntimeService runtimeService;)，就可以获取到各个流程引擎API，再在各个接口中引用。
常用的API使用如下：
保存流程定义：repositoryService.createDeployment().
                addInputStream(name, bpmn20.xml)
                .name(name)
                .category(category)
                .deploy();
	启动流程实例：runtimeService
.startProcessInstanceById(procDefId, variables);
获取用户发起的所有流程：historyService.createHistoricProcessInstanceQuery()
                .startedBy(userId)
                .orderByProcessInstanceStartTime()
                .desc();
	通过审批：taskService.complete(taskId,variables);
	获取流程变量: historyService.createHistoricTaskInstanceQuery()
                .includeProcessVariables()
                .finished()
                .taskId(taskId)
                .singleResult();
	通过上述调用API接口的方式，可以完成流程定义的部署，发布，挂起；用户流程的发起，撤销，审批等操作。由于Flowable流程引擎会将流程节点按照流程模板的定义而自动流转，所以在实现时只用关注当前节点进行操作。
4.5.3 流程接入通知
	为了使用户能够感知到流程节点的流转，项目在流程发起、流转和结束时接入了钉钉通知。具体实现过程为：
	首先引入钉钉客户端的Maven依赖: 
<dependency>
            <groupId>com.aliyun</groupId>
            <artifactId>alibaba-dingtalk-service-sdk</artifactId>
            <version>2.0.0</version>
        </dependency>
接着实现Iface暴露的sendLinkMsg(String userIdList, MsgType msgType,
                            String content, String url, String title)接口。通过钉钉开发者服务发送一个卡片消息。
同时Flowable引擎中的事件机制可以在引擎中发生多种事件的时候得到通知，即发布订阅模式。首先需要定义接口org.flowable.common.engine.api.delegate.event.FlowableEventListener
的实现类，这里是WorkFlowEventListener。然后需要将它注册到Flowable引擎中去，这里我们在SpringBoot服务启动之后，获取ApplicationContext上下文，然后将该自定义的监听器注册到Flowable引擎中去，就可以在事件发生时，完成自定义的功能。
RuntimeService runtimeService = applicationContext.getBean(RuntimeService.class);
WorkFlowEventListener workFlowEventListener = applicationContext.getBean(WorkFlowEventListener.class);
runtimeService.addEventListener(workFlowEventListener);
这里在以下三个事件ENTITY_CREATED，PROCESS_CREATED，PROCESS_COMPLETED
发生时，调用了钉钉通知，分别实现列流程流转时通知审批人，流程开始和结束时通知发起人。

4.5.4 流程变量发布
	接上节，在流程结束的时候，将该流程名称和流程变量广播到Kafka中，可以便于外部系统接入。
	首先在Maven项目中引入Kafka依赖。
	<dependency>
			<groupId>org.springframework.kafka</groupId>
			<artifactId>spring-kafka</artifactId>
	</dependency>
然后再在流程结束事件中添加消息发送逻辑：
Properties properties = new Properties();
//消息键序列化方式
properties.put("key.serializer",                    "org.apache.kafka.common.serialization.StringSerializer");
//消息值序列化方式
properties.put("value.serializer",                   "org.apache.kafka.common.serialization.StringSerializer");
//Kafka的broker地址
properties.put("bootstrap.servers", brokerList);
//使用上述配置初始化Kafka生产者
KafkaProducer<String, String> producer =
                        new KafkaProducer<>(properties);          processVariables.put(ProcessConstants.PROCESS_COMPLETE_CALLBACK,processDefinitionKey);
//构造Kafka消息
ProducerRecord<String, String> record =
new ProducerRecord<>(ProcessConstants.FLOWABLE_CALLBACK,
                                processVariables.toString());
//消息发送
try {
producer.send(record);
   } catch (Exception e) {
   e.printStackTrace();
}
//关闭Kafka生产者
producer.close();

4.5.4 分布式定时任务
首先在官网下载源码https://github.com/xuxueli/xxl-job，选择RELEASE分支进行下载，本项目使用的版本是2.4.0。加载好项目对应的Maven依赖后，在yaml文件中显式的加上登录调度中心的账号密码
xxl.job.login.username=admin
xxl.job.login.password=123456
然后启动admin模块，进入http://localhost:8080/xxl-job-admin 就可以得到如下所示的执行器管理界面了。
 
继续回到本项目中，引入如下所示的依赖：
<dependency>
			<groupId>com.xuxueli</groupId>
			<artifactId>xxl-job-core</artifactId>
			<version>2.4.0</version>
</dependency>
之后再在需要配置定时任务的方法处加上@XxlJob("deleteFiredJob")注解，就完成了被定时执行的方法。而后在任务管理界面找到需要被定时调用的方法，再配置其Cron表达式就完成了定时任务调度。
 


4.6 本章小结
	本章首先整理了开发过程需要搭建的环境和需要暴露的中间件，为后续的代码接入做准备。而后梳理了系统的整体架构模型，根据功能模块和总体流程来确定他们之间的调用关系。之后完成数据库表设计，存储运行过程中需要持久化的数据信息。最后是依据各个模块，完成关键过程的代码编写。本章侧重考虑了项目的代的实现方法，并给出了整体设计和关键代码的实现。



 
5  系统实现
5.1  系统运行环境
系统的运行时环境包括操作系统、硬件设备、系统配置、运行时依赖项等多个方面，需要进行适当的配置和管理才能保证系统的正常运行，下面对上述几点进行分别说明。
首先在操作系统上需要：Windows10（开发环境）和Ubuntu 20.04.2（中间件的运行环境）。硬件设备方面，由于可以在一台开发机上运行，因此只需要一台能够满足系统要求的计算机即可。
在系统配置上：
1.	JDK8:JDK1.8.0_151作为系统的Java运行环境。
2.	ZooKeeper:zookeeper-3.4.12作为Dubbo框架的注册中心，同时也作为Kafka中间件的broker注册中心，在Ubuntu 20.04.2上运行。
3.	Kafka:kafka_2.11-2.0.0作为系统的消息队列，在Ubuntu 20.04.2上运行。
4.	Xxl-job:Xxl-job-2.2.0作为系统的分布式任务调度中心。
5.	数据库:Mysql-8.0.28作为系统的数据库。
运行时依赖项：
1.	Flowable工作流引擎：作为系统的核心组件，负责处理工作流程的调度和任务的分配。
2.	Dubbo框架：作为系统的分布式服务框架，实现各个模块之间的调用。
3.	SprinBoot框架：作为系统的核心框架，负责管理系统中各个组件和模块之间的依赖关系。
4.	MyBatis框架：作为系统的持久化框架，负责将Java对象映射到关系型数据库中，将运行时数据持久化到磁盘。
5.	Tomcat服务器：作为系统的Web容器，负责运行和管理Web应用程序。
5.2  登录实现
	用户首先进入登录页界，输入自己的用户名和密码，以及验证码之后进入到首页。通过点击首页的侧边菜单栏，用户可以分别进入到系统管理，流程管理和任务管理模块。在系统管理模块，用户可以对系统用户，系统角色，部门信息，岗位信息进行修改操作。在流程管理模块，用户可以进行修改流程定义和表单配置。在任务管理模块用户可以查询和撤销我的流程，可以查看自己的待办任务和已办任务。

5.2  用户管理实现
	在用户管理界面，管理员可以修改用户信息，具体包括用户的岗位、邮箱、角色、部门等信息。同时也可以修改、角色等相关信息。

5.3  流程定义和表单定义实现
	在流程定义页面，用户可以定义流程，也可对现有的流程定义进行发布、挂起、配置表单等行为。在流程设计页面，用户可以通过拖拽流程模块的方式自定义流程实例，并将其部署到流程引擎中。


5.4  用户任务查看
	用户可以在此界面发起流程，查看自己正在进行中和已完成的历史流程，或者审批需要自己进行审批的流程，也可查看之前的审批历史记录。

5.5  钉钉通知实现
	在流程被用户发起、流转到某个审批人、流程完成审批的时候，系统会自动发送卡片消息通知给对应的用户。用户通过点击卡片的动作，可以跳转到对应的流程管理界面。

5.6  定时任务实现
	在应用程序启动后，系统中的定时任务将会自动注册到执行器中去，在任务管理界面配置其定时执行的时间（corn表达式）就可以完成任务的定时调度。

5.7 本章小结
	本章详细描述了系统的运行环境，给出了详细可操作的工作界面，证明了设计思路的可行性，并最终构建了面向用户使用的工作流管理系统。证实了通过Flowable流程引擎作为主要组件，可以方便快速地搭建出一个可定制化，可审计，可追溯的工作流系统。同时也通过Dubbo框架增强了系统的可扩展性，通过Kafka消息队列，方便了外部系统接入流程数据。
