# Overview
## Embedded system
- Computing system with tightly coupled hardware and software integration that are designed to perform a delicated function
## Characteristic of a embedded systems
1. Dependability
2. Efficiency
3. Cross-platform development

## Realtime embedded systems
- Hard realtime systems
	- Must meet deadlines with a non-zero degree of flexibility
	- Missing dealines derives catastrophes
- Soft realtime systems
	- Must meet deadlines but with a degree of flexibility
	- Missing deadline decreases the value of the computed result

# Realtime Operating system (RTOS)

## Definition
- RTOS kernel components: 
	- Scheduler: provides a set of scheduling algorithms to determine which and when task executes (preemptive or round robin)
	- Objects: special kernel constructs to help developers create embedded applications
	- Services: kernel operations on objects
### Scheduler 
- Provides the algorithms needed to determine which task executes.
- Issues on scheduler: 
	- Scheduling entities
	- Multitasking
	- Context switching 
	- Dispatcher
	- Scheduling algorithms
- Schedulable entities 
	- Kernel objects that can complete for execution time based on a predefined scheduling algorithm
- Task
	- Independent thread of execution that contain a sequence of independently schedulable instructions
	- Focus task
### Multitasking 
- Multitasking: kernel's ability to handle multiple activities within deadlines
- Multitasking scenario:
	- The kernel interleaves executions of multiple tasks sequentially based on a preset scheduling algorithms.
	- The kernel should ensure that all tasks meet their deadlines.
	- The tasks follows the kernel's scheduling algorithms, while interrupt service routines are triggered to run because of HW interrupts and their established priorities.
	- Higher CPU performance is requried as the number of tasks increase
	--> frequent context switching
### Context switching
- Each task has its own context
- A context switch occur when a scheduler switches from one task to another.
- Frequent context switching causes unnecessary performace overhead.
### The dispatcher
- The dispatcher: 
	- A part of scheduler that performs context switching and changes the flow of execution.
	- At any time an RTOS is running, the flow of execution, is passing through one of three areas: 
		- An application task
		- An ISR
		- The kernel
	-> Scheduler decides when and how to execute context switching 

**Scheduler vs dispatcher**
- Dispatcher 
	- Low level mechanism
	- Responsibility: context switching
- Scheduler
	- High level policy
	- Responsibility: deciding which task to run

## RTOS key characteristic
- Reliablity
- Predictability
- Performace
- Compactness
- Scalability

*********

# Task and task synchronization

## Definition of a task
- Independent thread of execution that can complete with concurrent task for processor execution time.
- Elements of task: 
	- Unique ID
	- Task control block
	- Stack
	- Priority
	- Task routine
- Task state:
	- *Ready state*: the task is ready to run but cannot because a higher priority task is executing
	- *Running state*: the task is the highest priority task and is running
	- *Blocked*: the task hash requested a resource that is not available, has requested to wait until some event occurs, or has delayed itself for some duration
- Typical task structures:
	- Run-to-completion task: Useful for initialization & startup tasks.
	- Endless-loop task: Work in an application by handling inputs & outputs

## Semaphores
- In multitask systems, concurrently-running tasks should be able to: 
	- Synchronize their execution, and
	- To coordinate mutual exclusive access to shared resource
- Semaphore: 
	- A kernel object to realize synchronization & mutual exclusion
	- One or more threads of execution can acquire & release to execute an operation to be synchronized or to access to a shared resource.
- Element of semaphores:
	- Semaphore control block (SCB)
	- Semaphore ID (unique in system)
	- Value
	- Task-waiting list
- Classification of semaphores:
	- Binary semaphore
	- Counting semaphore
	- Mutex
- Semaphore are used for:
	- Synchronizing execution of tasks
	- Coordinating access to a shared resource
- Synchronization design requirements:
	- Wait-and-signal
	- Multiple-task wait-and-signal
	- Credit-tracking
	- Single shared-resource-access
	- Recursive shared-resource-access
	- Multiple shared-resource access


