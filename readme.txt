【20200618】
在20200612基础上，增加了把平均分组/且每组中的Job相邻排列的功能，目的是减少线程处理Job时的cache/DRAM bound。
把Job类型变为三种：
- JOBALLOCATION_INTERLEAVED_JOB		//20200612中的ALL-EVEN，每个线程为了负载均衡，按固定间隔从job queue中取Job
- JOBALLOCATION_SEQUENTIAL_JOB		//新增的类型，每个线程从job queue中取一组连续的job
- JOBALLOCATION_JOB_SHARING		//20200612中的ALL-Stealing，该实现描述为job-sharing比job-stealing更确切。

实验表明：
线程处理连续的Job，比处理非连续的Job，确实memory bound有减少。


【20200612】
在20200610基础上，把主线程改成渲染Job到deferredcontext（而不是原先的immediate context）中。主线程和工作线程都调用同一个BuildCommandList函数。

实验表明：
1.主线程改为渲染到deferredcontext中（和工作线程一样），确实大大减少了主线程等待工作线程的概率。
2.对Job排序的开销比较大，无论是ALL_EVEN模式还是ALL_Stealing模式，FPS都明显下降。其次，即使对ALL_EVEN模式，job排序也没有起到负载均衡的作用，GPUView显示和没排序差不多的负载分配。


【20200610】
在20200606基础上，增加对job的排序，让最大的job先处理，目的是进一步提高多线程负载均衡性，降低主线程等待的概率。
这种方法可避免某个线程处理最后一个大任务而阻塞其他线程。不仅有利于work stealing Job的模式，更有利于静态均分Job的模式。

但即使是stealing模式，还是有可能出现主线程等待工作线程的情况，主要原因是：工作线程最后要执行finish command list操作，这个操作不属于Job，但耗时比Job大很多，而主线程执行完Job后，不需要执行此操作。

参考：
https://www.cnblogs.com/luorende/p/6121906.html
https://blog.csdn.net/zhangpiu/article/details/50564064


【20200606】
在20200602版本的基础上实现此版本，改纯immediate渲染为多线程deferred渲染，避免0602版本的驱动瓶颈问题。用于比较静态均分和动态Stealing模式在竞争平台上的性能。

实现2种模式：
1. 静态分配相同数量JOB给主线程和工作线程。
所有JOB放在同一个队列中，各线程以线程总数为Step从队列中获取Job，实现静态的负载均衡。
2. 主线程和工作线程同时Stealing Job。

JOB包含渲染draw calls。

工作线程数改为物理核数-2。因为主线程是busy的，还要减去驱动线程。

CDXUTSDKMesh::RenderMesh()中保留以下函数。
    if( 0 < GetOutstandingBufferResources() )
        return;

