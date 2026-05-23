# CPU Microarchitecture Studies

This repository is a collection of experimental studies in CPU microarchitecture. It packages simulator configurations, measurements, plots, automation scripts, and full Russian-language reports produced for an advanced microprocessor architecture course.

The top-level overview deliberately names only implementation details that are present in the published repository. The detailed reports preserve the original terminology and academic context.

## Studies

| Study | Tool | Implementation | Metrics |
|---|---|---|---|
| Instruction queues | gem5 | Split and 192-entry unified queue configurations | IPC |
| Branch prediction | ChampSim | Four published predictor configurations | IPC, MPKI |
| Cache replacement | ChampSim | Three published L2 replacement configurations | Miss rate, IPC |
| Cache insertion | ChampSim | Two published low-priority L2 insertion configurations | Miss rate, IPC |

## Instruction queues

- **Problem:** Determine whether combining specialized instruction queues into one shared queue improves utilization and throughput.
- **Implementation:** A gem5 Neoverse V2 configuration was changed from nine split queues to one 192-entry queue while retaining the aggregate functional-unit pool.
- **Experimental setup:** gem5 25.1.0.1 in syscall-emulation mode, six GAPBS workloads, and identical workload inputs for the split and unified configurations.
- **Result:** The unified configuration reduced IPC on every measured workload, with a mean reduction of approximately 13.6%.
- **Limitations:** The study covers one processor model, one queue capacity, and six graph workloads. The repository contains the report, measurements, and plots, but not the modified gem5 source tree.

[Full report and measurements (Russian)](hw1_report/README.md#3-gem5-split-iq-%D0%BF%D1%80%D0%BE%D1%82%D0%B8%D0%B2-monolithic-iq-%D0%B4%D0%BB%D1%8F-neoverse-v2)

## Branch prediction

- **Problem:** Compare several local/global-history predictor organizations with the supplied baseline under the same storage range and pipeline penalty.
- **Implementation:** Four ChampSim configuration variants are published together with run, parsing, and plotting scripts. The predictor module source files are not included, so this overview does not claim or enumerate unpublished implementations.
- **Experimental setup:** Twenty SPEC CPU 2017 DPC-3 traces, a 12-cycle misprediction penalty, approximately 33–37 Kbit predictor budgets, and a common ChampSim processor/cache configuration.
- **Result:** The best measured variant reached 0.9532 geometric-mean IPC and 1.3229 geometric-mean MPKI: 5.11% higher IPC and 43.33% lower MPKI than the baseline.
- **Limitations:** Results are trace-driven and limited to one configuration and one trace per benchmark. Implementation source and independent storage-cost verification are not published here.

[Full report, configurations, plots, and raw results (Russian)](hw2_report/README.md)

## Cache replacement

- **Problem:** Evaluate the performance and miss-rate trade-off of alternative L2 replacement behavior.
- **Implementation:** Three ChampSim L2 replacement configurations are published with scripts and results. Policy module source files are not part of this repository.
- **Experimental setup:** Twenty SPEC CPU 2017 DPC-3 traces, an eight-way 512 KiB L2 cache, the same branch predictor and cache hierarchy for every run, and no cache prefetcher.
- **Result:** One tested variant remained within 0.02% IPC of the baseline; another improved geometric-mean IPC by 1.97%, although its geometric-mean L2 miss rate was higher.
- **Limitations:** The experiment uses a single cache geometry and trace window. Because policy source is absent, the results document the evaluated binaries and configurations rather than a reproducible source-level implementation.

[Full report, configurations, plots, and raw results (Russian)](hw3_report/README.md)

## Cache insertion

- **Problem:** Test whether inserting incoming L2 lines at lower priority can reduce cache pollution and improve performance.
- **Implementation:** Two low-priority insertion configurations are published on top of the baseline replacement setup; their ChampSim module source files are not included.
- **Experimental setup:** The same twenty traces, processor model, 512 KiB eight-way L2 cache, and simulation settings used in the replacement study.
- **Result:** The two tested variants improved geometric-mean IPC by 2.83% and 2.28%. Both increased aggregate geometric-mean L2 miss rate, showing that miss rate alone did not predict IPC in these runs.
- **Limitations:** The study evaluates only one cache level, one geometry, and fixed policy parameters. Trace sampling and the missing policy source restrict reproducibility and generalization.

[Full report, configurations, plots, and raw results (Russian)](hw3_report/README.md#%D1%80%D0%B5%D0%B0%D0%BB%D0%B8%D0%B7%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D1%8B%D0%B5-%D0%BF%D0%BE%D0%BB%D0%B8%D1%82%D0%B8%D0%BA%D0%B8)

## Repository layout

- [`hw1_report/`](hw1_report/) — the instruction-queue experiment plus two supporting analytical coursework studies
- [`hw2_report/`](hw2_report/) — branch-prediction configurations, measurements, plots, raw output, scripts, and the full report
- [`hw3_report/`](hw3_report/) — cache-policy configurations, measurements, plots, raw output, scripts, and the full report

The complete reports remain in Russian inside their original assignment directories.