# Basic concept of scheduling

## Preemption 
- The running task can be interrupted at any point, so that a more important task that arrives can immediately gain the processor
- The to-be-preempted task is interrupted and inserted to the ready queue, while CPU is assigned to the most important ready task which just arrived.
- Why preemption ? 
	- Exception handling of a task
	- Treating with different criticalities of tasks, permits to anticipate the execution of the most critical activities.
	- Efficient scheduling to improve system responsiveness.

**Type of constraints**:
- Timing constraints
	- Constraint on execution time, is the time that must be meet in order to achieve the desired behavior.
	- Typical constraint: *deadline*
		- Relative deadline: deadline is specified with respect to the arrival time.
		- Absolute deadline: deadline is specified with respect to time zero.
- Classification of real-time task:
	- Hard: completion after deadline can cause catastrophic consequence
	- Soft: missing deadline decreases the performance of the system but does not jeopardize its correct behavior.

**Task parameters**: 
- Other task parameter: 
	- *Response time*: different between the finish time and the request time: R_i = f_i - r_i
	- *Ciriticality*: Hard or Soft
	- *Value* v_i: relative importance of task with respect to the other tasks
	- *Lateness*: the delay of a task completion with respect to its deadlines L_i = f_i - d_i
	- *Tardiness* or *exceeding time*: E_i = max(0,L_i) is the time a task stay active after its deadline.
	- *Laxity* or *Slack time* X_i = d_i - a_i - C_i is the maximum time a task can be delayed on its activation to complete within its deadline.
- Regularity of task activation
	- *Periodic tasks*: infinite sequence of identical activities **activated at constant rate**
	- *Aperiodic*: infinite sequence of identical activities with irregular activation
	- *Sporadic*: aperiodic tasks where consecutive activation are separated by some minimum inter-arrival time 

**Precedence**:
- Tasks can have precedence constraint: 
	- A task has to be executed after another task is completed
- Notation: 
	- Precedence relations are described by a directed acyclic graph G.
	- J_a < J_b: task J_a is a predecessor of task J_b
	- J_a -> J_b: task J_a is an immediate predecessor of J_b

**Resource constraints**:
- Resource
	- Any software structure that can be used by the process to advance its execution
- Private resource 
	- A resource dedicated to a particular process
- Shared resource
	- A resource that can be used by many tasks.
--> Critical section

**Definition of scheduling problems**
- Given 
	- J = {J_1,...,J_n}: a set of tasks
	- P = {P_1,...,P_m}: a set of processors
	- R = {R_1,...,R_r}: a set of resources
	- With precedence constraint and timing constraints
- Scheduling problem
	- Assigning processors from P and resource from R to tasks from J under given constraints.

**Complexity of scheduling algorithm**
- Complexity of scheduling decision problem
	- NP-complete in general 
	- Has strong influence on the performance of dynamic real-time systems
- Practical approach:
	- Simplify computer architecture: uniprocessor
	- Adopt additional conditions: preemptive model, priority, task activation ...
	-> Result in different classes of problem that can be solved by different scheduling algorithms.

**Classification of scheduling algorithms**:
- *Preemptive*: the running task can be interrupted at any time
- *Nonpreemptive*: A task, once started, is executed by the processor until completion without interruption by any other tasks.
- *Static*: Scheduling decisions are based one fixed parameters, assigned to task before their activations.
- *Dynamic*: Scheduling decisions are based on dynamic parameters that may change during system evolution.
- *Off-line*: Scheduling decision is executed on the entire task set before actual task execution.
- *On-line*: Scheduling decisions are taken at runtime every time a new task enter the systems or when a running task terminates.
- *Optimal*: Minimize some given cost function defined over the task set. If no cost function is given, optimal algorithm only fail to meet deadline only if no other algorithm can meet the deadline.
- *Heuristic*: Searches for a feasible schedule using an objective function. Does not guarantee to find the optimal schedule.

