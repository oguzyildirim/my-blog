---
title: Handling I/O
date: "2020-04-04"
description: "Handling IO"
---

---

![io](./io.jpg)

We want to  build concurrent and performant software applications and there are new tools to compute-heavy workloads!

I am big fan of Node.js and it is great experience developing applications with Node.js but today other platforms have their own APIs to implement non-blocking IO application with new libraries like Reactive Spring, .Net Core, Vert.x  but the way is same; non-blocking, single treaded and event driven applications.

---

## So, how to take advantage of I/O in software?

>In computing, input/output or I/O (or, informally, io or IO) is the communication between an information processing system, such as a computer, and the outside world, possibly a human or another information processing system. Inputs are the signals or data received by the system and outputs are the signals or data sent from it. The term can also be used as part of an action; to "perform I/O" is to perform an input or output operation.

## How do we handle I/O traditionally

- Lets try to imagine you are using an blocking I/O and building a system and you've got a server listening at your favorite port and your software waiting new connection. Finally a client start to send you some new bytes to your input stream and you start processing request and maybe you need to make some external api calls or querying your database so you  are basically waiting some network calls to happen waiting them, how is that sound? There is nothing the worry about there your software is working, all the network request finally finished and you can now respond to you client, happy end.

- The first scenario was perfect your input and output system doing their jobs and your business logic happened perfectly and your user is happy with your software. After some time your users start to talk about your software saying what an amazing software. One eternity later more people come to use your software and you see your system is blocked because every new user means new request to your software basically your software can't handle throughput. What the heck? Should start to worry about my piece of software that i wrote perfectly? Do i need to refactor some piece in it? No i don't want to do that? Why should i do that? But the day has come your boss says we have got some feedbacks some users can't react out our software. Time to provide a solution, new kid in street Multithreading.

---

## Multithreading

>In computer architecture, multithreading is the ability of a central processing unit (CPU) (or a single core in a multi-core processor) to provide multiple threads of execution concurrently, supported by the operating system. This approach differs from multiprocessing. In a multithreaded application, the threads share the resources of a single or multiple cores, which include the computing units, the CPU caches, and the translation lookaside buffer (TLB).

>Where multiprocessing systems include multiple complete processing units in one or more cores, multithreading aims to increase utilization of a single core by using thread-level parallelism, as well as instruction-level parallelism. As the two techniques are complementary, they are sometimes combined in systems with multiple multithreading CPUs and with CPUs with multiple multithreading cores.


- Lets continue on our story. You got worried a little bit but time to refactor your code to work with this thing called Multithreading and you have a great threads working on your cores. Finally your application ready to run on production. Summer has come again. Users are happy, more importantly your boss is happy. When heavy loads come to your application you schedule your threads, take the advantage of parallelism and you good to go. Your company is growing day by day and you are still happy about your software but winter is coming.

- Tech industry has grown and there are things called cloud native distributed architecture and your application should adopt this architecture because why not? You did your job adopt this thing called Twelve factor cloud native distributed architecture with this architecture you have microservices and your company hyped. We have microservices! But there is something you worry about because you realize your application speak via network calls even when your program starts you need to call an external API to give configuration of your application. Nothing to worry about you can add more threads to your system you would have thousands threads and you can schedule your threads but what? thousands of threads? No way! We didn't talk like that before I can't schedule this threads and every single threads costs me a lot. But when you were designing your microservices you realize that you have stateless RESTful APIs! You can scale your system.

---

## Stateless RESTful APIs

>Statelessness means that every HTTP request happens in complete isolation. When the client makes an HTTP request, it includes all information necessary for the server to fulfill that request. The server never relies on information from previous requests. If that information was important, the client would have sent it again in this request.
Statelessness helps in scaling the APIs to millions of concurrent users by deploying it to multiple servers. Any server can handle any request because there is no session related dependency.

>Being stateless makes REST APIs less complex – by removing all server-side state synchronization logic.
A stateless API is also easy to cache as well. A specific software can decide whether or not to cache the result of an HTTP request just by looking at that one request. There’s no nagging uncertainty that state from a previous request might affect the cacheability of this one. It improves the performance of applications.
The server never loses track of “where” each client is in the application because the client sends all necessary information with each request.

- Lets again continue on our story. We did an amazing job to adopt microservices because why not(that may be another topic to talk in future)? We have distributed services talking each other and tons of network request. It is an old problem isn't it? We have problems with network requests! Remember what we were building stateless APIs so we can take advantage of them. We may create a scale policy right now. Lets assume our system has overload at some period of day assume it 3PM. At 3PM we may horizontally scale our application. What i mean by that?

