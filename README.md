## Defective hBN-SiC Heterostructure using VASP-NEP-GPUMD

This project provides a systematic guide for generating **machine learning interatomic potentials (MLIP)** and conducting molecular dynamics (MD) simulations with [GPUMD software](https://gpumd.org/).

This developed potential can be used to 
(i) assess the stability of the two-dimensional hBN/SiC heterostructure,
(ii) elucidate **Si–N bond formation** triggered by a boron vacancy (\$\mathrm{V\_B}\$),
and (iii) investigate the behaviour of an isolated Cu adatom on the resulting defective surface.
The sections below describe how to (1) create robust, lightweight datasets and (2) run and analyze molecular-dynamics simulations.

---

### Database Compilation

Detailed instructions are provided for building a robust database (DB).
Density-functional theory (DFT) calculations were carried out with the [Vienna Ab-initio Simulation Package (VASP)](https://www.vasp.at/).


#### 1. Boron Vacancy ($\rm{V}_{\rm{B}}$) Defects
- **Lattice mismatch** results in 15 unique $\rm{V}_{\rm{B}}$ defects in the heterostrcuture's supercell.
<div align="center">
  <img src="fig1.png" alt="The hBN/SiC heterostructure. Unique $\rm{V}_{\rm{B}}$ sites are determined by red circles." width="400">
</div>
- To enrich the DB, all unique defects were created, and their geometries were optimized using [density functional theory](https://www.synopsys.com/glossary/what-is-density-functional-theory.html) implemented in VASP software.
- Depending on the VB site, various numbers of local chemical bonds (ranging from 0 to 4) are formed.
- **DFT Calculations:** conducted using **VASP** to generate `XDATCAR` files.
- Use the script `vasp_structure_rattler_deformer.py` with `--max_strain 0.05` and `--max_amplitude 0.2` to generate `POSCAR` files from XDATCARs.
- Perform single-shot DFT calculations (potentially with higher precision) for these structures, forming a database of **2180 structures**.

#### 2. Ab-Initio Molecular Dynamics for Stable Defects
- For the **most stable defect structure** (with 4 chemical bonds), ab-initio molecular dynamics (AIMD) simulations were conducted:
  - **Temperature:** 500 K
  - **Ensemble:** NVT
  - **Duration:** 6000 steps
- Using a **step size of 10**, an additional **~600 structures** were added to the database.

#### 3. Pristine System
- For the pristine system, **120 additional structures** (pristine and rattled) were added to the database.
- The hBN layer shifts from (0,0) to (ax/2,ay/2). ax=3.09 and ay=5.35 are the rectangular SiC lattice parameters.
---



### How to Use `vasp_structure_rattler_deformer.py`

The `vasp_structure_rattler_deformer.py` script generates strained and rattled `POSCAR` files from a VASP `XDATCAR`.

#### Example Command
Run the script with the following command:
```bash
python vasp_structure_rattler_deformer.py \
    --max_strain 0.05 \
    --max_amplitude 0.01 \
    --start_structure_id 1 \
    --vasp_file ~/XDATCAR \
    --step_size 1 \
    --number_of_rattling 1
```
### Command-Line Parameters

| Parameter | Description | Example|
| ------ | ------ | ------ |
| `--max_strain` | Maximum random strain to apply. | `0.05`
| `--max_amplitude` | Maximum random displacement amplitude. | `0.1`
| `--start_structure_id` | Starting ID for generated structures, greater than 0. | `1`
| `--step_size` | Interval to extract structures from file. | `1`
| `--number_of_rattling` | Number of rattle operations for each configuration. | `1`
| `--vasp_file`  | Path to the VASP structure file. This argument is required. | `./XDATCAR`
| `--output_dir` | Directory to store the generated POSCAR files. | `./poscars_db`


### Repository Structure


### Citation
If you use this workflow or data in your research, please cite the following:
  - our paper ...

### License
This project is licensed under the MIT License. See the LICENSE file for details.



### How to Save and Use
1. Copy the above content and save it to a file named `README.md` or another appropriate name in your repository.
2. Place it in the root directory or the specific project folder.
3. Rendered markdown will automatically be readable on GitHub or other markdown-compatible platforms.

Let me know if you'd like to further customize it!
