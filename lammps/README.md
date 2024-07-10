# LAMMPS (version 29Sep2021)

[LAMMPS](https://www.lammps.org/) is a popular molecular dynamics suite produced
by Sandia National Laboratories. LAMMPS simulates interactions between atoms and
molecules, and is an extensible system that can simulate pharmaceutical drug
interactions, biological processes, nuclear fuels, and other interactions over
very small timescales.

The following LAMMPS workflow runs a standard Lenard-Jones 3D melt experiment
and saves the log file using a LAMMPS container from Nvidia's NGC catalog on
a single node with a GPU. The log file contains simulation and benchmark results.

## Running the Workflow

The following section walks through the process of running the Fuzzball workflow
`lammps-gpu-ngc.yaml` using the CLI and GUI.

### Using the Fuzzball CLI

To start this workflow using the CLI, run the following command:

```text
$ fuzzball workflow start lammps-gpu-ngc.yaml
Workflow "45d81a83-dd54-4052-afab-09dd3216abbe" started.
```

To monitor the workflow status, run the following command:

```text
$ fuzzball workflow describe <workflow uuid>
Name:      lammps-gpu-ngc.yaml
Email:     bphan@ciq.co
UserId:    e554e134-bd2d-455b-896e-bc24d8d9f81e
Status:    STAGE_STATUS_FINISHED
Created:   2024-06-21 09:23:58AM
Started:   2024-06-21 09:23:58AM
Finished:  2024-06-21 09:26:44AM
Error:     


Stages:
KIND     | STATUS   | NAME                                    | STARTED               | FINISHED
Workflow | Finished | 45d81a83-dd54-4052-afab-09dd3216abbe    | 2024-06-21 09:23:58AM | 2024-06-21 09:26:44AM
Volume   | Finished | lammps-data-volume                      | 2024-06-21 09:23:58AM | 2024-06-21 09:24:16AM
Image    | Finished | docker://nvcr.io/hpc/lammps:29Sep2021   | 2024-06-21 09:23:58AM | 2024-06-21 09:24:15AM
File     | Finished | https://www.lammps.org/inputs/in.lj.txt | 2024-06-21 09:24:31AM | 2024-06-21 09:24:35AM
         |          | ->...                                   |                       | 
Job      | Finished | run-lammps                              | 2024-06-21 09:25:46AM | 2024-06-21 09:26:09AM
File     | Finished | file://log.lammps ->                    | 2024-06-21 09:26:24AM | 2024-06-21 09:26:28AM
         |          | s3://co-ciq-misc-supp...                |                       | 
```

To view outputs logged by the workflow, execute the following command:

```text
$ fuzzball workflow logs <workflow uuid> run-lammps
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

### Using the Fuzzball GUI

Navigate to the workflow editor.

Open the workflow YAML `lammps-gpu-ngc.yaml` in the Fuzzball GUI by drag and
drop or click "Open File" and select it using the file browser.

Start the workflow by clicking the play button. You will be prompted to name
your workflow. After providing a name for your workflow, click "Start Workflow".
After your workflow successfully starts, you will be prompted to navigate to the
workflow status page where you can monitor the various steps of the workflow.

To view outputs logged by the workflow, select job `run-lammps` and navigate to
the logs tab.

## Saving Results

This workflow can save the LAMMPS log file with simulation and benchmark results
to a destination of the user's choice. The section walks through the steps
required to save the log file.

### Prerequisites

In order to write the output file to a S3 bucket, you may need access to a
Fuzzball S3 secret. Please see the
[Fuzzball secrets guide](https://beta.fuzzball.io/docs/user-guide/secrets)
for instructions on how to set one up.

### Using the Fuzzball CLI

Update the workflow egress destination with a S3 URI and Fuzzball secret. The
volumes block below writes workflow output file `log.lammps` to destination
`s3://my-bucket/my-dir/` and names the output file `log.lammps`. The credentials
used to egress this file is Fuzzball user secret
`secret://user/my-s3-bucket-secret`.

```yaml
volumes:
  lammps-data-volume:
    name: lammps-data-volume
    reference: volume://user/ephemeral
    ingress:
      - source:
          uri: https://www.lammps.org/inputs/in.lj.txt
        destination:
          uri: file://in.lj.txt
    egress:
      - source:
          uri: file://log.lammps
        destination:
          uri: s3://my-bucket/my-dir/log.lammps
          secret: secret://user/my-s3-bucket-secret
```

### Using the Fuzzball GUI

After opening the workflow in the Fuzzball GUI, select a job in the workflow
editor. Navigate to the volumes tab. Select the volume named `lammps-data-volume`
. Edit the egress parameter by specifying a destination for output file
`log.lammps`. Using the secrets drop-down, specify a secret which has
permissions to write to your destination.

### Viewing Results

The CLI example above has saved result file `log.lammps` to an S3 bucket at
destination `ss3://my-bucket/my-dir/log.lammps`. To pull down the result file to
your working directory, the AWS CLI will be used.

```text
$ aws s3 cp ss3://my-bucket/my-dir/log.lammps . 
download: ss3://my-bucket/my-dir/log.lammps to ./log.lammps
```

To view the results, we will use the `cat` command.

```text
$ cat log.lammps
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

Data in the logs can be used in the various
[post-processing tools](https://www.lammps.org/prepost.html) which are
compatible with LAMMPS.

## Extending the Workflow

For more information on how to extend this workflow for your needs, please see
the
[Fuzzball workflow syntax guide](https://beta.fuzzball.io/docs/appendices/workflow-syntax/).
