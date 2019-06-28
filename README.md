System Design Related Basic Concept
===============================
## Core Concerns of System Design
### Reliability  
the ability of a system to tolerate faults or problems in order to prevent failures or complete shutdowns.  
### Scalability  
the system’s ability to deliver reasonable performance in face of increased load. From my points of view, we want to let today's 
system fit later's changes. 
### Maintainability  
means writing code that can easily be understood, refactored and upgraded by someone who is not the original author of the 
code.  

## Performance Optimization Guide
### General principle
#### Based on data
When we suspect that there is a problem with performance, we should use tests, logs, and profillig to analyze where there are 
problems and target them, rather than relying on feelings and crashing luck. A system has performance problems, the bottleneck 
may be CPU, there may be memory, there may be IO (disk IO, network IO), the general direction can be positioned using top and 
stat series (vmstat, iostat, netstat.. .), for a single process, you can use pidstat to analyze.
#### Avoid premature and excessive optimization
- The real problem is that programmers have spent far too much time worrying about efficiency in the wrong places and at the 
wrong times; premature optimization is the root of all evil (or at least most of it) in programming.  
- As performance is part of the specification of a program – a program that is unusably slow is not fit for purpose
#### Deep understanding of work(business needs)
The code is for the end user or other programmers. Without understanding the true needs, it is difficult to understand the 
system's processes, and it is difficult to find out the inadequacies of the system design.
#### Keep iteration
Performance optimization is a long-term battle
### Phases of performance optimization
#### Demand phase
The programmer's work may come from the business needs of the PM, UI (or functional requirements), or from the thoughts of the 
Team Leader. When we get a demand, the first thing we need to do is to think and discuss the rationality of the demand, rather than
designing and coding immediately.

Demand is to solve a problem, the problem is the essence, and the demand is the way to solve the problem.

The premise of the demand discussion is to have a deep understanding of it. Even if the demands have been met, when 
we find that there are performance problems, we still should start with the demands first.  

How does demand analysis help performance optimization? First, in order to achieve the same goal and solve the same problem, 
you may have better performance (less consumption). This kind of optimization is non-destructive, that is, it does not 
change the essence of the demands, but also achieves the effect of performance optimization; the second case, lossy 
optimization, that is, sometimes slightly modifing the demands and easing the conditions can greatly solve the performance 
problem.
#### Design phase
Experts spend 80% of their time thinking, 20% of the time implementing; new hand codes quickly, but behind is endless bug fixes
#### Implementation phase
Implementation is the process of translating functions into code. This level of optimization is mainly for a call process, 
a function, and a piece of code optimization. Various profile tools are also primarily active at this stage.
### General method
#### Cache
a cache is a hardware or software component that stores data so future requests for that data can be served faster; the 
data stored in a cache might be the result of an earlier computation, or the duplicate of data stored elsewhere.
#### Concurrent
Concurrency here refers to generalized concurrency, and the granularity includes multi-machine (cluster), multi-process, 
multi-thread.
#### `Lazy`
Postponing the calculation to the necessary time, it is possible to avoid redundant calculations, or even no calculation 
at all. This is kind of the idea of copy-on-write.
#### Batch, merge
When there is IO (network IO, disk IO), merge operations and batch operations can often improve throughput and improve 
performance.
The most common one is bulk reading: read more each time you read the data, in case you need it. 
Especially when there is a single point in the system, the cache and the batch essentially reduce the interaction with a 
single point, which is a cost-effective way to reduce the pressure of a single point.
When it comes to network requests, the network transmission time may be much longer than the request processing time, so 
it is necessary to merge network requests.

## Performance vs scalability
A service is said to be scalable if when we increase the resources in a system, it results in increased performance in a 
manner proportional to resources added. Increasing performance in general means serving more units of work, but it can also 
be to handle larger units of work, such as when datasets grow.
### Another way to look at performance vs scalability:
- If you have a performance problem, your system is slow for a single user.
- If you have a scalability problem, your system is fast for a single user but slow under heavy load.
### two problems for scalability:
- scalability cannot be an after-thought
- growing a system through scale-out generally results in a system that has to come to terms with heterogeneity

## Latency vs throughput
**Latency** is the time required to perform some action or to produce some result.
**Throughput** is the number of such actions executed or results produced per unit of time.
## Availability vs consistency
CAP theorem: it is impossible to build an implementation of read-write storage in an asynchronous network that satisfies all of the following three properties:  
- Availability - A read is guaranteed to return the most recent write for a given client
- Consistency - A non-failing node will return a reasonable response within a reasonable amount of time (no error or timeout)
- Partition tolerance - The system will continue to function when network partitions occur.  
### The following are some other basic concepts:
- atomic: Atomic, or linearizable, consistency is a guarantee about what values it's ok to return when a client performs get() operations. The idea is that the register appears to all clients as though it ran on just one machine, and responded to operations in the order they arrive.
- asynchronous: An asynchronous network is one in which there is no bound on how long messages may take to be delivered by the network or processed by a machine. The important consequence of this property is that there's no way to distinguish between a machine that has failed, and one whose messages are getting delayed.
- available: A data store is available if and only if all get and set requests eventually return a response that's part of their specification. This does not permit error responses, since a system could be trivially available by always returning an error.
- partition: A partition is when the network fails to deliver some messages to one or more nodes by losing them (not by delaying them - eventual delivery is not a partition).  

Networks and parts of networks go down frequently and unexpectedly.Given that networks aren’t completely reliable, you must tolerate partitions in a distributed system, period.
- CP - Consistency/Partition Tolerance - Wait for a response from the partitioned node which could result in a timeout error. The system can also choose to return an error, depending on the scenario you desire. Choose Consistency over Availability when your business requirements dictate atomic reads and writes.
- AP - Availability/Partition Tolerance - Return the most recent version of the data you have, which could be stale. This system state will also accept writes that can be processed later when the partition is resolved. Choose Availability over Consistency when your business requirements allow for some flexibility around when the data in the system synchronizes. Availability is also a compelling option when the system needs to continue to function in spite of external errors   
## Consistency






Thank you to these authors!  
Reference(not listed in order):  
[Werner Vogels](https://www.allthingsdistributed.com/2006/03/a_word_on_scalability.html)  
[xybaby](https://www.cnblogs.com/xybaby/p/9055734.html)  
[Vaibhav Aparimit](https://hackernoon.com/@v_aparimit)  
[henryr](https://github.com/henryr/cap-faq)  
[Robert Greiner](http://robertgreiner.com/2014/08/cap-theorem-revisited/)
