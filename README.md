# Open-Source Generative AI Tools in Chemistry

This README accompanies the **5th Pedagogical Webinar** and guides participants through the installation and setup of:

- **REINVENT4** — an open-source AI-driven generative molecule design framework
- **FLOWR.ROOT** — a flow-matching foundation model for structure-aware 3D ligand generation and affinity prediction

> **References:**
> - Loeffler, J.R. et al. *REINVENT 4: Modern AI-driven generative molecule design.* J Cheminform 16, 20 (2024). [https://doi.org/10.1186/s13321-024-00812-5](https://link.springer.com/article/10.1186/s13321-024-00812-5)
> - Cremer, J. et al. *FLOWR.ROOT: A flow matching based foundation model for joint multi-purpose structure-aware 3D ligand generation and affinity prediction.* arXiv (2024). [https://arxiv.org/abs/2510.02578](https://arxiv.org/abs/2510.02578)

---

## Contents

1. [Installation — REINVENT4](#installation--reinvent4)
2. [Dependencies — MAIZE](#dependencies--maize)
3. [Setting Up and Running a Generative Design Run](#setting-up-and-running-a-generative-design-run)
4. [Analysing the Results from the REINVENT Run](#analysing-the-results-from-the-reinvent-run)
5. [Installation — FLOWR.ROOT](#installation--flowrroot)

---

## Installation — REINVENT4

### Prerequisites

Ensure you have **Conda** (Miniconda or Anaconda) installed before proceeding. All prior public REINVENT models are available on [Zenodo](https://zenodo.org).

### Steps

**1. Clone the repository**

```bash
git clone https://github.com/MolecularAI/REINVENT4.git
cd REINVENT4
```

**2. Create and activate the Conda environment**

```bash
conda create --name reinvent4 python=3.10
conda activate reinvent4
```

**3. Install REINVENT4**

```bash
# CPU-only installation
python install.py cpu

# GPU-accelerated installation (requires a compatible NVIDIA GPU)
# python install.py cu126
```

> ⚠️ GPU/CUDA installation may require additional driver configuration on your system.

**4. Install analysis and visualisation dependencies**

```bash
pip install jupytext mols2grid seaborn
```

**5. Deactivate and return to the parent directory**

```bash
conda deactivate
cd ..
```

---

## Dependencies — MAIZE

MAIZE provides advanced scoring functions and property prediction nodes used within REINVENT4 workflows. It is recommended to install MAIZE in a dedicated Conda environment to avoid dependency conflicts.

> 📚 MAIZE tutorials: [REINVENT4 contrib/tutorials/maize](https://github.com/MolecularAI/REINVENT4/tree/main/contrib/tutorials/maize)

### Steps

**1. Clone the repository**

```bash
git clone https://github.com/MolecularAI/maize-contrib.git
cd maize-contrib
```

**2. Create and activate the environment**

```bash
conda env create -f env-users.yml
conda activate maize
```

**3. Install MAIZE**

```bash
pip install --no-deps .
```

**4. Deactivate and return to the parent directory**

```bash
conda deactivate
cd ..
```

---

## Setting Up and Running a Generative Design Run

A visual interface for compiling the input configuration file (`.yaml`) for a REINVENT run is available via the link below.

> 🔗 **[Web app — Reward function editor](https://github.com/btatsis/5eme_webinaire_pedagogique/blob/main/reinvent-mpo-editor.html)** *(reinvent-mpo-editor.html)*

---

## Analysing the Results from the REINVENT Run

The NLL training curve below provides a diagnostic overview of prior model convergence during a REINVENT run.

![NLL loss curve](figs/NLL_plot.png)
*Figure 1: NLL training curve for the REINVENT4 prior model.*

Compound selection from the generative output can be performed using a multi-objective optimisation approach based on Pareto front analysis (see: [Waller & Guba, J. Chem. Inf. Model., 2016](https://doi.org/10.1021/acs.jcim.6c00421)).

> 🔗 **[Web app — Pareto-based compound selector](https://github.com/btatsis/5eme_webinaire_pedagogique/blob/main/reinvent-pareto-selector.html)** *(reinvent-pareto-selector.html)*

---

## Installation — FLOWR.ROOT

FLOWR.ROOT is a structure-based generative model that jointly performs 3D ligand generation and binding affinity prediction using flow matching. Full installation instructions are available in the [FLOWR.ROOT GitHub repository](https://github.com/jule-c/flowr).

### Prerequisites

Ensure the FLOWR.ROOT Conda environment has been created by following the repository instructions, then activate it:

```bash
conda activate flowr_root
```

### Steps

**1. Install the web UI (flowr_vis)**

From the `flowr_vis/` directory, install the frontend and worker dependencies separately:

```bash
# Frontend (CPU) dependencies
pip install -r requirements.txt

# Worker (GPU) dependencies
pip install -r requirements_worker.txt
```

**2. Place a model checkpoint**

At least one `.ckpt` checkpoint file must be placed in the `ckpts/sbdd/` directory at the project root before launching the application:

```
project_root/
└── ckpts/
    └── sbdd/
        └── flowr_root_v2.2.ckpt   ← place your checkpoint here
```

**3. Launch the application locally**

```bash
cd flowr_vis/
./run_local.sh --ckpt ../ckpts/sbdd/flowr_root_v2.2.ckpt
```

> ℹ️ The worker process requires a CUDA-capable GPU. Ensure your NVIDIA drivers and CUDA toolkit are correctly configured before launching.
