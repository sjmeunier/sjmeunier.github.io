---
layout: post
title:  "The Hardest Things in Software Development"
date:   2019-06-06 18:00:00
categories: Programming
tags: [programming]
---

Software development is not an easy profession, with many aspects which can be really challenging, but often the most difficult things to get right are not difficult abstract complex algorithms, but rather, it's often the seemingly easy stuff that gets you.

So here is my list of things that are deceptively hard in programming.

#### Caching
For web development in general, caching is vital for applications to keep running, especially for high load applications.

In principal caching is not that hard. Backend requests should have a caching layer on top, and within the code itself, it is often advisable to cache frequently used data in caches, such as Redis, instead of having to fetch it each time from slower sources, such as databases.

However, the complexity comes in with how much to cache, and for how long. If you don't cache enough you can get performance problems, but if you cache too aggressively, you can end up with stale data, which causes a whole different set of problems.

Finding the correct balance in caching is never trivial.

#### Threading
Multithreaded applications have been around for decades, and like caching, the principals of threading is something that any decent programmer will know how to do, but it does have a few subtle gotchas.

The biggest challenge with threading is finding the optimum number of threads to spawn. For a given server size, if your application is running too few threads, then it is not using the machines full potential, but conversely, too many threads may use 100% of resources, but may have much more overhead managing the threads and have many processes that are blocking.

With each additional running thread, the overhead increases, and once full resource utilization is reached, that starts to become a big problem. 

The trick is to find the right amount of threads to get the best performance, but the difficulty here is that every application is different. The number of ideal threads can vary based on what the threads are doing. If the threads spend a lot of time waiting on external resources, you can get away with more threads, while if threads are doing very resource intensive work, then fewer threads are ideal. This is really the hard part of threading.

#### Scaling
In the modern era of cloud-based applications, scaling has become very important. You need to have sufficient instances of your application to meet the demand of requests. Scaling applications is easy on platforms such as Azure or AWS, but once again, finding the right sweet spot makes it hard - if you run too many instances, you are wasting money running servers you don't need, while if you underestimate your needs, your applications starts failing (essentially suffering something similar to a DDOS attack). 

The complexity does not end there though. Not all resources are equally scalable. A website could be scaled easily, but that may depend on resources such as a Redis cache, or a SQL database, that may not be so easily scalable, and these will introduce hard limits in how far you can scale. Solving these issues may require considerable redesigns, if they become a problem. Maybe you need a more scalable database, such as a NoSql database instead of a SQL database, or optimising caching, as discussed above. 

The hardest part of all, is that the correct solution for scaling is completely dependant on the particular application, and thus there are no generic one-size-fits-all answers to solving scaling issues.

#### Naming
Ask any programmer what the hardest thing in programming is and this answer will invariably come up. Naming methods and classes well is not always easy, and make the code easier to read, but is really only on this list for fun. This entry has no impact on performance, like the points above, and won't casue anyone to wake up in the middle of the night with a cold sweat.
