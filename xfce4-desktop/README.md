# XFCE4 Desktop

An XFCE4 desktop environment presented via TurboVNC and a novnc proxy.

## Running the Workflow

The following section walks through how to run the Fuzzball workflow
`xfce4-desktop.yaml` using the CLI and GUI.

### Accessing the container image

This workflow uses an xfc4-desktop container image stored in CIQ Mountain. Running this workflow requires that Fuzzball be configured with a Mountain acccess key as an OCI secret.

For more information, see <https://beta.fuzzball.io/docs/secrets/> and <https://secure.ciq.com/docs/getting-started>.

### Using the Fuzzball CLI

To start this workflow using the CLI, run the following command:

```text
$ fuzzball workflow start xfce4-desktop.yaml
Workflow "<workflow UUID>" started.
```

To monitor the workflow's status, run the following command:

```text
$ fuzzball workflow describe <workflow UUID>
```

To port-forward the novnc proxy to the local workstation, run the following command:

```text
$ fuzzball workflow port-forward --keep-alive 10 <workflow UUID> xfce4-desktop 6080:6080
```

The `xfce4-desktop` container currently always runs its novnc proxy on tcp port 6080.

### Using the Fuzzball GUI

Navigate to the workflow editor.

Open the workflow specification file (Fuzzfile) `xfce4-desktop.yaml` in the Fuzzball
GUI by drag and drop or click "Open File" and select it using the file browser.

Start the workflow by clicking play button. You will be prompted to name your
workflow. After (optionally) providing a name for your workflow, click "Start Workflow".
After your workflow has successfully started, you will be prompted to navigate to your
workflow's status page where you can monitor the various steps of the workflow.

It is not currently possible to configure the necessary port forward to access novnc from the Fuzzball GUI. See the CLI instructions above for further instructions.

## Accessing the desktop

Once the xfce4-desktop job is running and the port forward is established, navigate to http://localhost:6080/vnc_auto.html