- We containerize our stateless APIs with docker and we put a tool like k8s  to babysitting our containers and define our policies and load balance our stateless RESTful APIs or you may go with other ways that cloud providers provide. We can do that because our API Gateway or our client that faces public requests can easily forward requests one of our stateless APIs. We build such an amazing system with an great effort but the dark clouds are coming to town again! Horizontally scaling costs a lot to company and the users keep coming your boss saying Gimli quotes `Let Them Come. There Is One Dwarf Yet In Moria Who Still Draws Breath.`. We can do better than right? It is time to rethink our I/O mechanism but today is friday so it is time to party that thing called twelwe factor cloud native microservices makes us tired we need to rest.

---

![StarWars async meme](./asynchrony.jpeg)

---

## Non-Blocking

>In computer science, asynchronous I/O (also non-sequential I/O) is a form of input/output processing that permits other processing to continue before the transmission has finished.
Input and output (I/O) operations on a computer can be extremely slow compared to the processing of data. An I/O device can incorporate mechanical devices that must physically move, such as a hard drive seeking a track to read or write; this is often orders of magnitude slower than the switching of electric current. For example, during a disk operation that takes ten milliseconds to perform, a processor that is clocked at one gigahertz could have performed ten million instruction-processing cycles.

>A simple approach to I/O would be to start the access and then wait for it to complete. But such an approach (called synchronous I/O, or blocking I/O) would block the progress of a program while the communication is in progress, leaving system resources idle. When a program makes many I/O operations (such as a program mainly or largely dependent on user input), this means that the processor can spend almost all of its time idle waiting for I/O operations to complete.

>Alternatively, it is possible to start the communication and then perform processing that does not require that the I/O be completed. This approach is called asynchronous input/output. Any task that depends on the I/O having completed (this includes both using the input values and critical operations that claim to assure that a write operation has been completed) still needs to wait for the I/O operation to complete, and thus is still blocked, but other processing that does not have a dependency on the I/O operation can continue.

- After a crazy friday night with your friends you need to go rest a bit. You wake up saturday morning, take a cup of coffee and start reading my blog and you notice there is a topic about something called handling I/O. Last thing you remember our system doesn't handle anything anymore, you goofed up because your system waste so much time between network requests waiting I/O to respond and your I/O system sitting there like a useless piece of software and waiting  endless roads to come end of the road and this makes your application inefficient because you are waiting this endless roads to respond you something and finally roads come to an end your thread able to finish his job.
- You simply blocked your thread to wait the network request and multithreading is not a solution anymore because context switching costs a lot and all threads add additional resource. You can't have unlimited number of threads you can do better than that it is time to break to walls. It is time to introduce something new! Actually the idea is simple instead of scaling your application horizontally or adding new threads. You just need to run your application on a single thread and make the I/O calls asynchronous, what i mean by that is make event driven HTTP servers. How is that sound now? Because remember network request are endless roads you experience it and you don't want to wait this roads to finish to do your other tasks. Basically you create  a thing called callback and when the road finished you got an signal saying you  **HEYYYYYY!!**, your request is here and you can proceed. Basically minimizing the time you spend I/O makes you able to handle throughput, scale your application.

- Every new concept we introduce to improve throughput, latency, and/or responsiveness of your application and also add complexity but with this way you don't have to deal with synchronization between threads or you don't need to scale your application horizontally because you have limited resources but that doesn't means statelessness is a bad thing but efficient way to solve your problems.

---

- Modern software design increasingly revolves around using asynchronous programming and modern languages has great APIs to allow you to program to do more than one thing at a time.
**Go** (has Goroutines it is perfect way to deal with threads i believe but the binary program runs on single thread) **Javascript** single treaded language and has the nature of event driven architecture and **Kotlin** has Coroutines.
- Big players has introduced new APIs as we talked before; **[.Net Core](https://docs.microsoft.com/en-us/dotnet/core/), **[SpringWebFlux](https://docs.spring.io/spring/docs/current/spring-framework-reference/web-reactive.html), **[Vert.x](https://vertx.io/)** and many others and it is worth to mention  the java community added to NIO module to **[J2SE 1.4](https://www.jcp.org/en/jsr/detail?id=51)** already back in 2002 and **[Netty](https://netty.io/)** is actively developed since 2004 so we can't say this is an new concept but with no regret nowadays it is popular and perfect way to handle IO. There are benchmark test you can found online comparison of these APIs.

- The idea is simple DO NOT block the IO so we have APIs for developing event driven HTTP servers.

-Today we have talked about I/O. Now it is time to tests these perfect APIs.

### Oguz

---
## Useful links

- **[Node js overview of Blocking vs Non-Blocking](https://nodejs.org/en/docs/guides/blocking-vs-non-blocking/)**

- **[Reactive Spring By Josh Long](https://www.youtube.com/watch?v=1F10gr2pbvQ&t=485s)**

- **[Async Kotlin](https://www.youtube.com/watch?v=hb0hfHVWCS0)**

- **[General asynchronous programming concepts](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Concepts)**


---


