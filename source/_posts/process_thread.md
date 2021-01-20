---
title: A gentle introduction to multithreading
date: 2019-01-13 19:19:18
tags:
- multithreading
- learning note
---
[线程知识介绍](https://www.internalpointers.com/post/gentle-introduction-multithreading)

介绍了process(进程)与thread(线程)
概要：（稍后汉译加深理解）
### Processes and threads: naming things the right way
Modern operating systems can run multiple programs at the same time. That's why you can read this article in your browser (a program)（进程） while listening to music on your media player (another program). Each program is known as a process that is being executed. The operating system knows many software tricks to make a process run along with others, as well as taking advantage from the underlying hardware. Either way, the final outcome is that you sense all your programs to be running simultaneously.

Running processes in an operating system is not the only way to perform several operations at the same time. Each process is able to run simultaneous sub-tasks within itself, called **threads**（线程）. You can think of a thread as a slice of the process itself. Every process triggers at least one thread on startup, which is called the main thread. Then, according to the program/programmer's needs, additional threads may be started or terminated. **Multithreading**（多线程） is about running multiple threads withing a single process.

For example, it is likely that your media player runs multiple threads: one for rendering the interface — this is usually the main thread, another one for playing the music and so on.

You can think of the operating system as a container that holds multiple processes, where each process is a container that holds multiple threads. In this article I will focus on threads only, but the whole topic is fascinating and deserves more in-depth analysis in the future.

![1. Operating systems can be seen as a box that contains processes, which in turn contain one or more threads.](https://raw.githubusercontent.com/monocasual/internalpointers-files/master/2019/02/processes-threads.png)

### The differences between processes and threads
Each process has its own chunk of memory assigned by the operating system. By default that memory cannot be shared with other processes: your browser has no access to the memory assigned to your media player and vice versa[/ˌvaɪsə ˈvə..sə/ adv. 反过来也一样]. The same thing happens if you run two instances of the same process, that is if you launch your browser twice. The operating system treats each instance as a new process with its own separate portion of memory assigned. So, by default, **two or more processes have no way to share data**, unless they perform advanced tricks — the so-called [inter-process communication (IPC).]
(https://en.wikipedia.org/wiki/Inter-process_communication)
Unlike processes, **threads share the same chunk of memory** assigned to their parent process by the operating system: data in the media player main interface can be easily accessed by the audio engine and vice versa. Therefore is easier for two threads to talk to eachother. On top of that(最重要的是), **threads are usually lighter than a process**: they take less resources and are faster to create, that's why they are also called *lightweight processes.*

Threads are a handy(便利的) way to make your program perform multiple operations at the same time. Without threads you would have to write one program per task, run them as processes and synchronize them through the operating system. This would be more difficult (IPC is tricky) and slower (processes are heavier than threads).

### Green threads, of fibers
Threads mentioned so far are an operating system thing: a process that wants to fire a new thread has to talk to the operating system. Not every platform natively support threads, though. Green threads, also known as fibers are a kind of emulation that makes multithreaded programs work in environments that don't provide that capability. For example a virtual machine might implement green threads in case the underlying operating system doesn't have native thread support.

Green threads are faster to create and to manage because they completely bypass the operating system, but also have disadvantages. I will write about such topic in one of the next episodes.

The name "green threads" refers to the Green Team at Sun Microsystem that designed the original Java thread library in the 90s. Today Java no longer makes use of green threads: they switched to native ones back in 2000. Some other programming languages — Go, Haskell or Rust to name a few — implement equivalents of green threads instead of native ones.

### What threads are used for
Why should a process employ multiple threads? As I mentioned before, doing things in parallel greatly speed up things. Say you are about to render a movie in your movie editor. The editor could be smart enough to spread the rendering operation across multiple threads, where each thread processes a chunk of the final movie. So if with one thread the task would take, say, one hour, with two threads it would take 30 minutes; with four threads 15 minutes, and so on.

Is it really that simple? There are three important points to consider:

1. not every program needs to be multithreaded. If your app performs sequential operations or often waits on the user to do something, multithreading might not be that beneficial;
2. you just don't throw more threads to an application to make it run faster: each sub-task has to be thought and designed carefully to perform parallel operations;
3. it is not 100% guaranteed that threads will perform their operations truly in parallel, that is at the same time: it really depends on the underlying hardware.
The last one is crucial: if your computer doesn't support multiple operations at the same time, the operating system has to fake them. We will see how in a minute. For now let's think of concurrency as the perception of having tasks that run at the same time, while true parallelism as tasks that literally run at the same time.