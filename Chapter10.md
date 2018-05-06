### Chapter10.康威定律和系统设计

1. **康威定律**：任何组织在设计一套系统（广义概念上的系统）时，所交付的设计方案在结构上都与该组织的沟通结构保持一致。（组织架构决定系统架构）。
2. **松耦合组织和紧耦合组织**：紧耦合组织例子是商业产品公司，松耦合组织例子是分布式开源社区。组织的耦合度越低，其创建的系统的模块化就越好，耦合也越低；组织的耦合度越高，其创建的系统的模块化也越差。
3. **服务所有权**：一般来说，它意味着拥有服务的团队负责对该服务进行更改。只要更改不破坏服务的消费者，团队就可以随时重新组织代码。对于许多团队而言，所有权延伸到服务的方方面面，从应用程序的需求，构建，部署到运维。
4. 很多团队还是选择使用**共享服务**的模式，其实这并不提倡。原因在于
   + 难以分割：拆分服务的成本太高。
   + 特性团队：特性团队只专注于一系列特性需要的所有功能开发，会出现横跨多个服务的可能性，这其实是与微服务的出发点相违背。
   + 交付瓶颈：当共享服务的时候，如果某个服务突然出现了大量的变更需求怎么办？有下面几种方式去应对：
     + 等待，功能交集使用串行方式，团队转而开发别的功能，这取决于特性的重要性和延迟的时长是，这可能是个好的主意，但也可能是一个糟糕的想法。
     + 派人到产品目录团队帮助他们更快地工作。你的系统使用越标准化的技术栈和编程范式，就越容易让其他人更改服务。当然，另一方面，如前文所述，标准化会导致团队降低采取正确的解决方案来解决问题的能力，并可能会降低效率。
     + 把产品目录拆分成一般音乐目录和铃声目录两个服务。如果支持铃声的工作量非常小，而我们未来在这一领域工作的可能性也很低，这个选择可能是不成熟的。另一方面，如果铃声相关的功能积累有10周的工作量，拆分出服务，并且让移动团队拥有所有权，可能还是有意义的。
5. 内部开源
   + 守护者的角色
   + 成熟
   + 工具
6. 限界上下文和团队结构
7. 孤儿服务
8. 反向的康威定律：也存在系统设计能改变组织的情况，这一类型的案例都属于历史原因引起的。无论系统有什么设计缺陷，我们都不得不通过改变组织结构来推动系统的更改。许多年后，这个过程仍然在进行中。
9. 人：“不管一开始看起来什么样，它永远是人的问题。”我们必须承认，在微服务环境中，开发人员很难只在自己的小世界中编写代码。他需要意识到像跨网络边界调用及隐式失败等隐式问题。微服务治理，很大程度上就是解决人的问题。