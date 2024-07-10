# Jupyter Notebook

The Jupyter Notebook is the original web application for creating and sharing
computational documents. It offers a simple, streamlined, document-centric
experience.

The following workflow starts a Jupyter notebook within a Fuzzball job.
Leveraging Fuzzball's port-forwarding feature for jobs, a user can connect to
the running notebook and begin interacting with it through their browser.

## Running the Workflow

The following section walks through how to run the Fuzzball workflow
`jupyter-notebook.yaml` using the CLI and GUI.

### Using the Fuzzball CLI

To start this workflow using the CLI, run the following command:

```text
$ fuzzball workflow start jupyter-notebook.yaml
Workflow "aa3b04f0-f245-4032-a7b1-5cef0acfaf6c" started.
```

To monitor the workflow's status, run the following command:

```text
$ fuzzball workflow describe aa3b04f0-f245-4032-a7b1-5cef0acfaf6c
Name:      jupyter-notebook.yaml
Email:     bphan@ciq.co
UserId:    e554e134-bd2d-455b-896e-bc24d8d9f81e
Status:    STAGE_STATUS_STARTED
Created:   2024-06-18 02:42:53PM
Started:   2024-06-18 02:42:53PM
Finished:  N/A
Error:     


Stages:
KIND     | STATUS   | NAME                                          | STARTED               | FINISHED
Workflow | Started  | aa3b04f0-f245-4032-a7b1-5cef0acfaf6c          | 2024-06-18 02:42:53PM | N/A
Volume   | Finished | jupyter-volume                                | 2024-06-18 02:42:54PM | 2024-06-18 02:43:14PM
Image    | Finished | docker://quay.io/jupyter/minimal-notebook:... | 2024-06-18 02:42:54PM | 2024-06-18 02:46:35PM
Job      | Started  | jupyter                                       | 2024-06-18 02:46:50PM | N/A
```

To view outputs logged by the workflow, execute the following command:

```text
$ fuzzball workflow log aa3b04f0-f245-4032-a7b1-5cef0acfaf6c jupyter
[W 2024-06-18 21:46:59.371 ServerApp] A `_jupyter_server_extension_points` function was not found in nbclassic. Instead, a `_jupyter_server_extension_paths` function was found and will be used for now. This function name will be deprecated in future releases of Jupyter Server.
[I 2024-06-18 21:46:59.374 ServerApp] jupyter_lsp | extension was successfully linked.
[I 2024-06-18 21:46:59.381 ServerApp] jupyter_server_terminals | extension was successfully linked.
[W 2024-06-18 21:46:59.383 LabApp] 'token' has moved from NotebookApp to ServerApp. This config will be passed to ServerApp. Be sure to update your config before our next release.
[W 2024-06-18 21:46:59.388 ServerApp] ServerApp.token config is deprecated in 2.0. Use IdentityProvider.token.
[I 2024-06-18 21:46:59.388 ServerApp] jupyterlab | extension was successfully linked.
[I 2024-06-18 21:46:59.392 ServerApp] nbclassic | extension was successfully linked.
[I 2024-06-18 21:46:59.408 ServerApp] notebook | extension was successfully linked.
[I 2024-06-18 21:46:59.410 ServerApp] Writing Jupyter server cookie secret to /home/jovyan/.local/share/jupyter/runtime/jupyter_cookie_secret
[I 2024-06-18 21:47:00.384 ServerApp] notebook_shim | extension was successfully linked.
[W 2024-06-18 21:47:00.498 ServerApp] All authentication is disabled.  Anyone who can connect to this server will be able to run code.
[I 2024-06-18 21:47:00.500 ServerApp] notebook_shim | extension was successfully loaded.
[I 2024-06-18 21:47:00.510 ServerApp] jupyter_lsp | extension was successfully loaded.
[I 2024-06-18 21:47:00.515 ServerApp] jupyter_server_terminals | extension was successfully loaded.
[I 2024-06-18 21:47:00.519 LabApp] JupyterLab extension loaded from /opt/conda/lib/python3.11/site-packages/jupyterlab
[I 2024-06-18 21:47:00.519 LabApp] JupyterLab application directory is /opt/conda/share/jupyter/lab
[I 2024-06-18 21:47:00.520 LabApp] Extension Manager is 'pypi'.
[I 2024-06-18 21:47:00.546 ServerApp] jupyterlab | extension was successfully loaded.
[I 2024-06-18 21:47:00.555 ServerApp] nbclassic | extension was successfully loaded.
[I 2024-06-18 21:47:00.563 ServerApp] notebook | extension was successfully loaded.
[I 2024-06-18 21:47:00.563 ServerApp] Serving notebooks from local directory: /data
[I 2024-06-18 21:47:00.563 ServerApp] Jupyter Server 2.14.1 is running at:
[I 2024-06-18 21:47:00.564 ServerApp] http://jupyter:8888/tree
[I 2024-06-18 21:47:00.564 ServerApp]     http://127.0.0.1:8888/tree
[I 2024-06-18 21:47:00.564 ServerApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[I 2024-06-18 21:47:01.814 ServerApp] Skipped non-installed server(s): bash-language-server, dockerfile-language-server-nodejs, javascript-typescript-langserver, jedi-language-server, julia-language-server, pyright, python-language-server, python-lsp-server, r-languageserver, sql-language-server, texlab, typescript-language-server, unified-language-server, vscode-css-languageserver-bin, vscode-html-languageserver-bin, vscode-json-languageserver-bin, yaml-language-server
```