**Guarantee-based algorithms**
- In hard real-time systems, feasibility of the schedule should be guaranteed before task execution.
- In order to guarantee feasibility: 
	- Static real-time systems:
		- Offline scheduling is used. Requires high predictability and the system becomes inflexible.
	- Dynamic real-time systems:
		- Online scheduling with acceptance test
- Acceptance test in online scheduling
	- For every arrival of new task J_new, it is checked whether J' = J + J_new is schedulable or not 
	- If J' is schedulable, J_new is accepted
	- Otherwise, J_new is rejected
	- Generally based on worst-case assumption
- Demerit: unnecessary rejection
- Merit: detecting potential overload situations
	- Can avoid *domino effects*
- *Domino effect*: 
	- A dangerous phenomena under transient overload.
	- The arrival of a new task causes all previously guaranteed tasks to miss their deadlines.

**Best-effort algorithm**
- In soft real-time system, some deadline misses are acceptable.
- Best effort algorithm
	- Accept all tasks that arrived.
	- Aborts some tasks under real overload conditions.
	- Cannot guarantee feasibility
- Pros: Good avarage performance & avoiding unnecessary rejection
- Cons: Domino effect

**Metrics for performance evaluation**
- Performance of scheduling algorithms is evaluated by a *cost function*.
- An optimal scheduling algorithm generates a schedule that minimizes a given cost function.

**Utility function**
- A kind of cost function with respect to the completion time of a task
- Evaluates lateness in a real-time task that depends on the completion time
- Typical utility functions
	- Non realtime
	- Soft 
	- On time
	- Firm
- To evaluate whole schedule, the cumulative value of utility function values of all task is used.

**Scheduling anomalies**
- Real-time computing != fast computing
- Increase of computing power != improvement of the performance
- *Graham's theorem*
	- If a task is optimally scheduled on a multiprocessor with some priority assignment, a fixed number of processors, fixed execution time, and precedence constraints, then increase the number of processors, reducing the execution times, or weakening the precedence constraint can increase the schedule length.



# Aperiodic realtime scheduling

## Notation of scheduling algorithms
- Systematic notation of scheduling algorithm:
	- $\alpha$ | $\beta$ | $\gamma$ 
	- $\alpha$ : machine environment : uniprocessor, multiprocessor, distributed architecture, etc
	- $\beta$ : tasks & resource characteristics: preemptive, independent, precedence constrained, synchronous activations ...
	- $\gamma$ : optimality criterion
- Example: 
	- <font style="color: red"> 1 </font>| <font style= "color:blue">prec </font>| <font style = "color: green">L_max</font> : Minimizes <font style="color:green">the maximum lateness</font> of a task set with <font style="color:blue">precedence constraint</font> on a <font style="color:red">uniprocessor</font> machine
	- <font style="color: red"> 3 </font>| <font style= "color:blue">no_preem </font>| <font style = "color: green">sumOf(f_i)</font> : Minimizes <font style="color:green">sum of finishing times</font> of a  <font style="color:blue">non preemptive</font> task set on a <font style="color:red"> 3 processors</font> machine
	- <font style="color: red"> 2 </font>| <font style= "color:blue">sync</font> | <font style = "color: green">sumOf(Late_i)</font> : Minimizes <font style="color:green">number of late tasks</font> of a task set with <font style="color:blue">synchronous arrival times</font> on a <font style="color:red">2 processors</font> machine

## Jackson's algorithm (1|sync|L_max)
- A uniprocessor scheduling algorithm of a task set with *synchronous arrival times* -> all task have the same arrival time
- For synchronous arrival times, the scheduling problem only considers execution times & deadlines
- Task set notation:
	- J = {J_i(C_i,D_i), i = 1,..., n}
**Jackson's rule**
- Given a set of n *independent tasks* (no resource & precedence constraints), any algorithm that executes the tasks *in order of non-decreasing deadline* is optimal with respect to *minimizing the maximum lateness* => Earliest Due Date (EDD)

