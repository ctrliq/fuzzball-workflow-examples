# BLAST

Version: 2.12.0

[BLAST](https://blast.ncbi.nlm.nih.gov/Blast.cgi) is both a genomic alignment
algorithm and also a software implementation of said algorithm produced by the
NCBI. It takes genomic sequences (strings of A (adenine), T (thymine), C
(cytosine), G (guanine), the four base pairs that make up DNA) that are
generated from sequencing laboratory equipment and aligns them again a reference
genome to determine where in that genome they came from, and thus what genes
they are.

The workflow uses some of the BLAST tooling to pull one set of sequences for
querying and another set of sequences for building a database, builds the
database, and then executes the query. It generates output file
`results/blastp.out` which can be egressed to a path defined in a S3 URI.
All steps of the workflow use a BLAST container published by NCBI on
[Dockerhub](https://hub.docker.com/r/ncbi/blast).

## Prerequsites

This workflow egresses an output file. In order to write the output file to a S3
bucket, you may need access to a Fuzzball S3 secret. Please see the [Fuzzball
secrets guide](https://integration.ciq.dev/docs/user-guide/secrets) for
instructions on how to set one up.

## Running the Workflow

The following section walks through how to run the Fuzzball workflow
`blast.yaml` using the CLI and GUI.

### Using the CLI

First update the workflow's egress destination with a S3 URI and Fuzzball secret
. The volumes block below writes workflow output file `results/blastp.out` to
destination `s3://my-bucket/my-dir/` and names the output file
`my-blastp-results.out`. Credentials used to egress this file, a Fuzzball user
secret `secret://user/my-s3-bucket-secret` is used.

```text
volumes:
  blast-volume:
    name: blast-volume
    reference: volume://user/ephemeral
    egress:
      - source:
          uri: file://results/blastp.out
        destination:
          uri: s3:s3://my-bucket/my-dir/my-blastp-results.out
          secret: secret://user/my-s3-bucket-secret
```

To start this workflow using the CLI, run the following command:

```text
$ fuzzball workflow start blast.yaml
Workflow "8ae68827-4bce-45c6-ab0c-9f086a8052fb" started.
```

To monitor the workflow's status, run the following command:

```text
$ fuzzball workflow describe 8ae68827-4bce-45c6-ab0c-9f086a8052fb 
Name:      blast.yaml
Email:     bphan@ciq.co
UserId:    e554e134-bd2d-455b-896e-bc24d8d9f81e
Status:    STAGE_STATUS_FINISHED
Created:   2024-06-18 09:37:23AM
Started:   2024-06-18 09:37:23AM
Finished:  2024-06-18 09:44:21AM
Error:     


Stages:
KIND     | STATUS   | NAME                                 | STARTED               | FINISHED
Workflow | Finished | 8ae68827-4bce-45c6-ab0c-9f086a8052fb | 2024-06-18 09:37:23AM | 2024-06-18 09:44:21AM
Volume   | Finished | blast-volume                         | 2024-06-18 09:37:24AM | 2024-06-18 09:37:45AM
Image    | Finished | docker://ncbi/blast:2.12.0           | 2024-06-18 09:37:24AM | 2024-06-18 09:41:20AM
Job      | Finished | create-dirs                          | 2024-06-18 09:41:35AM | 2024-06-18 09:41:40AM
Job      | Finished | retrieve-query-sequence              | 2024-06-18 09:41:56AM | 2024-06-18 09:42:04AM
Job      | Finished | retrieve-database-sequences          | 2024-06-18 09:41:58AM | 2024-06-18 09:42:06AM
Job      | Finished | make-blast-database                  | 2024-06-18 09:42:21AM | 2024-06-18 09:42:28AM
Job      | Finished | run-blast                            | 2024-06-18 09:43:38AM | 2024-06-18 09:43:45AM
File     | Finished | file://results/blastp.out ->         | 2024-06-18 09:44:00AM | 2024-06-18 09:44:05AM
         |          | s3://co-ciq-m...                     |                   
```

To view outputs logged by the workflow, use the `fuzzball workflow log` command.
Provide the workflow UUID and job name. For example:

```text
$ fuzzball workflow log 8ae68827-4bce-45c6-ab0c-9f086a8052fb make-blast-database
Building a new DB, current time: 06/18/2024 16:42:26
New DB name:   /data/fasta/nurse-shark-proteins
New DB title:  Nurse shark proteins
Sequence type: Protein
Keep MBits: T
Maximum file size: 1000000000B
Adding sequences from FASTA; added 7 sequences in 0.244187 seconds.
```

### Using the Fuzzball Web UI

Navigate to the workflow editor.

Open the workflow YAML `blast.yaml` in the Fuzzball GUI by drag and drop
or click "Open File" and select it using the file browser.

Select a job in the workflow editor. Navigate to the volumes tab. Select
the volume named `blast-volume`. Edit the egress parameter by specifying a
destination for output file `results/blastp.out`. Using the secrets drop-down,
specify a secret which has permissions to write to your destination.

Start the workflow by clicking play button. You will be prompted to name your
workflow. After providing a name for your workflow, click "Start Workflow".
After your workflow successfully start, you will be prompted to navigate to your
workflow's status page where you can monitor the various steps of the workflow.

To view outputs logged by the workflow, select a job (for example,`run-blast`)
and navigate to the logs tab.

### Viewing Results

The CLI example above has egressed result file `results/blastp.out` to an S3
bucket at destination `s3://co-ciq-misc-support/bphan/blastp-valid-results.out`.
To pull down the result file to your working directory, the AWS CLI will be
used.

```text
$ aws s3 cp s3://co-ciq-misc-support/bphan/blastp-valid-results.out . 
download: s3://co-ciq-misc-support/bphan/blastp-valid-results.out to ./blastp-valid-results.out
```

To view the results, we will use the `cat` command.

```text
$ cat blastp-valid-results.out 
BLASTP 2.12.0+


Reference: Stephen F. Altschul, Thomas L. Madden, Alejandro A.
Schaffer, Jinghui Zhang, Zheng Zhang, Webb Miller, and David J.
Lipman (1997), "Gapped BLAST and PSI-BLAST: a new generation of
protein database search programs", Nucleic Acids Res. 25:3389-3402.


Reference for composition-based statistics: Alejandro A. Schaffer,
L. Aravind, Thomas L. Madden, Sergei Shavirin, John L. Spouge, Yuri
I. Wolf, Eugene V. Koonin, and Stephen F. Altschul (2001),
"Improving the accuracy of PSI-BLAST protein database searches with
composition-based statistics and other refinements", Nucleic Acids
Res. 29:2994-3005.



Database: Nurse shark proteins
           7 sequences; 922 total letters



Query= sp|P01349.2|RELX_CARTA RecName: Full=Relaxin; Contains: RecName:
Full=Relaxin B chain; Contains: RecName: Full=Relaxin A chain

Length=44
                                                                      Score     E
Sequences producing significant alignments:                          (Bits)  Value

P80049.1 RecName: Full=Fatty acid-binding protein, liver; AltName...  14.2    0.96 


>P80049.1 RecName: Full=Fatty acid-binding protein, liver; AltName: Full=Liver-type 
fatty acid-binding protein; Short=L-FABP
Length=132

 Score = 14.2 bits (25),  Expect = 0.96, Method: Compositional matrix adjust.
 Identities = 3/9 (33%), Positives = 6/9 (67%), Gaps = 0/9 (0%)

Query  2    LCGRGFIRA  10
            +C R ++R 
Sbjct  123  VCTREYVRE  131



Lambda      K        H        a         alpha
   0.334    0.143    0.520    0.792     4.96 

Gapped
Lambda      K        H        a         alpha    sigma
   0.267   0.0410    0.140     1.90     42.6     43.6 

Effective search space used: 22680


  Database: Nurse shark proteins
    Posted date:  Jun 18, 2024  4:42 PM
  Number of letters in database: 922
  Number of sequences in database:  7



Matrix: BLOSUM62
Gap Penalties: Existence: 11, Extension: 1
Neighboring words threshold: 11
Window for multiple hits: 40
```

## Extending the Workflow

For more information on how to extend this workflow for your needs, please see
the
[Fuzzball workflow syntax guide](https://integration.ciq.dev/docs/appendices/workflow-syntax/).