### Using the Fuzzball GUI

Navigate to the workflow editor.

Open the workflow specification file (Fuzzfile) `jupyter-notebook.yaml` in the
uzzball GUI by drag and drop or click "Open File" and select it using the file
browser.

Start the workflow by clicking play button. You will be prompted to name your
workflow. After providing a name for your workflow, click "Start Workflow".
After your workflow successfully start, you will be prompted to navigate to your
workflow's status page where you can monitor the various steps of the workflow.

To view outputs logged by the workflow, select job `jupyter` and navigate to
the logs tab.

## Accessing the Running Notebook Using Port-Forwarding

To access the Jupyter environment through your browser, you will need access to
the Fuzzball CLI. If you have not installed the Fuzzball CLI, please see the
[Fuzzball quick start guide](https://beta.fuzzball.io/docs/user-guide/quick-start/)
to set it up.

Using the Fuzzball CLI, we will use the Fuzzball workflow port-forward
subcommand. Arguements required for this subcommand are the job name (`jupyter`)
, the port which the Jupyter notebook is running on in the job, and the local
port which it should be forwarded to. The command below forwards job port 8888
to local port 8888.

```text
$ fuzzball workflow port-forward aa3b04f0-f245-4032-a7b1-5cef0acfaf6c jupyter 8888:8888
2024/06/18 14:58:56 port_forward.go:47: Listening on 127.0.0.1:8888
2024/06/18 14:59:03 port_forward.go:57: Accepting connection on 127.0.0.1:8888 from 127.0.0.1:38772
2024/06/18 14:59:03 port_forward.go:57: Accepting connection on 127.0.0.1:8888 from 127.0.0.1:38774
2024/06/18 14:59:04 port_forward.go:57: Accepting connection on 127.0.0.1:8888 from 127.0.0.1:38778
...
```

Once the port is successfully forwarded, you should be able to access the
Jupyter environment at <http://localhost:8888> in your browser.

## Stopping the Workflow

When you are done using your Jupyter environment, you can stop the job using
the Fuzzball CLI or GUI.

### Using the Fuzzball CLI

To stop the workflow using the Fuzzbal CLI, you can use the Fuzzball workflow
stop subcommand. The required arguement for this subcommand is the workflow 
UUID. Below is an example command which stop workflow UUID
`aa3b04f0-f245-4032-a7b1-5cef0acfaf6c`.

```text
$ fuzzball workflow stop aa3b04f0-f245-4032-a7b1-5cef0acfaf6c
Workflow stopped with name aa3b04f0-f245-4032-a7b1-5cef0acfaf6c
```

### Using the Fuzzball GUI

Navigate to your workflow's status page through the workflows list page by
clicking on its UUID.

Click the `Cancel` button to stop your workflow.

## Extending the Workflow

For more information on how to extend this workflow for your needs, please see
the
[Fuzzball workflow syntax guide](https://beta.fuzzball.io/docs/appendices/workflow-syntax/).