## Horn's algorithm (1|preem|L_max)
- Given a set of n indedependent task with arbitrary arrival times, any algorithm that *at any instant executes the task with the earliest absoute deadline among all teh ready tasks* is optimal with respect to minimizing the maximum lateness => Earliest deadline first (EDF)
- Time complexity O(n^2)
**Guarantee**
- Guarantee test has to be done dynamically, whenever a new task enters the system.
- Given new task J_new arrives at t.
- The new task set is listed with increasing deadline
- Guarantee test: 
	- The worst-case finishing time f_i is less than or equal to its deadline d_i
	- C(t): remaining worst-case execution time of task J_i at time t

## Non-Preemptive scheduling - Bratley's algorithm (1|no_preem|feasible)
- Find a feasible schedule of non-preemptive tasks
- Start with empty schedule
- Present all possible schedules with a tree structure
- Calculate finishing time of each task considering the finishing time of the predecessor, the arrival time and execution time.
- Finds a path of a feasible schedule

## Scheduling with precedence constraints
- NP-hard problem
- Synchronous activation: LDF algorithm
- Pre-emptive task sets: EDF with precedence constraints
## Latest Deadline First (LDF) (1|prec,sync|L_max)
- Given a task set J and a directed acyclic graph representing its precedence constraints
- Scheduling algorithm
	- Select the subtask J' of J with no successor
	- In J', select the task j with the latest deadline
	- Put j into a LIFO queue
	- Repeat until all task are put into queue
	- The tasks in queue will be pop out for execution

## EDF with precedence constraint (1|prec, preem|L_max)
- Scheduling problem: (1|prec,preem|L_max)
- Difficulty: EDF is not optimal with precedence constraints
- Provided task J_a, J_b $\in$ J that J_a -> J_b
-> Start time of J_b and deadline of J_a will be modified to replace precedence constraint
- *Starting time*: 
	- s_b >= r_b: J_b must start the execution not earlier than its release time
	- s_b >= r_a + C_a: J_b must start the execution not earlier than the minimum finishing time of J_a
-> Modifying release time of J_b
	r_b = max(r_b,r_a+C_a)
- *Deadline*: 
	- f_a <= d_a: J_a must finish the execution within its deadline
	- f_a <= d_b - C_b: J_a must finish the execution not later than the maximum start time of J_b.
-> Modifying deadline of J_a
	d_a = min(d_a, d_b - C_b)



# Periodic realtime scheduling

**Periodic scheduling algorithms**
- Timeline scheduling
- Earliest deadling first
- Rate monotonic
- Deadling monotonic
- Earliest Deadline first

**Notation**
- $\Gamma$: a set of periodic tasks
- $\tau_i$: a generic periodic task
- $\tau_{i,j}$: the j-th instance of task $\tau_i$
- $r_{i,j}$: the release time of $\tau_{i,j}$
- $\Phi_i=r_{i,1}$: the phase of $\tau_i$
- $D_i$: the relative deadline of $\tau_i$
- $d_{i,j}$: the absolute deadline of $\tau_{i,j}$
	- $d_{i,j}=\Phi_{i,j}+(j-1)T_i+D_i$ 
- $s_{i,j}$: the start time of $\tau_{i,j}$
- $f_{i,j}$: the finishing time of $\tau_{i,j}$
- Task $\tau_i$'s timing parameters
![[Screenshot from 2023-03-12 14-50-45.png]]
- Task $\tau_i$'s timing parameters is feasible if all its instances finish within their absolute deadline
- A set $\Gamma$ of periodic tasks is schedulable if all task in $\Gamma$ are feasible

**Assumptions**
- The instance of $\tau_i$ is reguarly activated at a constant rate. (Period $T_i$)
- All instances of a task have the same worst-case execution time $C_i$ 
- All instances of a task have the same relative deadline $D_i$ and $D_i=T_i$
- All tasks are independent; no precedence & resource constraints
- No task can suspend itself, for example on I/O operations
- All task are fully pre-emptible
- All overheads in the kernel are ignored

