# LAMMPS

Version: 29Sep2021

[LAMMPS](https://www.lammps.org/) is a popular molecular dynamics suite produced
by Sandia National Laboratories. LAMMPS simulates interactions between atoms and
molecules, and is an extensible system that can simulate pharmaceutical drug
interactions, biological processes, nuclear fuels, and other interactions over
very small timescales.

The following LAMMPS workflow runs a standard Lenard-Jones 3D melt experiment
using a LAMMPS container from Nvidia's NGC catalog on a single node with a GPU.

## Running the Workflow

The following section walks through how to run the Fuzzball workflow
`lammps-gpu-ngc.yaml` using the CLI and GUI.

### Using the CLI

To start this workflow using the CLI, run the following command:

```text
$ fuzzball workflow start lammps-gpu-ngc.yaml
Workflow "cb187830-1c08-4cb9-b932-3de8a65a4237" started.
```

To monitor the workflow's status, run the following command:

```text
$ fuzzball workflow describe cb187830-1c08-4cb9-b932-3de8a65a4237
Name:      lammps-ngc-gpu
Email:     bphan@ciq.co
UserId:    e554e134-bd2d-455b-896e-bc24d8d9f81e
Status:    STAGE_STATUS_STARTED
Created:   2024-06-17 12:51:28PM
Started:   2024-06-17 12:51:28PM
Finished:  N/A
Error:     


Stages:
KIND     | STATUS   | NAME                                          | STARTED               | FINISHED
Workflow | Started  | cb187830-1c08-4cb9-b932-3de8a65a4237          | 2024-06-17 12:51:28PM | N/A
Volume   | Finished | lammps-data-volume                            | 2024-06-17 12:51:29PM | 2024-06-17 12:51:48PM
Image    | Started  | docker://nvcr.io/hpc/lammps:29Sep2021         | 2024-06-17 12:51:29PM | N/A
File     | Finished | https://lammps.sandia.gov/inputs/in.lj.txt... | 2024-06-17 12:52:02PM | 2024-06-17 12:52:07PM
Job      | Pending  | run-lammps                                    | N/A                   | N/A
```

To view outputs logged by the workflow, execute the following command:

```text
$ fuzzball workflow logs cb187830-1c08-4cb9-b932-3de8a65a4237 run-lammps
LAMMPS (29 Sep 2021)
KOKKOS mode is enabled (src/KOKKOS/kokkos.cpp:97)
  will use up to 1 GPU(s) per node
  using 1 OpenMP thread(s) per MPI task
Lattice spacing in x,y,z = 1.6795962 1.6795962 1.6795962
Created orthogonal box = (0.0000000 0.0000000 0.0000000) to (268.73539 134.36770 268.73539)
  1 by 1 by 1 MPI processor grid
Created 8192000 atoms
  using lattice units in orthogonal box = (0.0000000 0.0000000 0.0000000) to (268.73539 134.36770 268.73539)
  create_atoms CPU = 3.047 seconds
Neighbor list info ...
  update every 20 steps, delay 0 steps, check no
  max neighbors/atom: 2000, page size: 100000
  master list distance cutoff = 2.8
  ghost atom cutoff = 2.8
  binsize = 2.8, bins = 96 48 96
  1 neighbor lists, perpetual/occasional/extra = 1 0 0
  (1) pair lj/cut/kk, perpetual
      attributes: full, newton off, kokkos_device
      pair build: full/bin/kk/device
      stencil: full/bin/3d
      bin: kk/device
Setting up Verlet run ...
  Unit style    : lj
  Current step  : 0
  Time step     : 0.005
Per MPI rank memory allocation (min/avg/max) = 1181.0 | 1181.0 | 1181.0 Mbytes
Step Temp E_pair E_mol TotEng Press 
       0         1.44   -6.7733681            0   -4.6133683   -5.0196694 
     100   0.75927734    -5.761232            0   -4.6223161   0.19102612 
Loop time of 1.92832 on 1 procs for 100 steps with 8192000 atoms
Performance: 22402.943 tau/day, 51.859 timesteps/s
72.4% CPU use with 1 MPI tasks x 1 OpenMP threads
MPI task timing breakdown:
Section |  min time  |  avg time  |  max time  |%varavg| %total
---------------------------------------------------------------
Pair    | 0.16802    | 0.16802    | 0.16802    |   0.0 |  8.71
Neigh   | 0.29642    | 0.29642    | 0.29642    |   0.0 | 15.37
Comm    | 0.072074   | 0.072074   | 0.072074   |   0.0 |  3.74
Output  | 0.00063816 | 0.00063816 | 0.00063816 |   0.0 |  0.03
Modify  | 1.3215     | 1.3215     | 1.3215     |   0.0 | 68.53
Other   |            | 0.06967    |            |       |  3.61
Nlocal:    8.19200e+06 ave   8.192e+06 max   8.192e+06 min
Histogram: 1 0 0 0 0 0 0 0 0 0
Nghost:        727553.0 ave      727553 max      727553 min
Histogram: 1 0 0 0 0 0 0 0 0 0
Neighs:         0.00000 ave           0 max           0 min
Histogram: 1 0 0 0 0 0 0 0 0 0
FullNghs:  6.15293e+08 ave 6.15293e+08 max 6.15293e+08 min
Histogram: 1 0 0 0 0 0 0 0 0 0
Total # of neighbors = 6.1529347e+08
Ave neighs/atom = 75.109066
Neighbor list builds = 5
Dangerous builds not checked
Total wall time: 0:00:13
```

### Using the Fuzzball Web UI

Navigate to the workflow editor.

Open the workflow YAML `lammps-gpu-ngc.yaml` in the Fuzzball GUI by drag and drop
or click "Open File" and select it using the file browser.

Start the workflow by clicking play button. You will be prompted to name your
workflow. After providing a name for your workflow, click "Start Workflow".
After your workflow successfully start, you will be prompted to navigate to your
workflow's status page where you can monitor the various steps of the workflow.

To view outputs logged by the workflow, select job `run-lammps` and navigate to
the logs tab.
