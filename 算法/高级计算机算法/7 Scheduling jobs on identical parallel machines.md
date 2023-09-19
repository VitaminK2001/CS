Suppose that there are n jobs to be processed, and there are m identical machines (running in parallel) to which each job may be assigned.

Each job j = 1, . . . , n, must be processed on one of these machines for $p_j$ time units without interruption, and each job is available for processing at time 0.

Each machine can process at most one job at a time. The aim is to complete all jobs as soon as possible.that is, if job j completes at a time Cj(presuming that the schedule starts at time 0), then we wish to minimize $C_{max}= max_{j=1,...,n}C_j$, which is often called the _**makespan**_ or length of the schedule.

# Local Search

Local search algorithms are defined by a set of local changes or local moves that change one feasible solution to another.

Start with any schedule;

consider the job _l_ that finishes last;

check whether or not there exists a machine to which it can be reassigned that would cause this job to finish earlier.

If so, transfer job l to this other machine.

We can determine whether to transfer job _l_ by checking if there exists a machine that finishes its currently assigned jobs earlier than _Cl−pl_.

The local search algorithm repeats this procedure until the last job to complete cannot be transferred.

$C^∗_{max}$ ： length of the optimal schedule

existing bounds

$C^∗_{max}≥ max_{j=1,...,n}p_j$ 至少要大于等于任务时间最长的任务时间

$C^∗_{max}≥\sum^n_{j=1}p_j/m$ 至少存在一台机器完成任务的时间是大于等于平均时间的

Let _l_ be a job that completes last in this final schedule; the completion time of job _l_, _Cl_, is equal to this solution’s objective function value.

By the fact that the algorithm terminated with this schedule, every other machine must be busy from time 0 until the start of job _l_ at time _Sl = Cl−pl._

we know that each machine is busy processing jobs throughout this period. The total amount of work being processed in this interval is m*Sl*, which is clearly no more than the total work to be done,$\sum^n_{j=1}p_j$.

Hence, $S_l≤\sum^n_{j=1}p_j/m.$

we see that $S_l \leq C^∗_{max}$.

But now, we see that the length of the schedule before the start of job _l_ is at most $C^∗_{max}$, （Sl的原因）as is the length of the schedule afterward（pj的原因）; in total, the makespan of the schedule computed is at most 2 $C^∗_{max}$.

## the running time of this algorithm

Let $C_{min}$ be the completion time of a machine that completes all its processing the earliest.

One consequence of focusing on the new variant is that $C_{min}$ never decreases.

We argue next that this implies that we never transfer a job twice.

Suppose this claim is not true, and consider the first time that a job j is transferred twice, say, from machine i to i′, and later then to i∗.

When job j is reassigned from machine i to machine i′, it then starts at $C_{min}$ for the current schedule.

Similarly, when job j is reassigned from machine i′to i∗, it then starts at the current $C′_{min}$

Furthermore, no change occurred to the schedule on machine i′in between these two moves for job j. Hence, $C_{min}$must be strictly smaller than $C_{min}$ (in order for the transfer to be an improving move), but this contradicts our claim that the $C_{min}$ value is non-decreasing over the iterations of the local search algorithm.

Hence, no job is transferred twice, and after at most n iterations, the algorithm must terminate. We have thus shown the following theorem.

## approximation rate
Sl表示的是执行最后一个任务l之前所花费的时间
因为一共有m台机器，所以m * Sl ≤ 除l外的任务时间和
总时长是(1-1/m)* pl + 平均时长
平均时长≤ C* max 
pl ≤ C* max
所以approximationrate = (2-1/m)
![[截屏2022-11-25 17.14.54.png]]
