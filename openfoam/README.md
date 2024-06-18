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

To view outputs logged by the workflow, execute the following command:

```text
$ fuzzball workflow logs 34a21687-6936-4cbe-a8dc-5428cd802d72 reconstruct-mesh
/*---------------------------------------------------------------------------*\
| =========                 |                                                 |
| \\      /  F ield         | OpenFOAM: The Open Source CFD Toolbox           |
|  \\    /   O peration     | Version:  2212                                  |
|   \\  /    A nd           | Website:  www.openfoam.com                      |
|    \\/     M anipulation  |                                                 |
\*---------------------------------------------------------------------------*/
Build  : _c9081d5d-20230220 OPENFOAM=2212 patch=230110 version=2212
Arch   : "LSB;label=32;scalar=64"
Exec   : reconstructParMesh -constant
Date   : May 21 2024
Time   : 22:37:08
Host   : reconstruct-mesh
PID    : 221
I/O    : uncollated
Case   : /data/motorBike
nProcs : 1
trapFpe: Floating point exception trapping enabled (FOAM_SIGFPE).
fileModificationChecking : Monitoring run-time modified files using timeStampMaster (fileModificationSkew 5, maxFileModificationPolls 20)
allowSystemOperations : Allowing user-supplied system call operations
// * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * //
Create time
Merge individual processor meshes back into one master mesh.
Use if the original master mesh has been deleted or the processor meshes
have been modified (topology change).
This tool will write the resulting mesh to a new time step and construct
xxxxProcAddressing files in the processor meshes so reconstructPar can be
used to regenerate the fields on the master mesh.
Not well tested & use at your own risk!
Merge assuming correct, fully matched procBoundaries.
Found 6 processor directories
    Reading database "motorBike/processor0"
    Reading database "motorBike/processor1"
    Reading database "motorBike/processor2"
    Reading database "motorBike/processor3"
    Reading database "motorBike/processor4"
    Reading database "motorBike/processor5"
Time = constant
Writing merged mesh to "constant/polyMesh"
Reconstructing addressing from processor meshes to the newly reconstructed mesh
Processor 0
Read processor mesh: "motorBike/processor0"
Writing addressing : "motorBike/processor0/constant/polyMesh"
    pointProcAddressing
    faceProcAddressing
    cellProcAddressing
    boundaryProcAddressing
Processor 1
Read processor mesh: "motorBike/processor1"
Writing addressing : "motorBike/processor1/constant/polyMesh"
    pointProcAddressing
    faceProcAddressing
    cellProcAddressing
    boundaryProcAddressing
Processor 2
Read processor mesh: "motorBike/processor2"
Writing addressing : "motorBike/processor2/constant/polyMesh"
    pointProcAddressing
    faceProcAddressing
    cellProcAddressing
    boundaryProcAddressing
Processor 3
Read processor mesh: "motorBike/processor3"
Writing addressing : "motorBike/processor3/constant/polyMesh"
    pointProcAddressing
    faceProcAddressing
    cellProcAddressing
    boundaryProcAddressing
Processor 4
Read processor mesh: "motorBike/processor4"
Writing addressing : "motorBike/processor4/constant/polyMesh"
    pointProcAddressing
    faceProcAddressing
    cellProcAddressing
    boundaryProcAddressing
Processor 5
Read processor mesh: "motorBike/processor5"
Writing addressing : "motorBike/processor5/constant/polyMesh"
    pointProcAddressing
    faceProcAddressing
    cellProcAddressing
    boundaryProcAddressing
Time = 100
No meshes
Time = 200
No meshes
Time = 300
No meshes
Time = 400
No meshes
Time = 500
No meshes
End
/*---------------------------------------------------------------------------*\
| =========                 |                                                 |
| \\      /  F ield         | OpenFOAM: The Open Source CFD Toolbox           |
|  \\    /   O peration     | Version:  2212                                  |
|   \\  /    A nd           | Website:  www.openfoam.com                      |
|    \\/     M anipulation  |                                                 |
\*---------------------------------------------------------------------------*/
Build  : _c9081d5d-20230220 OPENFOAM=2212 patch=230110 version=2212
Arch   : "LSB;label=32;scalar=64"
Exec   : reconstructPar -latestTime
Date   : May 21 2024
Time   : 22:37:13
Host   : reconstruct-mesh
PID    : 1
I/O    : uncollated
Case   : /data/motorBike
nProcs : 1
trapFpe: Floating point exception trapping enabled (FOAM_SIGFPE).
fileModificationChecking : Monitoring run-time modified files using timeStampMaster (fileModificationSkew 5, maxFileModificationPolls 20)
allowSystemOperations : Allowing user-supplied system call operations
// * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * //
Create time
Reconstructing fields
region=region0
Time = 500
Reconstructing FV fields
    Reconstructing volScalarFields
        k
        nut
        omega
        p
    Reconstructing volVectorFields
        U
        UNear
    Reconstructing surfaceScalarFields
        phi
Reconstructing point fields
No point fields
No lagrangian fields
No FA fields
End
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

Currently in job `prepare-motorbike-case`, there are some `sed` commands which
modify decomposeParDict. Modify the first `sed` command to change the number of
subdomains from 6 to 16. Next, update the second `sed` command to update the
coefficients to 3 numbers which are factors of 16. For example, 2, 2, and  4.
The `sed` command in job `prepare-motorbike-case` should now look like this:

```text
sed -i 's/numberOfSubdomains\\ 6/numberOfSubdomains\\ 16/g' motorBike/system/decomposeParDict; \
sed -i 's/\\(3 2 1\\)/2 2 4/g' motorBike/system/decomposeParDict; \
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
    cores: 6
    affinity: NUMA
  memory:
    size: 4GiB
```

## Extending the Workflow

For more information on how to extend this workflow for your needs, please see
the
[Fuzzball workflow syntax guide](https://integration.ciq.dev/docs/appendices/workflow-syntax/).
