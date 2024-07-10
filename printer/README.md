# Printer

A simple printer workflow which prints integers 1 to 300 with a 1 second sleep
between each integer getting printed using a public Alpine container from
Dockerhub.

## Running the Workflow

The following section walks through how to run the Fuzzball workflow
`printer.yaml` using the CLI and GUI.

### Using the Fuzzball CLI

To start this workflow using the CLI, run the following command:

```text
$ fuzzball workflow start printer.yaml
Workflow "065e8665-f025-4247-8783-d4abdb7723be" started.
```

To monitor the workflow's status, run the following command:

```text
$ fuzzball workflow describe <workflow UUID>
Name:      printer.yaml
Email:     bphan@ciq.co
UserId:    e554e134-bd2d-455b-896e-bc24d8d9f81e
Status:    STAGE_STATUS_STARTED
Created:   2024-06-27 01:56:58PM
Started:   2024-06-27 01:56:58PM
Finished:  N/A
Error:


Stages:
KIND     | STATUS   | NAME                                 | STARTED               | FINISHED
Workflow | Started  | 42bc8d9b-6b20-4b8a-824a-b83cdc610a1c | 2024-06-27 01:56:58PM | N/A
Image    | Finished | docker://alpine:latest               | 2024-06-27 01:56:59PM | 2024-06-27 01:57:14PM
Job      | Started  | printer                              | 2024-06-27 01:57:28PM | N/A
```

To view outputs logged by the workflow, execute the following command:

```text
$ fuzzball workflow logs <workflow UUID> printer
1
2
3
4
...
```

### Using the Fuzzball GUI

Navigate to the workflow editor.

Open the workflow specification file (Fuzzfile) `printer.yaml` in the Fuzzball
GUI by drag and drop or click "Open File" and select it using the file browser.

Start the workflow by clicking play button. You will be prompted to name your
workflow. After providing a name for your workflow, click "Start Workflow".
After your workflow successfully start, you will be prompted to navigate to your
workflow's status page where you can monitor the various steps of the workflow.

To view outputs logged by the workflow, select job `printer` and navigate to
the logs tab.
