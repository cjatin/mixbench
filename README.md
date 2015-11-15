# mixbench
The purpose of this benchmark tool is to evaluate performance bounds of GPUs on mixed operational intensity kernels. The executed kernel is customized on a range of different operational intensity values. Modern GPUs are able to hide memory latency by switching execution to compute operations. Using this tool one can assess the practical optimum balance in both types of operations for a GPU. It's is based on CUDA programming platform so it can be executed only on NVidia GPUs. A long term goal is to develop an OpenCL port.

Kernel types
--------------

Three types of experiments are executed combined with global memory accesses:

1. Single precision Flops (multiply-additions)
2. Double precision Flops (multiply-additions)
3. Integer multiply-addition operations

Building program
--------------

In order to build the program you should make sure that the following variable in "Makefile" is set to the CUDA installation directory:

```
CUDA_INSTALL_PATH = /usr/local/cuda
```

Afterwards, just do make:

```
make
```

Two executables will be produced: "mixbench-cuda" & "mixbench-cuda-bs". The former applies grid strides between accesses of the same thread where the latter applies block size strides.

Execution results
--------------

A typical execution output on a GTX480 GPU is:
```
mixbench (compute & memory balancing GPU microbenchmark)
------------------------ Device specifications ------------------------
Device:              GeForce GTX 480
CUDA driver version: 5.50
GPU clock rate:      1401 MHz
Memory clock rate:   924 MHz
Memory bus width:    384 bits
WarpSize:            32
L2 cache size:       768 KB
Total global mem:    1535 MB
ECC enabled:         No
Compute Capability:  2.0
Total SPs:           480 (15 MPs x 32 SPs/MP)
Compute throughput:  1344.96 GFlops (theoretical single precision FMAs)
Memory bandwidth:    177.41 GB/sec
-----------------------------------------------------------------------
Total GPU memory 1610285056, free 1195106304
Buffer size: 256MB
Trade-off type:compute with global memory (block strided)
----------------------------------------- EXCEL data -----------------------------------------
Operations ratio,  Single Precision ops,,,   Double precision ops,,,     Integer operations   
  compute/memory,    Time,  GFLOPS, GB/sec,    Time,  GFLOPS, GB/sec,    Time,   GIOPS, GB/sec
       0/32,       240.46,    0.00, 142.89,  475.48,    0.00, 144.53,  240.35,    0.00, 142.96
       1/31,       233.58,    9.19, 142.50,  460.28,    4.67, 144.63,  233.64,    9.19, 142.47
       2/30,       225.32,   19.06, 142.96,  445.09,    9.65, 144.75,  225.31,   19.06, 142.97
       3/29,       218.79,   29.45, 142.32,  430.34,   14.97, 144.72,  218.59,   29.47, 142.45
       4/28,       210.23,   40.86, 143.01,  415.24,   20.69, 144.81,  210.31,   40.84, 142.95
       5/27,       203.21,   52.84, 142.66,  400.51,   26.81, 144.77,  203.09,   52.87, 142.75
       6/26,       194.33,   66.31, 143.66,  385.32,   33.44, 144.91,  194.37,   66.29, 143.63
       7/25,       187.40,   80.21, 143.24,  370.81,   40.54, 144.78,  187.47,   80.19, 143.19
       8/24,       175.16,   98.08, 147.12,  355.76,   48.29, 144.87,  175.16,   98.08, 147.12
       9/23,       171.80,  112.50, 143.75,  341.41,   56.61, 144.67,  171.87,  112.45, 143.69
      10/22,       163.28,  131.52, 144.67,  326.14,   65.85, 144.86,  163.18,  131.61, 144.77
      11/21,       155.63,  151.78, 144.88,  311.48,   75.84, 144.79,  155.79,  151.63, 144.73
      12/20,       146.60,  175.78, 146.48,  296.45,   86.93, 144.88,  146.69,  175.68, 146.40
      13/19,       138.97,  200.89, 146.80,  281.73,   99.09, 144.83,  139.08,  200.73, 146.69
      14/18,       129.64,  231.92, 149.09,  266.31,  112.90, 145.15,  130.01,  231.26, 148.67
      15/17,       121.12,  265.96, 150.71,  251.39,  128.14, 145.22,  121.41,  265.31, 150.34
      16/16,       120.11,  286.07, 143.04,  235.84,  145.69, 145.69,  120.05,  286.22, 143.11
      17/15,       111.36,  327.82, 144.63,  219.47,  166.34, 146.77,  111.63,  327.05, 144.29
      18/14,       106.48,  363.01, 141.17,  231.50,  166.98, 129.87,  106.66,  362.40, 140.93
      19/13,        96.10,  424.59, 145.25,  244.40,  166.95, 114.23,   96.65,  422.16, 144.42
      20/12,        89.51,  479.84, 143.95,  257.19,  167.00, 100.20,   89.76,  478.51, 143.55
      21/11,        81.93,  550.41, 144.15,  269.05,  167.61,  87.80,   83.18,  542.19, 142.00
      22/10,        76.16,  620.34, 140.99,  281.86,  167.61,  76.19,   76.29,  619.24, 140.74
      23/ 9,        65.62,  752.64, 147.26,  295.74,  167.01,  65.35,   77.19,  639.87, 125.19
      24/ 8,        60.76,  848.31, 141.38,  308.62,  167.00,  55.67,   80.10,  643.41, 107.24
      25/ 7,        52.04, 1031.56, 144.42,  321.45,  167.02,  46.76,   83.28,  644.68,  90.25
      26/ 6,        48.32, 1155.46, 133.32,  334.31,  167.02,  38.54,   86.50,  645.52,  74.48
      27/ 5,        49.51, 1171.09, 108.43,  347.16,  167.02,  30.93,   89.72,  646.25,  59.84
      28/ 4,        50.70, 1185.88,  84.71,  360.01,  167.02,  23.86,   92.90,  647.25,  46.23
      29/ 3,        52.03, 1197.01,  61.91,  372.87,  167.02,  17.28,   96.09,  648.10,  33.52
      30/ 2,        53.37, 1207.05,  40.23,  385.72,  167.02,  11.13,   99.33,  648.58,  21.62
      31/ 1,        53.41, 1246.43,  20.10,  397.20,  167.60,   5.41,  100.91,  659.74,  10.64
      32/ 0,        53.50, 1284.51,   0.00,  410.01,  167.60,   0.00,  102.51,  670.40,   0.00
----------------------------------------------------------------------------------------------
```

Keep in mind that the relation of a compute to a memory operation is not one by one. One compute operation corresponds to 8 Flops/Iops and one memory operation corresponds to 1 element access (either 32bit float, 64bit float or 32bit int). 

Publications
--------------

If you use this benchmark tool for a research work please provide citation to the following paper:

Konstantinidis, E.; Cotronis, Y., "A Practical Performance Model for Compute and Memory Bound GPU Kernels," Parallel, Distributed and Network-Based Processing (PDP), 2015 23rd Euromicro International Conference on , vol., no., pp.651,658, 4-6 March 2015
doi: 10.1109/PDP.2015.51
URL: http://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=7092788&isnumber=7092002