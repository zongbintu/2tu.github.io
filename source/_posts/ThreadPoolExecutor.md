title: ThreadPoolExecutor学习
tags: [ThreadPoolExecutor]
date: 2023-04-07 20:24:22
categories: Java
---

- 创建线程的几种方式
 - Thread
 - Runnable
 - Callable（配合Future获取返回值）
 - 线程池
 	- Executors
 	- ThreadPoolExecutor
- 线程池ThreadPoolExcecutor的7个参数
``` java
    /**
     * Creates a new {@code ThreadPoolExecutor} with the given initial
     * parameters.
     *
     * @param corePoolSize the number of threads to keep in the pool, even
     *        if they are idle, unless {@code allowCoreThreadTimeOut} is set
     * @param maximumPoolSize the maximum number of threads to allow in the
     *        pool
     * @param keepAliveTime when the number of threads is greater than
     *        the core, this is the maximum time that excess idle threads
     *        will wait for new tasks before terminating.
     * @param unit the time unit for the {@code keepAliveTime} argument
     * @param workQueue the queue to use for holding tasks before they are
     *        executed.  This queue will hold only the {@code Runnable}
     *        tasks submitted by the {@code execute} method.
     * @param threadFactory the factory to use when the executor
     *        creates a new thread
     * @param handler the handler to use when execution is blocked
     *        because the thread bounds and queue capacities are reached
     * @throws IllegalArgumentException if one of the following holds:<br>
     *         {@code corePoolSize < 0}<br>
     *         {@code keepAliveTime < 0}<br>
     *         {@code maximumPoolSize <= 0}<br>
     *         {@code maximumPoolSize < corePoolSize}
     * @throws NullPointerException if {@code workQueue}
     *         or {@code threadFactory} or {@code handler} is null
     */
    public ThreadPoolExecutor(int corePoolSize,
                              int maximumPoolSize,
                              long keepAliveTime,
                              TimeUnit unit,
                              BlockingQueue<Runnable> workQueue,
                              ThreadFactory threadFactory,
                              RejectedExecutionHandler handler)
```
	- corePoolSize（核心线程数）
	- maximumPoolSize（最大线程数）
	- keepAliveTime、unit（最大空闲时间）
	- workQueue（阻塞队列）
	- threadFactory（线程工厂，为线程自定义名称）
	- handler（拒绝策略）
- 线程池执行流程
<!-- more -->
![ThreadPoolExcutor Flow](ThreadPoolExcutor_flow.png)
- 线程池属性标识
``` java
    /**
     * The main pool control state, ctl, is an atomic integer packing
     * two conceptual fields
     *   workerCount, indicating the effective number of threads
     *   runState,    indicating whether running, shutting down etc
     *
     * In order to pack them into one int, we limit workerCount to
     * (2^29)-1 (about 500 million) threads rather than (2^31)-1 (2
     * billion) otherwise representable. If this is ever an issue in
     * the future, the variable can be changed to be an AtomicLong,
     * and the shift/mask constants below adjusted. But until the need
     * arises, this code is a bit faster and simpler using an int.
     *
     * The workerCount is the number of workers that have been
     * permitted to start and not permitted to stop.  The value may be
     * transiently different from the actual number of live threads,
     * for example when a ThreadFactory fails to create a thread when
     * asked, and when exiting threads are still performing
     * bookkeeping before terminating. The user-visible pool size is
     * reported as the current size of the workers set.
     *
     * The runState provides the main lifecycle control, taking on values:
     *
     *   RUNNING:  Accept new tasks and process queued tasks
     *   SHUTDOWN: Don't accept new tasks, but process queued tasks
     *   STOP:     Don't accept new tasks, don't process queued tasks,
     *             and interrupt in-progress tasks
     *   TIDYING:  All tasks have terminated, workerCount is zero,
     *             the thread transitioning to state TIDYING
     *             will run the terminated() hook method
     *   TERMINATED: terminated() has completed
     *
     * The numerical order among these values matters, to allow
     * ordered comparisons. The runState monotonically increases over
     * time, but need not hit each state. The transitions are:
     *
     * RUNNING -> SHUTDOWN
     *    On invocation of shutdown(), perhaps implicitly in finalize()
     * (RUNNING or SHUTDOWN) -> STOP
     *    On invocation of shutdownNow()
     * SHUTDOWN -> TIDYING
     *    When both queue and pool are empty
     * STOP -> TIDYING
     *    When pool is empty
     * TIDYING -> TERMINATED
     *    When the terminated() hook method has completed
     *
     * Threads waiting in awaitTermination() will return when the
     * state reaches TERMINATED.
     *
     * Detecting the transition from SHUTDOWN to TIDYING is less
     * straightforward than you'd like because the queue may become
     * empty after non-empty and vice versa during SHUTDOWN state, but
     * we can only terminate if, after seeing that it is empty, we see
     * that workerCount is 0 (which sometimes entails a recheck -- see
     * below).
     */
     // 1.高3位：声明当前线程池状态  2.低29位：声明线程池中的线程个数
    private final AtomicInteger ctl = new AtomicInteger(ctlOf(RUNNING, 0));
    // 29，方便后面做位运算
    private static final int COUNT_BITS = Integer.SIZE - 3;
    // 通过位运算得出最大容量
    private static final int CAPACITY   = (1 << COUNT_BITS) - 1;

	// 线程池状态
    // runState is stored in the high-order bits
    // 111 正常接收任务
    private static final int RUNNING    = -1 << COUNT_BITS;
    // 000 不接收新任务，仍会处理阻塞队列中的任务，正在进行中的任务也正常进行
    private static final int SHUTDOWN   =  0 << COUNT_BITS;
    // 001 不接收新任务，不处理阻塞队列中的任务，中断正在进行中的任务
    private static final int STOP       =  1 << COUNT_BITS;
    // 010 过渡状态，当前线程池即将结束
    private static final int TIDYING    =  2 << COUNT_BITS;
    // 011 结束
    private static final int TERMINATED =  3 << COUNT_BITS;

    // Packing and unpacking ctl
    // 获取线程池状态
    private static int runStateOf(int c)     { return c & ~CAPACITY; }
    // 获取线程池的线程数量
    private static int workerCountOf(int c)  { return c & CAPACITY; }
    private static int ctlOf(int rs, int wc) { return rs | wc; }
```



遗留
IO密集型、CPU密集型如何设置？