**Periodic task parameters**
- Response time: 
	- Duration from the release time to finishing time
	- $R_{i,k}=f_{i,k}-r_{i,k}$
- Critical instant:
	- The time at which the release of a task will produce the largest response time
- Critcal time zone: 
	- Response time with respect to the critical instant

**Processor utilization factor**:
- Processor utilization for n tasks $$U=\sum_{i=1}^{n}\frac{C_i}{T_i}$$
- U represents how many percent of processor resource is utilized by a given task set
- If U > 1: 
	- Let H be the hyperperiod: U > 1 => UH > H => $\sum_{i=1}^{n}\frac{H}{T_i}C_{i}>H$ 
	- $\frac{H}{T_i}C_i$: total CPU time requested by $T_i$ during H
	-> Total request time during $[0,H)$ bigger than H
	-> Task set is not schedulable
- Given a scheduling algorithm A and a task set $\Gamma$, there will be a upper bound value of U $$U_{ub}(\Gamma,A)$$
	- $U>U_{ub}(\Gamma,A)$: $\Gamma$ is not schedulable by A
	- if $U=U_ub(\Gamma,A)$: $\Gamma$ fully utilizes the processor
- For a given algorithm A, let $$U_{lub}(A)=min U_{ub}(\Gamma,A)$$
	- All task set having $U \le U_{lub}(A)$ will be schedule by A
	- If $1 > U > U_{lub}(A)$, schedualibity depends on actual tasks parameters (activation time, period, ...)

## Algorithm for periodic scheduling
### Time scheduling
- Divide the time into Minor Cycles and Major Cycles
	- Major Cycle = $lcm(T_i)$= H
	- Minor Cycle = $gcd(T_i)$ 
- Scheduling and implementation:
	- Schedule the task execution in each minor cycle of a major cycle
	- Set up a timer with period equal to minor cycle
	- The main function synchronize task execution with timer event
- Advantage: 
	- Simpele, does not require RTOS
	- No context switching, minimal runtime overhead
- Disadvantage: 
	- Domino effect if task does not terminate on time
	- May need to divide task into small pieces
	- Difficult to handle aperiodic and long tasks
	- Sensitive to task parameter changes

### Earliest deadline first (EDF)
- Pre-emptible task set, dynamic priorities
- All tasks instances are consider aperiodice tasks
- Priority and scheduling of task is based on the instances's absolute deadline: $$d_{i,j}=\Phi_i+(j-1)T_{i}+D_i$$
- Proof of optimality is the same as with aperiodic tasks
### Rate Monotonic 
- Pre-emptible task set, static scheduling with fixed priorities
- Priority of task is based on the task's request rate: higher rates (shorter periods) correspond to higher priories
- Optimality: RM is optimal among all fixed-priority
- Schedulability/feasibility analysis: $U_{lub}$

**Proof of optimality**
- For any task T, the critical instance occurs when it is released simultaneously with all higher-priority task
- If a task set $\Gamma$ is schedulable by any fixed priority algorithm, it will be schedulable by RM
	- Given 2 tasks $\tau_1,\tau_2$ with $T_1<T_2$, in critical instants
	- Provided the scheduled violates RM -> $\tau_2$ has higher priority -> The schedule is feasible if $C_1+C_2\le T_1$ 
**RM Schedulability: using U**
- Necessary but not sufficient $U\le1$ 
- Sufficient but not neccessary (LL-bound) $$U\le n(2^\frac{1}{n}-1)$$ As the number of tasks n increases to infinite U->ln2 = 0.69 

### Deadline Monotonic (DM)

- Each task is assigned a priority inversely proportional to its relative deadline 
- Shorter deadlines imply higher priorities

### EDF with D < T
- Dynamic scheduling
- Utilization bound does not work
-> The processor demand approach
-> During any time interval, the total processor demand of the whole task set must be not greater than the available time

