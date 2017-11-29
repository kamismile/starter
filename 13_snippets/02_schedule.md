```scheme
@startuml

scale 3

package task {

    class TaskResult
    class TaskSet
    class Task

    TaskSet -- Task
    Task <|-- ResultTask
    Task <|-- ShuffleMapTask
}

package scheduler {
    interface TaskScheduler <<interface>> {

    }
    interface TaskSchedulerListener <<interface>>

    class DAGScheduler
    TaskSchedulerListener <|-- DAGScheduler
    TaskScheduler <.. DAGScheduler
    class ActiveJob
    class Stage

    Stage -- DAGScheduler
    ActiveJob - DAGScheduler    
}

package result {
    class JobResult
    class JobSucceeded
    class JobFailed
    interface JobListener
    class JobWaiter

    JobResult <|-- JobSucceeded
    JobResult <|-- JobFailed

    JobListener <|-- JobWaiter
}

DAGScheduler "1" *-- "n" JobResult : contains
DAGScheduler "1" *-- "n" JobListener : contains

package event {
    class DAGSchedulerEvent
    class JobSubmitted
    class CompletionEvent
    class ExecutorLost
    class TaskSetFailed
    class StopDAGScheudler
    JobSubmitted - ExecutorLost
    DAGSchedulerEvent <|-- CompletionEvent
    TaskSetFailed - StopDAGScheudler
    DAGScheduler "1" *-- "n" DAGSchedulerEvent

}
@enduml

```

