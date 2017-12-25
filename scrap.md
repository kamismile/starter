

https://stackedit.io/editor

RDDOperationGraph
RDDOperationGraphListener



rdd是数据也是任务
operation在调度里面是没有意义的，仅是关联，也可以看作是map



1) sparkcontext 初始化过程
2) RDD提交过程生成graph SparkListener
	ParallelCollectionRDD
	RDDOperationScope ???
	RDDCheckPointData ???


​	
	map

	filter

​	
	reduce() {
		runJob()
	}


​	
DAGScheduler.runJob()时机是类reduce操作	
submitJob()

TaskContext

JobListener


RootJob
	.nextJob()
	.nextJob()
	.finishJob()
		SparkContext.submitJob()
			JobWaiter()
			JobSubmitted() -> post eventProcessLoop

DAGSchedulerEventProcessLoop		


Dependency
NarrowDependency

​	


ListenerBus 和DAGSchedulerEventProcessLoop联系


metricsource