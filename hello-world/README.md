# Hello, world

A simple "Hello, world" workflow which prints the host which the job is running
on and its namespace using a public Alpine container from Dockerhub.

## Running the Workflow

The following section walks through how to run the Fuzzball workflow
`hello-world.yaml` using the CLI and GUI.

### Using the CLI

To start this workflow using the CLI, run the following command:

```text
$ fuzzball workflow start hello-world.yaml
Workflow "3ff3095e-5f78-4054-a5cd-aa3acecf8be1" started.
```

To monitor the workflow's status, run the following command:

```text
$ fuzzball workflow status --watch 3ff3095e-5f78-4054-a5cd-aa3acecf8be1
KIND       | STATUS   | NAME                                          | STARTED               | FINISHED             
Workflow   | Finished | 3ff3095e-5f78-4054-a5cd-aa3acecf8be1          | 2023-01-20 02:24:54PM | 2023-01-20 02:25:45PM
Image      | Finished | docker://docker.io/library/alpine:latest      | 2023-01-20 02:24:54PM | 2023-01-20 02:25:11PM
Job        | Finished | hello_world                                   | 2023-01-20 02:25:28PM | 2023-01-20 02:25:29PM
Completion | Finished | Completion hooks                              | 2023-01-20 02:25:45PM | 2023-01-20 02:25:45PM
```

To view outputs logged by the workflow, execute the following command:

```text
$ fuzzball workflow logs 3ff3095e-5f78-4054-a5cd-aa3acecf8be1
hello_world          | Hello, world! Hostname aede50f3-c171-42c2-831a-a0566ba9989d, Namespace /mnt:[4026533570]
```

### Using the Fuzzball Web UI

Navigate to the workflow editor.

Open the workflow YAML `hello-world.yaml` in the Fuzzball GUI by drag and drop
or click "Open File" and select it using the file browser.

Start the workflow by clicking play button. You will be prompted to name your
workflow. After providing a name for your workflow, click "Start Workflow".
After your workflow successfully start, you will be prompted to navigate to your
workflow's status page where you can monitor the various steps of the workflow.

To view outputs logged by the workflow, select job `hello-world` and navigate to
the logs tab.
