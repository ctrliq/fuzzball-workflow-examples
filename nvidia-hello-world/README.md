# Hello, world

A simple "Hello, world" workflow which prints the host which the job is running
on using a public Alpine container from Dockerhub.

Then runs `nvidia-smi` to demonstrate that a GPU is available.

## Running the Workflow

The following section walks through how to run the Fuzzball workflow
`nvidia-hello-world.yaml` using the CLI and GUI.

### Using the Fuzzball CLI

To start this workflow using the CLI, run the following command:

```text
$ fuzzball workflow start nvidia-hello-world.yaml
Workflow "065e8665-f025-4247-8783-d4abdb7723be" started.
```

To monitor the workflow's status, run the following command:

```text
$ fuzzball workflow describe 065e8665-f025-4247-8783-d4abdb7723be
Name:      nvidia-hello-world.yaml
Email:     bphan@ciq.co
UserId:    e554e134-bd2d-455b-896e-bc24d8d9f81e
Status:    STAGE_STATUS_FINISHED
Created:   2024-06-14 04:11:03PM
Started:   2024-06-14 04:11:03PM
Finished:  2024-06-14 04:11:43PM
Error:     


Stages:
KIND     | STATUS   | NAME                                     | STARTED               | FINISHED
Workflow | Finished | 065e8665-f025-4247-8783-d4abdb7723be     | 2024-06-14 04:11:03PM | 2024-06-14 04:11:43PM
Image    | Finished | docker://docker.io/library/alpine:latest | 2024-06-14 04:11:04PM | 2024-06-14 04:11:22PM
Job      | Finished | nvidia-hello-world                       | 2024-06-14 04:11:40PM | 2024-06-14 04:11:42PM
```

To view outputs logged by the workflow, execute the following command:

```text
$ fuzzball workflow logs 065e8665-f025-4247-8783-d4abdb7723be nvidia-hello-world
Hello, world! Hostname nvidia-hello-world
Thu Nov  7 18:41:07 2024       
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 555.42.06              Driver Version: 555.42.06      CUDA Version: 12.5     |
|-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  Tesla V100-SXM2-16GB           Off |   00000000:00:1E.0 Off |                    0 |
| N/A   29C    P0             37W /  300W |       1MiB /  16384MiB |      2%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+
                                                                                         
+-----------------------------------------------------------------------------------------+
| Processes:                                                                              |
|  GPU   GI   CI        PID   Type   Process name                              GPU Memory |
|        ID   ID                                                               Usage      |
|=========================================================================================|
|  No running processes found                                                             |
+-----------------------------------------------------------------------------------------+
```

### Using the Fuzzball GUI

Navigate to the workflow editor.

Open the workflow specification file (Fuzzfile) `nvidia-hello-world.yaml` in the
Fuzzball GUI by drag and drop or click "Open File" and select it using the file
browser.

Start the workflow by clicking play button. You will be prompted to name your
workflow. After providing a name for your workflow, click "Start Workflow".
After your workflow successfully start, you will be prompted to navigate to your
workflow's status page where you can monitor the various steps of the workflow.

To view outputs logged by the workflow, select job `nvidia-hello-world` and navigate to
the logs tab.
