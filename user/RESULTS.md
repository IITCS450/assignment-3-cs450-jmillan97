# Lottery Scheduler Results

## Setup

Goal of this assignment is to implement a lottery scheduler where each RUNNABLE process is assigned a number of tickets. Using random selection a process is chosen to continue.

Configuration: 
Two test versions;
1st

- CPUs: 1 (run using `make qemu CPUS=1`)
- Number of children: 2
- Ticket counts: 10, 1000
- Duration per run: 500 ticks

2nd
- CPUs: 1
- Number of children: 4
- Ticket counts: 10, 1000, 250, 20
- Duration: 500 ticks

Each child process:
1. Calls `settickets(n)` with its assigned ticket count.
2. Executes a CPU-bound loop until `uptime()` reaches a fixed end time.
3. Reports the total number of loop iterations completed ("work").

---

## Workload

The workload consists of a tight CPU-bound loop:

```c
while(uptime() < end) {
    work++;
}

```
Ensuring each process continiously consumes CPU time
number of loops (work) serves to approximate CPU time

Ex. of output
pid 9 tickets 10 work 5829
pid 11 tickets 250 work 172183
pid 12 tickets 20 work 12229
pid 10 tickets 1000 work 244088

Expected outcome:
10:20:250:1000 -> ~ 0.78%, 1.5%, 19%, 78%

Outcome behaves similar to expectation, not perfect but that is within expectations.

### Technical Challenges
CPUs=1: running the qemu sim with multiple CPUs allowed for each process to control the work for each core, giving results that did not align with expectations. This took me a long time to figure out that it wasn't my code that was the issue, but that multiple cores was the culprit. 

## Variance and Convergence
With each selection process, the lottery of scheduling is chosen at random. This means while there is a probabilistic chance that each process is chosen with a high degree of certainty, it might not realistically play out like that. Every time a process is chosen, the process resets. While process A might have a 10% chance of getting chosen every time, it might go on long runs not being selected or vice versa. Short runs show this variance stronger, individual runs can deviate from expected ratios.

The Law of Large Numbers enforces that as the number of scheduling decisions increase, the average outcome will converge closer to the expected results. Taking multiple runs and finding the average of these would force the results even closer to expected values. Taking many, many runs would force these results even closer. 

### Conclusion
Overall, this assignment was successful, I've attached screenshots of the run examples to the canvas assignment, showing multiple versions of the test. Having 2 processes with widely varying ticket numbers shows the scheduler immediately working, while having 4 processes with a scattered amount of tickets show the law of large numbers in action.
