Conventions regarding JVM-based applications in the department will be described here.

# Garbage collection
## Why unify?
## Why G1?
### General purpose, battle-tested
### Low latency
### Prior knowledge/experience
## Tuning

# Garbage collection
All JVMs MUST use the G1 Garbage Collector.

Exceptions to this rule require approval with a clearly defined and well-tested reason to use a different garbage collector.

## Why unify?
Garbage collection is an advanced topic with many nuances. We want to unify to one garbage collector because we want to focus our expertise and experience into a single collector, rather than fragment our knowledge and spread it across multiple collectors (without a good reason to do so). This will allow us to share tips and pitfalls across the department, generally increasing performance/stability while avoiding issues and surprising differences caused by different garbage collectors.

## Why G1?
General purpose, battle-tested
The G1 collector has been continuously improved with new releases of Java and has been deemed general purpose and stable enough to be the default garbage collector from Java 9.

## Low latency
One of G1's stated purposes and known qualities is consistently low GC pauses compared to other collectors. Our applications are generally latency sensitive, so latency caused by GC pauses is important. CPU usage and therefore throughput may be sacrificed to achieve these lower pause times, but we also strive for horizontally scalable applications, which is another reason G1 is a good fit for our JVMs.

## Prior knowledge/experience
Within the department, we have substantial experience running applications with the G1 collector, and we have not had a serious problem caused by the collector. We have knowhow for tuning the garbage collector, if necessary.

# Tuning
Before tuning garbage collection, it is important to understand garbage collection in general and the specific collector your JVM is configured to use.

Here are some resources, with many more available by searching the internet.

[Getting Started with the G1 Garbage Collector](http://www.oracle.com/technetwork/tutorials/tutorials-1876574.html)

[HotSpot JVM Garbage Collection Tuning Guide - Garbage-First Garbage Collector](https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/g1_gc.html)