# AlphaFold Apptainer Container

This repo provides definition files to build an Apptainer container of AlphaFold v2 (https://github.com/deepmind/alphafold). The current repository fork deals with an updated list of Python packages. Bare in mind that the accuracy of Alphafold has been tested against a specific set of packages where some of those are either too old and/or buggy, and hence not maintained anymore.

Build instructions from [non-docker setting](https://github.com/kalininalab/alphafold_non_docker) by kalininalab were used.

## Build container
```
# build base container
apptainer build --fakeroot base.sif base.def

# build alphafold container
apptainer build --fakeroot alphafold.sif alphafold.def
```

## Run Alphafold
If using GPUs then use the '--nv' flag, i.e. 'apptainer exec --nv ...'
```
apptainer exec --nv -B <DATA_DIR> alphafold.sif bash
source /opt/miniconda3/etc/profile.d/conda.sh
conda activate alphafold
cd /opt/alphafold/
./run.sh -d <DATA_DIR>  -o <OUTPUT_DIR> -m model_1 -f <SEQUENCE_FILE> -t 2020-05-14
```

## Required Parameters:

-d <data_dir> Path to directory of supporting data

-o <output_dir> Path to a directory that will store the results

-m <model_names> Names of models to use (a comma separated list)

-f <fasta_path> Path to a FASTA file containing one sequence

-t <max_template_date> Maximum template release date to consider (ISO-8601 format - i.e. YYYY-MM-DD). Important if folding historical test sets

## Optional Parameters:

-b Run multiple JAX model evaluations to obtain a timing that excludes the compilation time, which should be more indicative of the time required for inferencing many proteins (default: 'False')

-g <use_gpu> Enable NVIDIA runtime to run with GPUs (default: True)

-a <gpu_devices> Comma separated list of devices to pass to 'CUDA_VISIBLE_DEVICES' (default: 0)

-p Choose preset model configuration - no ensembling (full_dbs) or 8 model ensemblings (casp14) (default: 'full_dbs')
