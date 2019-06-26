System Design Related Knowledge
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

Another way to look at performance vs scalability:
- If you have a performance problem, your system is slow for a single user.
- If you have a scalability problem, your system is fast for a single user but slow under heavy load.

## Latency vs throughput
**Latency** is the time to perform some action or to produce some result.
**Throughput** is the number of such actions or results per unit of time.




Thank you to these authors!  
Reference(not listed in order):  
[Werner Vogels](https://www.allthingsdistributed.com/2006/03/a_word_on_scalability.html)  
[xybaby](https://www.cnblogs.com/xybaby/p/9055734.html)  
[Vaibhav Aparimit](https://hackernoon.com/@v_aparimit)
