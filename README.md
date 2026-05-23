# Parallel Intelligence & Galactic Simulations 🌌🚀

An advanced high-performance computing (HPC) project focused on accelerating computational algorithms using shared-memory and distributed-memory parallelization models. Developed as a Course Project for **CCCS 423 Parallel & Distributed Computing** at the **University of Jeddah**.

## Authors
Riyam Assad Alawaji
Dana Suwylih Aljuaid

---

## Project Overview
This project explores multi-core and multi-node optimization techniques applied to two classical algorithmic problems:
1. **Part I: Parallel K-Means Clustering** using **OpenMP** (Shared-Memory Model).
2. **Part II: N-Body Simulation** using **MPI** (Distributed-Memory Model).

---

## Implementation Details

### 1. Parallel K-Means Clustering (OpenMP)
* **Concept:** Accelerates the *Assignment Phase* (categorizing points into nearest centroids via Euclidean distance) utilizing data parallelism.
* **Synchronization:** Handled potential race conditions in the *Update Phase* by utilizing local accumulators per thread and merging them into global variables via `#pragma omp atomic`.
* **Experimental Setup:** Tested with **100,000 points**, **8 clusters**, and **100 max iterations**.

#### OpenMP Performance Benchmarks:
| Threads/Processes (p) | Execution Time (sec) | Speedup ($T_1/T_p$) | Efficiency |
|-----------------------|----------------------|--------------------|------------|
| 1                     | 0.322                | 1.00               | 100.00%    |
| 2                     | 0.161                | 2.00               | 100.00%    |
| 4                     | 0.084                | 3.83               | 95.75%     |
| 8                     | 0.054                | 5.96               | 74.50%     |

### 2. Distributed N-Body Simulation (MPI)
* **Concept:** Simulates gravitational forces and physical interactions where each particle interacts with every other particle ($O(n^2)$ complexity). 
* **Parallelization:** Particle positions are distributed across distinct processes. At every time-step, `MPI_Allgather` is utilized to share updated coordinates globally.
* **Experimental Setup:** Tested using **10,000 particles**.

#### MPI Performance Benchmarks:
| Processes (p) | Execution Time (sec) | Speedup ($T_1/T_p$) | Efficiency |
|---------------|----------------------|--------------------|------------|
| 1             | 0.007264             | 1.00               | 1.00       |
| 2             | 0.012104             | 0.60               | 0.30       |
| 4             | 0.011524             | 0.63               | 0.16       |
| 8             | 0.014333             | 0.51               | 0.06       |

 > Key Insight on MPI: Increasing processes led to a performance decrease due to high communication overhead from `MPI_Allgather` at every step, which eclipsed the small computational workload per process.

---

## How to Run the Code

### Prerequisites
Make sure you have GCC compiler with OpenMP support and an MPI library installed (e.g., MSYS2, MPICH, or OpenMPI).

### Compilation
**For OpenMP (K-Means):**
```bash
gcc -fopenmp kmeans.c -o kmeans.exe
./kmeans.exe
