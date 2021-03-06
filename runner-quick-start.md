## Example of running of INTEGRAL workflows

Department of Astronomy at UNIGE relies on two clusters local [LESTA] and university-level [Baobab]. Both use SLURM. Baobab supports Singularity, which is what we use here.

* Acquire token

ask someone (e.g. support@odahub.io).

### Setting up the environment

Node manager to fetch workflows to compute and upload results (established facts of equivalence between DAG expressions and data references):

```bash
$ pip install oda-node
```

pipeline managent used for INTEGRAL:

```bash
$ pip install data-analysis 
```

To allow fetching INTEGRAL data:

```bash
$ pip install integral-data-mirror
```

* Execute a single task

```bash
oda-node runner execute
```

* Start the runner

Runner will discover unsolved tasks and lauch jobs whenever it is necessary.

```bash
$ oda-node runner start-executor \
    command-to-execute-runner
    command-to-return-list-of-runners  \
    --token my-oda-token
```

### Re-using environment  with a singularity container

```
singularity exec -B/tmp/pfiles:/home/build/pfiles \
            -B $DATA_ROOT:/data \
            -B $DATA_ROOT/integral:/isdc/arc/rev_3/ \
            -B $DATA_ROOT/integral:/isdc/pvphase/nrt/ops/ \
            oda-integral-worker.img
```


### Example of INTEGRAL analysis at several UNIGE clusters:

```bash
$ oda-node runner start-executor \
    'bash -c "export -n partition; export batch_time=00:10:00; seq 1 3 > jobs; bao-submit-array ../integral-oda-worker/ jobid jobs"'  \
    'bao-squeue'  \
    --token my-oda-token
```

## Interruptable runner

Some HPC platforms allow analysis to declare capacity to check-point, and runner supports this mode.