**Processor demand**
- Given time interval [0,L], total processor demand for task $\tau_i$ is
$$C_i(0,L)=(\frac{L-D_i}{T_i}+1)C_i$$
**Schedulability analysis**
- Condition on processor demand
- For all L > 0 task set is schedulable by EDF if only if $$L\ge\sum_{i=i}^{n}(\frac{L-D_i}{T_i}+1)C_i$$
-> How many value of L 
- Check at deadline
	- C(0,L) is a step function so we can check the schedulability condition on deadlines
	- The number of values to check is still large






# Fixed priority servers

## Introduction
- Practical systems contain different types of task
	- Periodic tasks for critical activities: time driven, usually with hard timing constrain
	- Aperiodic tasks: event driven, may be hard/soft or non-realtime. 
	-> Hybrid task set
- Problem: How to produce a schedule that
	- Guarantee the scheduablility of critical tasks
	- Provide acceptable response time of soft and non-realtime tasks

**Assumption**
- All periodic task start at t=0 and their deadline and period are equal
- Periodic task are scheduled by RM (fixed priority)
- Arrival times of aperiodic request are unknown
- The minimum inter-arrival time of a sporadic task is assumed to be equal to its deadline.
- All task are fully preemptible.

## Background scheduling
- Schedule periodic task with RM as usual
- Aperiodic tasks are scheduled at background: run when there is no periodic load
![[Screenshot from 2023-03-12 16-35-37.png]]
## Polling server
- Improve average aperiodic task response time
- Create an additional periodic task
	- Called polling server (PS)
	- PS serves aperiodic load asap upon request
- Server task parameter
	- Period $T_S$
	- Computation time $C_S$ 
- PS is scheduled together with other periodic tasks
- When PS is activated 
	- Select a waiting aperiodic task and execute it with server capacity
	- If there is no aperiodic task waiting, server suspends itself and gives up its capacity
- Select $T_{S},C_S$ 
	- Select the smallest server period as possible $T_S=T_1$
	- $C_S=U_ST_S$ with $U_{S}=\frac{2-P}{P}$, $P=\prod_{i=1}^n(U_i+1)$ 
- Problem: If an aperiodic request arrives after the server suspends itself, the request must wait until the next server period
-> Lowering average response time 
## Deferable server
- Improve responsiveness of aperiodic task
- Algorithm
	- Create high priority periodic task to serve aperiodic tasks
	- Server replenishes its capacity at the beginning of each period
	- If no aperiodic load are pending upon server invocation, the server preserves its capacity.
	-> Aperiodic load can be served at anytime
- Select $T_S,C_S$ 
	- Choose $T_S=min(T_i)$
	- $C_S=U_S*T_S$ with $U_S=\frac{2-P}{2P-1}$, $P=\prod_{i=1}^n(U_i+1)$ 

## Priority Exchange
- Similar with DS with
	- Better schedulablity bound 
	- Worse aperiodic responsiveness
- PR algorithm
	- Create a periodic task (PE) with high priority for aperiodic load
	- PE reserves capacity by exchanging for lower priority tasks's execution time.
	- Upon PE activation: if there is no aperiodic load, lower priority tasks can execute and PE accumulates capacity at the corresponding priories.
	- If there is no task waiting, PE capacity resolves to 0.

## Sporadic Server
- Similar to DS
- Delay the replenishing time of server -> server becomes equivalent to a normal periodic task
- Idea: 
	- Divide the timeline of SS to active and inactive time slices
		- Active: server serves or may serve periodic task
		- Inactive: server does not serve periodic task
	- Start of active time slice: mark delayed replenishing time
	- End of active time slice: calculate replenishing amount




# Resource access protocol

## Resource constraint
- Resource:
	- Any software structure that can be used by the process to advance its execution
- Many shared resources do not allow simultaneous access -> required mutual exclusion
- Critical section
	- A piece of code under mutual exclusion constraints
	- Tasks entering critical section have to wait until no other task is holding the resource
