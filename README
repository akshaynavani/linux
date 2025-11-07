# Steps used to complete the assignment

# Q. Comment on the frequency of exits – does the number of exits increase at a stable rate? Or are there more exits performed during certain VM operations? Approximately how many exits does a full VM boot entail?

The Number of exits do not increase at a stable rate, instead the exit rate is **bursty** 
and tied to what the guest is doing.
- **Early Boot** ( <= ~180K exits ) : Big bursts from paging/EPROM setup and paravirt calls  you can see 
  EPT_MISCONFIG and EPT_VIOLATION show up early, and VMCALL climbs quickly.
- **Post-boot steady state** (> ~180k): VMCALL plateaus at ~24,171 and stays flat, which is a good marker 
  that the heavy “bring-up” hypercalls finished. From here, the slope is dominated by IO_INSTRUCTION and TASK_SWITCH as normal OS
- **Approx exits for a full boot**: From the traces, a reasonable “**boot completed / steady state**” inflection is when VMCALL stops increasing (**≈ 180k exits**). 
  So a full VM boot on our setup is on the order of **~150k–200k exits**.

# Q. Of the exit types, which are the most frequent? Least?

- **Most Frequent** 
    - **IO_INSTRUCTION (#30)**: ~259,090 (~43%) - PIO/MMIO-heavy device and driver activity; this dominates your run.
    - **TASK_SWITCH (#10)**: ~205,089 (~34%) - scheduler churn as the guest runs lots of short tasks/interrupt work.
    - **CPUID (#12)**: ~30,338 (~5%); 

- **Least Frequent** 
    - **RDTSCP (#55)**: 3 (~0.0005%) and HLT (#18): 7 (~0.001%) - essentially negligible in our workload.
    - **EPT_VIOLATION (#48)**: ~1,156 (~0.19%) - low after initial memory mapping settles.
    - **RDMSR (#31)**: ~2,745 (~0.46%), **WRMSR (#32)**: ~2,212 (~0.37%) - relatively infrequent.
    - “Unknown” categories (e.g., #52 ~4,811 (~0.8%)) are minor overall;
