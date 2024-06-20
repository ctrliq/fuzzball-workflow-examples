# OpenFOAM - MotorBike Example

Version: 2212

OpenFOAM is the free, open source CFD software developed primarily by OpenCFD
Ltd since 2004. It has a large user base across most areas of engineering and
science, from both commercial and academic organisations. OpenFOAM has an
extensive range of features to solve anything from complex fluid flows involving
chemical reactions, turbulence and heat transfer, to acoustics, solid mechanics
and electromagnetics.

The following workflow splits up the steps of the OpenFOAM motorbike tutorial
example which runs solver simpleFoam into individual Fuzzball jobs. The workflow
starts off by copying the example into a Fuzzball ephemeral volume. The workflow
then runs single process tasks surfaceFeatureExtract, blockMesh, and
decomposePar. Next, meshing (using snappyHexMesh) and topoSet are run using 6
cores. Initial conditions for the simulation are set followed by another series
of parallel tasks which include patchSummary, potentialFoam, checkMesh, and the
simulation which executes solver simpleFoam. Finally, the mesh and partitions of
the decomposed model are reconstructed using reconstructParMesh and
reconstructPar respectively.

All steps of the workflow use an OpenFOAM container on
[Dockerhub](https://hub.docker.com/r/opencfd/openfoam-default) published by
OpenCFD.

## Running the Workflow

The following section walks through how to run the Fuzzball workflow
`openfoam-motorbike-mpi.yaml` using the CLI and GUI.

### Using the CLI

To start this workflow using the CLI, run the following command:

```text
$ fuzzball workflow start openfoam-motorbike-mpi.yaml
Workflow "34a21687-6936-4cbe-a8dc-5428cd802d72" started.
```

To monitor the workflow's status, run the following command:

```text
$ fuzzball workflow describe 34a21687-6936-4cbe-a8dc-5428cd802d72
Name:      openfoam-motorbike-mpi.yaml
Email:     bphan@ciq.co
UserId:    e554e134-bd2d-455b-896e-bc24d8d9f81e
Status:    STAGE_STATUS_FINISHED
Created:   2024-05-21 03:22:52PM
Started:   2024-05-21 03:22:52PM
Finished:  2024-05-21 03:37:35PM
Error:     


Stages:
KIND     | STATUS   | NAME                                   | STARTED               | FINISHED
Workflow | Finished | 34a21687-6936-4cbe-a8dc-5428cd802d72   | 2024-05-21 03:22:52PM | 2024-05-21 03:37:35PM
Volume   | Finished | openfoam-data-volume                   | 2024-05-21 03:22:53PM | 2024-05-21 03:23:11PM
Image    | Finished | docker://opencfd/openfoam-default:2212 | 2024-05-21 03:22:53PM | 2024-05-21 03:23:09PM
Job      | Finished | prepare-motorbike-case                 | 2024-05-21 03:23:26PM | 2024-05-21 03:23:31PM
Job      | Finished | setup-blockmesh-decompose-par          | 2024-05-21 03:23:49PM | 2024-05-21 03:24:08PM
Job      | Finished | snappy-hex-mesh                        | 2024-05-21 03:25:22PM | 2024-05-21 03:26:43PM
Job      | Finished | toposet                                | 2024-05-21 03:27:42PM | 2024-05-21 03:27:48PM
Job      | Finished | set-initial-conditions                 | 2024-05-21 03:28:46PM | 2024-05-21 03:28:52PM
Job      | Finished | patch-summary                          | 2024-05-21 03:29:57PM | 2024-05-21 03:30:05PM
Job      | Finished | potential-foam                         | 2024-05-21 03:31:23PM | 2024-05-21 03:31:32PM
Job      | Finished | check-mesh                             | 2024-05-21 03:32:58PM | 2024-05-21 03:33:05PM
Job      | Finished | simple-foam                            | 2024-05-21 03:33:59PM | 2024-05-21 03:36:49PM
Job      | Finished | reconstruct-mesh                       | 2024-05-21 03:37:04PM | 2024-05-21 03:37:19PM
```

To view outputs logged by the workflow, use the `fuzzball workflow log` command.
Provide the workflow UUID and job name. For example:

```text
$ fuzzball workflow logs 34a21687-6936-4cbe-a8dc-5428cd802d72 set-initial-conditions
Restore 0/ from 0.orig/  [processor directories]
```

### Using the Fuzzball Web UI

Navigate to the workflow editor.

Open the workflow YAML `openfoam-motorbike-mpi.yaml` in the Fuzzball GUI by drag
and drop or click "Open File" and select it using the file browser.

Start the workflow by clicking play button. You will be prompted to name your
workflow. After providing a name for your workflow, click "Start Workflow".
After your workflow successfully start, you will be prompted to navigate to your
workflow's status page where you can monitor the various steps of the workflow.

To view outputs logged by the workflow, select a job (for example,
`setup-blockmesh-decompose-par`) and navigate to the logs tab.

## Modifying the Workflow to Run on More Cores

The following section will walk through how to run the parallel steps of the
OpenFOAM motorbike tutorial workflow on more cores by modifying
`openfoam-motorbike-mpi.yaml`. In this example, we will run the parallel steps
using 8 cores and node 2 for a total of 16 cores.

First, in job `prepare-motorbike-case`, you will need to update input file
`motorBike/system/decomposeParDict` to decompose the model into 16 subdomains
and update the coefficients in the file to 3 integers which are factors of 16
(ex. 2, 2, and 4). You can update the contents of this file using the `sed`
command. Below is an example job command for job `prepare-motorbike-case` which
sets up the simulations and meshing steps to run on 16 cores.

```text
command: ["/bin/bash", "-c", "cp -r $WM_PROJECT_DIR/tutorials/incompressible/simpleFoam/motorBike .; \
            cp motorBike/system/decomposeParDict.6 motorBike/system/decomposeParDict; \
            sed -i 's/numberOfSubdomains\\ 6/numberOfSubdomains\\ 16/g' motorBike/system/decomposeParDict; \
            sed -i 's/\\(3 2 1\\)/2 2 4/g' motorBike/system/decomposeParDict; \
            cat motorBike/system/decomposeParDict;"]
```

Update the resource requests of parallel jobs `snappy-hex-mesh`,`toposet`,
`patch-summary`, `potential-foam`, `check-mesh`, and `simple-foam` to use 16
cores (2 nodes, 8 cores per node). First, request 2 node by updating `nodes` in
the `multinode` block to 2. Each job mentioned should have a multinode block
as follows:

```text
multinode:
  nodes: 2
  implementation: openmpi
```

Finally, update the resource block of the jobs to use 8 cores. Each job 
mentioned should have a resource block as follows:

```text
resource:
  cpu:
    cores: 8
    affinity: NUMA
  memory:
    size: 4GiB
```

## Extending the Workflow

For more information on how to extend this workflow for your needs, please see
the
[Fuzzball workflow syntax guide](https://integration.ciq.dev/docs/appendices/workflow-syntax/).
