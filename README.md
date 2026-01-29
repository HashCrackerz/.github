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

## 📖 Descrizione

**HashCrackerz** è un applicativo per il cracking di password SHA-256 sviluppata come progetto per il corso di **Sistemi di Elaborazione Accelerata** della facoltà di Ingegneria Informatica Magistrale di UniBo.

Il progetto dimostra come diverse architetture di calcolo parallelo (GPU NVIDIA, GPU AMD, CPU sequenziale e CPU parallelizzata) possano essere sfruttate per risolvere lo stesso problema computazionale intensivo.

## 🎯 Caratteristiche Principali

- **Attacco Brute Force Incrementale**: generazione dinamica di password dato un charset e range di lunghezza
- **Attacco a Dizionario**: supporto per wordlist esterne (es. rockyou.txt)
- **Supporto Hash Saltati**: gestione di salt come prefisso/suffisso
- **Multi-Platform**: implementazioni native per CUDA, HIP e OpenMP
- **Ottimizzazioni Avanzate**: memory hierarchy optimization, register optimization, loop unrolling

## 🚀 Progetti

Questa organizzazione contiene tre implementazioni parallele dello stesso algoritmo di password cracking:

### 1. [HashCracker-CUDA](https://github.com/HashCrackerz/HashCracker-CUDA)
![NVIDIA](https://img.shields.io/badge/Platform-NVIDIA%20CUDA-76B900?logo=nvidia)

Implementazione ottimizzata per GPU NVIDIA con focus su:
- Gestione efficiente della memoria CUDA (Global vs Constant Memory)
- Ottimizzazione kernel SHA-256 con heavy register usage
- Analisi occupancy vs IPC per workload compute-bound

**Versioni disponibili:**
- CUDA Naive (baseline)
- CUDAv1 (constant memory optimization)
- CUDAv2 (register optimization + loop unrolling)
- Estensione (salt + dictionary attack)

### 2. [HashCracker-HIP](https://github.com/HashCrackerz/HashCracker-HIP)
![AMD](https://img.shields.io/badge/Platform-AMD%20HIP-ED1C24?logo=amd)

Port per GPU AMD tramite ROCm/HIP con:
- Script di conversione semi-automatica da CUDA

**Include:**
- Script PowerShell per porting semi-automatico CUDA → HIP
- Implementazioni parallele alle versioni CUDA
- Guida al porting manuale

### 3. [HashCracker-OpenMP](https://github.com/HashCrackerz/HashCracker-OpenMP)
![OpenMP](https://img.shields.io/badge/Platform-OpenMP%20CPU-0071C5?logo=intel)

Implementazione per sistemi HPC multi-core con:
- Parallelismo a grana grossa tramite OpenMP
- Strategie di scheduling dinamico
- Early exit con flag volatile condivisa
- Template SLURM per cluster computing

**Focus su:**
- Transizione da milioni di thread leggeri (GPU) a thread CPU pesanti
- Suddivisione efficiente dello search-space
- Scalabilità su architetture multi-socket

## 📊 Insight Tecnici
- **SHA-256 è compute-bound**: le ottimizzazioni si concentrano su IPC piuttosto che memory bandwidth
- **Register pressure**: alto uso di registri (118 in v2) riduce occupancy ma aumenta performance per thread
- **Block size optimization**: 64-128 thread/block mostrano performance migliori rispetto ai classici 256
- **Early exit**: fondamentale per evitare computazioni inutili dopo il match

## 🛠️ Requisiti Generali

### Hardware
- **CUDA**: GPU NVIDIA (Compute Capability 5.0+)
- **HIP**: GPU AMD con ROCm support
- **OpenMP**: CPU multi-core

### Software
- **CUDA Toolkit** (11.0+) per versione NVIDIA
- **ROCm + HIP** per versione AMD
- **GCC/Clang con OpenMP** per versione CPU
- **OpenSSL** (tutte le versioni)
- **PowerShell** (solo per porting CUDA→HIP)

## 💻 Quick Start

```bash
# Clona il repository della versione desiderata
git clone https://github.com/HashCrackerz/HashCracker-CUDA.git
cd HashCracker-CUDA

# Compila (esempio CUDA)
nvcc -arch=sm_89 -O3 kernel_v2.cu ... -o hashcracker

# Esegui
./hashcracker 256 <target_hash> 1 8 ASSETS/CharSet.txt
```

Per istruzioni dettagliate di compilazione ed esecuzione, consulta il README specifico di ogni progetto.

## 📚 Struttura Comune

Tutti e tre i progetti condividono una struttura simile:

```
HashCracker-[Platform]/
├── ASSETS/           # Charset e dictionary
├── UTILS/            # Funzioni di supporto
├── SHA256_*/         # Implementazione SHA-256
├── ESTENSIONE/       # Salt + Dictionary attack
├── Sequenziale/      # Baseline CPU (solo CUDA/HIP)
└── kernel_*.cu/cpp   # Entry points
```

## 👥 Autori

- [Andrea Vitale](https://github.com/WHYUBM)
- [Matteo Fontolan](https://github.com/itsjustwhitee)

## 📜 Licenza

Tutti i progetti sono distribuiti sotto licenza **AGPL-3.0**. Vedi il file `LICENSE` in ogni repository per i dettagli.

---

<p align="center">
  <sub>⚠️ Disclaimer: Questi strumenti sono pensati esclusivamente per scopi educativi e di ricerca. L'uso per attività illegali è severamente vietato.</sub>
</p>
