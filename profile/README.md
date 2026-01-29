<p align="center">
  <img width="200" alt="HashCrackerz Logo" src="https://github.com/user-attachments/assets/73297594-3581-4afd-ad0b-39d4bc0e66bf" />
</p>

<h1 align="center">HashCrackerz</h1>

<p align="center">
  <strong>Multi-Platform Parallel SHA-256 Password Cracker Suite</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Language-C++%20%7C%20CUDA%20%7C%20OpenMP-green" alt="Languages">
  <img src="https://img.shields.io/badge/Platforms-NVIDIA%20%7C%20AMD%20%7C%20CPU-blue" alt="Platforms">
  <img src="https://img.shields.io/badge/Algorithm-SHA256-purple" alt="Algorithm">
</p>

---
[🇬🇧 English](README.md) | [🇮🇹 Italiano](README-IT.md)

## 📖 Description

**HashCrackerz** is a SHA-256 password cracking application developed as a project for the **Accelerated Computing Systems** course at the Master's degree in Computer Engineering, University of Bologna.

The project demonstrates how different parallel computing architectures (NVIDIA GPU, AMD GPU, sequential CPU, and parallel CPU) can be leveraged to solve the same computationally intensive problem.

## 🎯 Key Features

- **Incremental Brute Force Attack**: dynamic password generation given a charset and length range
- **Dictionary Attack**: support for external wordlists (e.g., rockyou.txt)
- **Salted Hash Support**: salt handling as prefix/suffix
- **Multi-Platform**: native implementations for CUDA, HIP, and OpenMP
- **Advanced Optimizations**: memory hierarchy optimization, register optimization, loop unrolling

## 🚀 Projects

This organization contains three parallel implementations of the same password cracking algorithm:

### 1. [HashCracker-CUDA](https://github.com/HashCrackerz/HashCracker-CUDA)
![NVIDIA](https://img.shields.io/badge/Platform-NVIDIA%20CUDA-76B900?logo=nvidia)

Optimized implementation for NVIDIA GPUs focusing on:
- Efficient CUDA memory management (Global vs Constant Memory)
- SHA-256 kernel optimization with heavy register usage
- Occupancy vs IPC analysis for compute-bound workloads

**Available versions:**
- CUDA Naive (baseline)
- CUDAv1 (constant memory optimization)
- CUDAv2 (register optimization + loop unrolling)
- Extension (salt + dictionary attack)

### 2. [HashCracker-HIP](https://github.com/HashCrackerz/HashCracker-HIP)
![AMD](https://img.shields.io/badge/Platform-AMD%20HIP-ED1C24?logo=amd)

Port for AMD GPUs via ROCm/HIP with:
- Semi-automatic conversion script from CUDA

**Includes:**
- PowerShell script for semi-automatic CUDA → HIP porting
- Implementations parallel to CUDA versions
- Manual porting guide

### 3. [HashCracker-OpenMP](https://github.com/HashCrackerz/HashCracker-OpenMP)
![OpenMP](https://img.shields.io/badge/Platform-OpenMP%20CPU-0071C5?logo=intel)

Implementation for HPC multi-core systems with:
- Coarse-grained parallelism via OpenMP
- Dynamic scheduling strategies
- Early exit with shared volatile flag
- SLURM templates for cluster computing

**Focus on:**
- Transition from millions of lightweight threads (GPU) to heavyweight CPU threads
- Efficient search-space partitioning
- Scalability on multi-socket architectures

## 📊 Technical Insights
- **SHA-256 is compute-bound**: optimizations focus on IPC rather than memory bandwidth
- **Register pressure**: high register usage (118 in v2) reduces occupancy but increases per-thread performance
- **Block size optimization**: 64-128 threads/block show better performance than the classic 256
- **Early exit**: essential to avoid unnecessary computations after match

## 🛠️ General Requirements

### Hardware
- **CUDA**: NVIDIA GPU (Compute Capability 5.0+)
- **HIP**: AMD GPU with ROCm support
- **OpenMP**: Multi-core CPU

### Software
- **CUDA Toolkit** (11.0+) for NVIDIA version
- **ROCm + HIP** for AMD version
- **GCC/Clang with OpenMP** for CPU version
- **OpenSSL** (all versions)
- **PowerShell** (only for CUDA→HIP porting)

## 💻 Quick Start

```bash
# Clone the repository of the desired version
git clone https://github.com/HashCrackerz/HashCracker-CUDA.git
cd HashCracker-CUDA

# Compile (CUDA example)
nvcc -arch=sm_89 -O3 kernel_v2.cu ... -o hashcracker

# Run
./hashcracker 256 <target_hash> 1 8 ASSETS/CharSet.txt
```

For detailed compilation and execution instructions, refer to the specific README of each project.

## 📚 Common Structure

All three projects share a similar structure:

```
HashCracker-[Platform]/
├── ASSETS/           # Charset and dictionary
├── UTILS/            # Support functions
├── SHA256_*/         # SHA-256 implementation
├── ESTENSIONE/       # Salt + Dictionary attack
├── Sequenziale/      # CPU Baseline (CUDA/HIP only)
└── kernel_*.cu/cpp   # Entry points
```

## 👥 Authors

- [Andrea Vitale](https://github.com/WHYUBM)
- [Matteo Fontolan](https://github.com/itsjustwhitee)

## 📜 License

All projects are distributed under the **AGPL-3.0** license. See the `LICENSE` file in each repository for details.

---

<p align="center">
  <sub>⚠️ Disclaimer: These tools are intended exclusively for educational and research purposes. Use for illegal activities is strictly prohibited.</sub>
</p>
