### Chapter8.监控

1. 单一服务，单一服务器

   首先，我们希望监控主机本身。CPU，内存等所有这些主机的数据都有用。我们想知道，系统健康的时候它们应该是什么样子的，这样当它们超出边界值时，就可以发出告警。（如Nagios，New Relic）；

   接下来，我们要查看服务器本身的日志。如果用户报告了一个错误，这些日志应该可以告诉我们，在何时何地发生了这个错误。这个时候，对于单台主机来说，只需要登录到主机上使用命令行工具扫描日志就可以了。我们甚至可以更进一步，使用logrotate 帮助我们移除就得日志，避免日志占满了磁盘空间。

   最后，我们可能还想要监控应用程序本身。最低限度是要监控服务的响应时间。你可以通过查看运行服务的Web服务器，或者服务本身的日志做到这一点。如果我们想更进一步，可能还需要追踪报告中错误出现的次数。

2. 单一服务，多个服务器

   对于主机本身的健康度状况，可以对多个主机的自身数据进行聚合。

   接下来就是日志，我们的服务运行在多个服务器上，登录到每台服务器查看日志，会比较繁琐。如果只有几个主机，我们可以使用像 ssh-multiplexers 这样的工具，在多个主机上运行相同的命令。用一个大的显示屏，和一个grep "Error" app.log，我们就可以定位错误了。

   对于像响应时间这样的监控，我们可以在负载均衡器中进行追踪，很容易就能拿到聚合后的数据。不过负载均衡器本身也需要监控；如果它的行为异常，也会导致问题。对于服务本身的监控，我们可能更关心健康的服务时什么样的，这样当我们配置负载均衡器的时候，就可以从应用程序中移除不健康的节点。

3. 多个服务，多个服务器

   从日志到应用程序指标，集中收集和聚合尽可能多的数据到我们的手上。

4. 日志，日志，更多的日志

   面对服务的增加，日志也会随着而膨胀。我们既要满足业务的日志需求，又能够从大量的日志检索出对应的结果，我们需要用专门获取日志的子系统来代替它，让日志能够集中在一起方便使用。这方面的一个例子是**logstash**，它可以解析多种日志文件格式，并将它们发送给下游系统进行进一步调查。

   **Kibana**是一个基于 ElasticSearch 查看日志的系统。

5. 多个服务的指标跟踪

   在复杂的环境中，我们会频繁地重建服务的新实例，所以我们希望选择一个系统能够方便地从新的主机收集指标。我们希望能够看到整个系统聚合后的指标（比如，平均的CPU负载），但也会想要给定的一些服务实例聚合后的指标，甚至单个服务实例的指标。这意味着，我们需要将指标的元数据关联，用来帮助推导出这样的结构。

   **Graphite**就是一个让上述要求变得很容易的系统。可以了解下，它提供API发送实时指标数据，也允许跨样本做聚合，或深入到某个部分，还有做容量规划等。

6. 服务指标

   二八原则：80%的软件功能从未使用过。

7. 综合监控

   我们可以通过定义正常的CPU级别，或者可接受的响应时间，判断一个服务是否健康。如果我们的监控系统监测到实际值超出这些安全水平，就可以触发警告。类似像Nagios这样的工具。

   然而，在许多方面，这些测量结果离我们真正关心的内容仍有一步之遥，即系统是否正常工作？

   在实践中，我发现使用这样合成事务执行语义监控的行为，比使用低层指标的告警更能表明系统的问题。当然，它们不会取代低层次的指标，那些细节有助于我们了解为什么语义监控会报告问题。

   因此，**实现语义监控**。

8. 关联标识

   为了便于追踪调用的情况，监控下游的调用链情况。一个非常有用的方法是使用关联标识（ID）。在触发第一个调用时，生成一个GUID。然后把它传递给所有的后续调用。而且，类似日志级别和日期，你也可以把关联标识以结构化的方式写入日志。使用合适的日志局和工具，你能够对事件在系统中触发的所有调用进行跟踪。

   像**Zipkin**这样的软件，也可以跨多个系统边界跟踪调用。

9. 级联

   级联故障特别危险。想象这样一个情况，我们的音乐商店网站和产品目录服务之间的网络连接瘫痪了，服务本身是健康的，但它们之间无法交互。如果只查看某个服务的健康状态，我们不会知道已经出问题了。使用合成监控会将问题暴露出来。但为了确定问题的原因，我们需要报告这一事实是一个服务无法访问另一个服务。

   因此，监控系统之间的集点非常关键。每个服务的实例都应该追踪和显示其下游服务的健康状态，从数据库到其他合作服务。你也应该将这些信息汇总，以得到一个整合的画面。你会想了解下游服务调用的响应时间，并检测是否有错误。

10. 标准化

    监控这个领域的标准化是至关重要的。服务之间使用多个接口，以很多不同的方式来合作为用户提供功能，你需要以整体的视角查看系统。

11. 考虑受众

12. 未来

    许多组织正在朝一个完全不同的方向迈进：不再为不同类型的指标提供专门的工具链，而是提供伸缩性很好的更为通用的事件路由系统。这些系统能提供更多的灵活性，同时还能简化我们的架构。比如：**Riemann**和**Suro**。