**Problems**:
- The task with higher priority has to wait for the task with lower priority
- Blocking time is unbounded -> the system is not predictable

**Terminology & assumptions**
- Periodic task set $\Gamma$ = {$\tau_1,\tau_2,...,\tau_n$}
	- $\tau_n=(C_i,T_i)$
	- Relative deadline $D_i=T_i$ 
- Resource $R_1,...,R_m$ 
	- Each $R_k$ is guarded by semaphore $S_k$ 
- $J_i$: a job of $\tau_i$
- $P_i$: nominal priority of $\tau_i$
- $p_{i}\ge P_i$ : active priority of $\tau_i$
- $z_{i,j}$: j-th critical section of $J_i$
- $d_{i,j}$: duration of $z_{i,j}$ 
- $S_{i,j}$: the semaphore guarding $z_{i,j}$
- $R_{i,j}$: the resource used in $z_{i,j}$ 
- Notation $z_{i,j}\subset z_{i,k}$ mean $z_{i,j}$ is entirely contained in $z_{i,k}$
- Assumptions 
	- $J_1,...,J_n$ are listed in decreasing order of $P_i$ 
	- Jobs don't suspend themselves.
	- The critical sections used by any task are properly nested $$z_{i,k} \subset z_{i,k} ||z_{i,k} \subset z_{i,j} || z_{i,k} \wedge z_{i,k}=0$$
	- Critical sections are guarded by binary semaphores

## Non-preemptive Protocol (NPP)
- Block all other tasks whenever a task enters a critical section
- The dynamic priority of the running task is raised to the highest level $$p_i(R_k)=max{P_h}$$
**Blocking time of Non-preemptive protocol**
- Given the task $T_i$, the set of critical sections that can block $T_i$ 
$\gamma_i=\{Z_{i,k}|P_{j}<P_i,k=1,...,m\}$ 
- The maximum blocking time is $B_i=max\{d_{j,k}-1|Z_{j,k}\in\gamma_i\}$ 
-> Duration of the longest critical section that can block $T_i$

## Highest Locker Priority Protocol (HLP)
- Improves NPP: Raising the priority of the task entering a critical section to the highest priority among the tasks sharing resource.
- When a task enter resources $R_k$, its dynamic priority is raised to $p_i(R_k)=max\{P_h|\tau_{h}uses R_{k}\}$ 
- When the task exits the resource, its dynamic priority is reset to the nomial value $P_i$
- Priority ceiling can be computed offline $C(R_{k)}= max\{P_h|\tau_{h}uses R_k\}$ 
**HLP Blocking time**:
- The set of critical instants that can block task $T_i$
$\gamma_i$ = {$Z_{i,j}$|$P_{j}<P_{i}$ and $C_{R_{k}}\ge P_i$} 
- Hence, maximum blocking time is $B_i=max\{\delta_{i,j}-1|Z_{j,k} \in \gamma_i\}$ 

## Priority Inheritance Protocol
- Modify the priority of tasks in critical sections
- When a task blocks higher-priority tasks, it temporarily inherits the highest priority of the blocked tasks.
	- Prevents preemption of medium-priority tasks.

**Definition**:
- Jobs are scheduled based on their active priorities.
- When the higher-priority job $J_{high}$ is blocked on a semaphore because the lower-priority job $J_{low}$ is in execution of its critical section, the active priority $p_{high}$ of $J_{high}$ is inherited to that of $J_{low}$ 
- The rest of the critical section of $J_{low}$ is executed with the active priority $p_{high}$.
- In the case of the medium-priority job $J_{medium}$ activates, it cannot preempt the execution of $J_{low}$ -> Unbounded priority inversion is avoided.
- Priority inheritance is transitive; if a job $J_3$ blocks a job $J_2$, and $J_2$ blocks a job $J_1$, then $J_3$ inherits the priority of $J_1$ via $J_2$.