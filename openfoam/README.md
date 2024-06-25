# OpenFOAM - MotorBike Example

Version: 2212

[OpenFOAM](https://www.openfoam.com/) is the free, open source Computation Fluid
Dynamics (CFD) software developed primarily by OpenCFD Ltd since 2004. It has a
large user base across most areas of engineering and science, from both
commercial and academic organisations. OpenFOAM has an extensive range of
features to solve anything from complex fluid flows involving chemical reactions
, turbulence and heat transfer, to acoustics, solid mechanics, and
electromagnetics.

The following workflow splits up the steps of the
[canonical OpenFOAM motorbike tutorial](https://develop.openfoam.com/Development/openfoam/-/tree/master/tutorials/incompressible/simpleFoam/motorBike)
into individual Fuzzball jobs. The workflow calcualates steady flow around a
motorbike and its rider. It starts off by copying the example into a Fuzzball
ephemeral volume. Then, it runs single process tasks `surfaceFeatureExtract`,
`blockMesh`, and `decomposePar`. Next, meshing (using `snappyHexMesh`) and
`topoSet` are run using 6 cores. Initial conditions for the simulation are set
followed by another series of parallel tasks which include `patchSummary`,
`potentialFoam`, `checkMesh`, and the simulation which executes solver
`simpleFoam`. Finally, the mesh and partitions of the decomposed model are
reconstructed using `reconstructParMesh` and `reconstructPar` respectively. The
case directory is compressed and saved from the workflow's ephemeral volume to a
destination of the user's choice.

All steps of the workflow use an OpenFOAM container on
[Dockerhub](https://hub.docker.com/r/opencfd/openfoam-default) published by
OpenCFD.

## Running the Workflow

The following section walks through how to run the Fuzzball workflow
`openfoam-motorbike-mpi.yaml` using the CLI and GUI.

### Using the Fuzzball CLI

To start this workflow using the CLI, run the following command:

```text
$ fuzzball workflow start openfoam-motorbike-mpi.yaml
Workflow "9324e249-33d3-4424-8867-27d67b6f98a3" started.
```

To monitor the workflow's status, run the following command:

```text
$ fuzzball workflow describe <workflow uuid>
Name:      openfoam-motorbike-mpi.yaml
Email:     bphan@ciq.co
UserId:    e554e134-bd2d-455b-896e-bc24d8d9f81e
Status:    STAGE_STATUS_FINISHED
Created:   2024-05-21 03:22:52PM
Started:   2024-05-21 03:22:52PM
Finished:  2024-05-21 03:37:35PM
Error:     


Stages:
KIND     | STATUS   | NAME                                    | STARTED               | FINISHED
Workflow | Finished | 9324e249-33d3-4424-8867-27d67b6f98a3    | 2024-06-20 04:04:16PM | 2024-06-20 04:20:28PM
Volume   | Finished | openfoam-data-volume                    | 2024-06-20 04:04:17PM | 2024-06-20 04:04:40PM
Image    | Finished | docker://opencfd/openfoam-default:2212  | 2024-06-20 04:04:17PM | 2024-06-20 04:04:34PM
Job      | Finished | prepare-motorbike-case                  | 2024-06-20 04:04:55PM | 2024-06-20 04:05:01PM
Job      | Finished | setup-blockmesh-decompose-par           | 2024-06-20 04:05:15PM | 2024-06-20 04:05:26PM
Job      | Finished | snappy-hex-mesh                         | 2024-06-20 04:06:22PM | 2024-06-20 04:07:39PM
Job      | Finished | toposet                                 | 2024-06-20 04:08:39PM | 2024-06-20 04:08:46PM
Job      | Finished | set-initial-conditions                  | 2024-06-20 04:09:42PM | 2024-06-20 04:09:49PM
Job      | Finished | patch-summary                           | 2024-06-20 04:10:55PM | 2024-06-20 04:11:03PM
Job      | Finished | potential-foam                          | 2024-06-20 04:12:15PM | 2024-06-20 04:12:23PM
Job      | Finished | check-mesh                              | 2024-06-20 04:13:19PM | 2024-06-20 04:13:29PM
Job      | Finished | simple-foam                             | 2024-06-20 04:14:25PM | 2024-06-20 04:17:40PM
Job      | Finished | reconstruct-mesh                        | 2024-06-20 04:17:55PM | 2024-06-20 04:18:11PM
Job      | Finished | tar-results                             | 2024-06-20 04:18:26PM | 2024-06-20 04:19:31PM
File     | Finished | file://motorbike-example-results.tar.gz | 2024-06-20 04:19:47PM | 2024-06-20 04:20:12PM
         |          | ->...                                   |                       | 
```

To view outputs logged by the workflow, use the `fuzzball workflow log` command.
Provide the workflow UUID and job name. For example:

```text
$ fuzzball workflow logs <workflow-uuid> set-initial-conditions
Restore 0/ from 0.orig/  [processor directories]
```

### Using the Fuzzball GUI

Navigate to the workflow editor.

Open the workflow YAML `openfoam-motorbike-mpi.yaml` in the Fuzzball GUI by drag
and drop or click "Open File" and select it using the file browser.

Start the workflow by clicking play button. You will be prompted to name your
workflow. After providing a name for your workflow, click "Start Workflow".
After your workflow successfully starts, you will be prompted to navigate to the
workflow status page where you can monitor the various steps of the workflow.

To view outputs logged by the workflow, select a job (for example,
`setup-blockmesh-decompose-par`) and navigate to the logs tab.

## Saving Results

This workflow can save the results tarball to a destination of the user's choice
. The section walks through the steps required to save the results tarball.

### Prerequisites

In order to write the output file to a S3 bucket, you may need access to a
Fuzzball S3 secret. Please see the
[Fuzzball secrets guide](https://integration.ciq.dev/docs/user-guide/secrets)
for instructions on how to set one up.

### Using the Fuzzball CLI

Update the workflow's egress destination with a S3 URI and Fuzzball secret
. The volumes block below writes workflow output file
`motorbike-example-results.tar.gz` to destination `s3://my-bucket/my-dir/` and
names the output file `motorbike-example-results.tar.gz`. The credentials used
to egress this file is Fuzzball user secret `secret://user/my-s3-bucket-secret`.

```yaml
volumes:
  openfoam-data-volume:
    name: openfoam-data-volume
    reference: volume://user/ephemeral
    egress:
      - source:
          uri: file://motorbike-example-results.tar.gz
        destination:
          uri: s3://my-bucket/my-dir/motorbike-example-results.tar.gz
          secret: secret://user/my-s3-bucket-secret
```

### Using the Fuzzball GUI

After opening the workflow in the Fuzzball GUI, select a job in the workflow
editor. Navigate to the volumes tab. Select the volume named
`openfoam-data-volume`. Edit the egress parameter by specifying a destination
for output file `motorbike-example-results.tar.gz`. Using the secrets drop-down,
specify a secret which has permissions to write to your destination.

### Viewing Results

The YAML example above has egressed result file `motorbike-example-results.tar.gz`
to an S3 bucket at destination
`s3://my-bucket/my-dir/motorbike-example-results.tar.gz`. To pull down the
result file to your working directory, the AWS CLI will be used.

```text
$ aws s3 cp s3://my-bucket/my-dir/motorbike-example-results.tar.gz . 
download: s3://my-bucket/my-dir/motorbike-example-results.tar.gz to ./motorbike-example-results.tar.gz
```

The results archived can be decompressed using command:

```text
$ tar -zxf ./motorbike-example-results.tar.gz
```

After decompressing the archive, you can post-process your results and create
visualizations using open source software ParaView.

## Modifying the Workflow to Run on More Cores

The following section will walk through how to run the parallel steps of the
OpenFOAM motorbike tutorial workflow on more cores by modifying
`openfoam-motorbike-mpi.yaml`. In this example, we will run the parallel steps
using 8 cores and 2 nodes for a total of 16 cores.

First, in the job called `prepare-motorbike-case`, you will need to update the input file
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
