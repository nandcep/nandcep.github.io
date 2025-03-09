---
title: Reactive System in a Nutshell
date: 2022-03-02 00:00:01
categories: []
tags: [reactive-programming,cloud,multithreading,software development,technology]     # TAG names should always be lowercase
---

Original post on [Life at Telkomsel](https://medium.com/life-at-telkomsel/reactive-system-in-a-nutshell-dd263889fe61).

![](https://images.unsplash.com/photo-1625315714730-d0830cd368bd?q=80&w=3132&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D)
Credit to Unsplash

At modern era computer hardware becomes cheaper and easy to obtain, especially when cloud computing was booming. But even the infrastructure is getting cheaper, at the wholehearted most of organization are still thinking about minimum costs with higher deliverables. And later will be reflected on the environment they are choosing.

Many of traditional enterprise software depends on a great specification of hardware. Let’s say the number of CPU core, the amount of memory (RAM), and network bandwidth. Those utilizations well known as **multi-thread** and a sign of **distributed system**.

A distributed system usually deployed on a several physical or virtual machines with a complex architecture to achieve high availability criteria. Operate in a long runway, over the years.

> Many of modern infrastructure in the clouds are offering cheap hardware such as ARM processor and limited size of RAM, so how to deal with the world of modern hardware with their own legacy system?

At this article, **modernization** is the keyword.

# Thread Bottleneck

Thread in term of software engineering is a component that usually used to do multi task (of course it should be an asynchronous). But it is very expensive and not wise to spawn a new thread for every task with limited number of core.

Could we imagine if we are using a mid-end cloud environment that will handle so many task at the same priority? It will lead into alert, thread exception, or errors because the usage of the resource is too high. As a developer they exactly know that every thread that been activated will also consume and carry out the memory.

The common scenario to describe the situation of multi-thread deadlock is when a queue of restaurant customers is splitting into multiple batches (thread), the customer is waiting for the order and will not sit until their order is fulfilled. Even the batches are splitting to ease the queue, but what the actual needs is a great resource and at this scenario the processor is the restaurant staff.

> A great staff means a bigger resource to hold on, technically it can be a processor and RAM.

![](https://images.unsplash.com/photo-1605776988089-105148e14767?q=80&w=2940&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D)
Photo by shun idota on Unsplash

# The Reactive Manifesto

In the mid of 2014, some of agile engineer around the world are spending their effort to build a manifesto called Reactive Manifesto. Reactive is a paradigm that provides an efficient mechanism. The design itself approach to use asynchronous programming logic to handle real-time adjustments to typically static information.

Reactive can also can handle traditional sequential process if needed. The data that will be proceed between components is a message called as stream, not a bundle of an object. As the manifesto said.

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*hwlFyYj9nIqVPdM-lqNtdQ.png)
Criteria of Reactive

Reactive is about non-blocking, event-driven applications that scale with a small number of threads without feels to worry lost the result due have a recurring task check (_backpressure_) as a key ingredient that aims to ensure producers are not overwhelming the consumers so it has _elastic_ and _resilient_ criteria.

# Reactive as Solution

Systems built as Reactive Systems are more flexible, loosely-coupled and scalable so it has _maintainable, responsive, and extensible_ criteria. The most important for being reactive is **non-blocking** and it is not only mean to be asynchronous. Traditional enterprise system relied on the number of hardware threads.

For the advance usage usually the request from client is halted in a queue called **thread pool**. The painful fact of this mechanism, actually the process _is not so concurrent — not so async_ because the thread should be freed first and will be locked to do another request. This mechanism can be specified as blocking even each request handled by different thread.

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*YukRI0yXppMMEnG4uA5qXw.png)
Multiple Threads still a Blocking from [Quarkus](https://quarkus.io/guides/getting-started-reactive)

TL;DR; we do not have to be confused by how reactive can be a non-blocking, asynchronous, and become more efficient if compared to multi-thread. Reactive systems depends on IO for signaling process in message driven, rather than put the operation to be queued in a multiple thread. In a modern age by default our operating system could utilize IO stack (storage, network, processor, database, etc) that provided by its kernel.

Even our apps is running in a single core processor, by using reactive mechanism the hardware itself is still powerful enough to handle a lot of tasks that stored as an event. In contrast with the first scenario, customer can sit to do the another things after put an order and react on their order after get the result.

So it is just like the client acting as a busy consumer who order then the IO as a producer who will address the client’s request and notify the result as a callback. Thanks for the evolving computation architecture that become more efficient in every decade.

![](https://miro.medium.com/v2/resize:fit:1276/format:webp/1*Te1HNFdDpgCn9ImQivtwRA.jpeg)
https://stackoverflow.com/questions/32373645/node-js-non-blocking-io-vs-java-thread-pool-pattern-using-nio-unclear-scheduli

> Most non-blocking frameworks use an infinite loop that constantly checks (polls) if data is returned from IO. This is often called as **event loop**.

# Conclusion

In the real world, one of the programming language that has a mature reactive manner by default is Javascript (powered by NodeJS). NodeJS have influenced another programming language to adopt reactive mechanism to optimize the event loop. There are so many framework and tools that could assists developer to build a reactive system such as:
- Vert.X
- SmallRye Mutiny
- Spring Project Reactor

And for the another language could extend into reactive using ReactiveX. We could use it to modernize our legacy system. Reactive **does not restrict us to only work with a single thread**.

Being reactive is to lead the optimization of the whole things in computation component (thread, IO, memory, etc) that will be orchestrated as an event. So that is still legal to use multi-thread. At this modern age being reactive is relevant.

Just like what Netflix has done, it able to optimize our system without waste the hardware resource at minimum level even the hardware itself is easy to obtain. So can we imagine that we have a god specification of hardware and running in reactive way?

Maybe it can gain many benefits to boost the performance.

Reference(s):
- [https://www.oreilly.com/library/view/effective-multi-tenant-distributed/9781492042839/ch04.html](https://www.oreilly.com/library/view/effective-multi-tenant-distributed/9781492042839/ch04.html)
- [https://www.reactivemanifesto.org/](https://www.reactivemanifesto.org/)
- [https://reactivex.io/](https://reactivex.io/)
[https://quarkus.io/guides/getting-started-reactive](https://quarkus.io/guides/getting-started-reactive)
- [https://nodejs.org/en/docs/guides/blocking-vs-non-blocking/](https://nodejs.org/en/docs/guides/blocking-vs-non-blocking/)
- [https://netflixtechblog.com/](https://netflixtechblog.com/zuul-2-the-netflix-journey-to-asynchronous-non-blocking-systems-45947377fb5c)