# Landscaper

[![Snakemake](https://img.shields.io/badge/snakemake-≥7.24.0-brightgreen.svg)](https://snakemake.github.io)
[![DOI](https://zenodo.org/badge/634765201.svg)](https://zenodo.org/badge/latestdoi/634765201)
![GitHub Actions](https://github.com/chiba-ai-med/Landscaper/actions/workflows/build_test_push.yml/badge.svg)
![GitHub Actions](https://github.com/chiba-ai-med/Landscaper/actions/workflows/dockerrun1.yml/badge.svg)
![GitHub Actions](https://github.com/chiba-ai-med/Landscaper/actions/workflows/dockerrun2.yml/badge.svg)
![GitHub Actions](https://github.com/chiba-ai-med/Landscaper/actions/workflows/dockerrun3.yml/badge.svg)
![GitHub Actions](https://github.com/chiba-ai-med/Landscaper/actions/workflows/unittest1.yml/badge.svg)
![GitHub Actions](https://github.com/chiba-ai-med/Landscaper/actions/workflows/unittest2.yml/badge.svg)
![GitHub Actions](https://github.com/chiba-ai-med/Landscaper/actions/workflows/unittest3.yml/badge.svg)
![GitHub Actions](https://github.com/chiba-ai-med/Landscaper/actions/workflows/release-please.yml/badge.svg)
[![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2Fchiba-ai-med%2FLandscaper.svg?type=shield)](https://app.fossa.com/projects/git%2Bgithub.com%2Fchiba-ai-med%2FLandscaper?ref=badge_shield)

`Snakemake` workflow to perform Energy Landscape Analysis

`Landscaper` consists of the rules below:

![](https://github.com/chiba-ai-med/Landscaper/blob/main/plot/dag.png?raw=true)

# Pre-requisites (our experiment)
- Snakemake: v7.24.0
- Singularity: v3.2.0
- Docker: v20.10.10 (optional)

`Snakemake` is available via Python package managers like `pip`, `conda`, or `mamba`.

`Singularity` and `Docker` are available by the installer provided in each website or package manager for each OS like `apt-get/yum` for Linux, or `brew` for Mac.

For the details, see the installation documents below.

- https://snakemake.readthedocs.io/en/stable/getting_started/installation.html
- https://docs.sylabs.io/guides/3.0/user-guide/installation.html
- https://docs.docker.com/engine/install/

**Note: The following source code does not work on M1/M2 Mac. M1/M2 Mac users should refer to [README_AppleSilicon.md](README_AppleSilicon.md) instead.**

# Usage

In this demo, we use a toy data from [ELAT](https://github.com/tkEzaki/energy-landscape-analysis) (`data/testdata.tsv`) but user can specify any user's data.

## Example with local machine

Perform `Landscaper` by the `snakemake` command as follows.

```bash
snakemake -j 4 --config input=data/testdata.tsv outdir=output \
seed=123456 rownames="" colnames="" \
--resources mem_gb=10 --use-singularity
```

The meanings of all the arguments are below.

- `-j`: Snakemake option to set [the number of cores](https://snakemake.readthedocs.io/en/stable/executing/cli.html#useful-command-line-arguments) (e.g. 10, mandatory)
- `--config`: Snakemake option to set [the configuration](https://snakemake.readthedocs.io/en/stable/snakefiles/configuration.html) (mandatory)
- `input`: Input file (e.g., data/testdata.tsv, mandatory)
- `outdir`: Output directory (e.g., output, mandatory)
- `seed`: Random seed used when visualize status network (e.g., 123456) (optional)
- `rownames`: Row names of input matrix (e.g., data/rownames.tsv) (optional)
- `colnames`: Column names of input matrix (e.g., data/colnames.tsv) (optional)
- `--resources`: Snakemake option to control [resources](https://snakemake.readthedocs.io/en/stable/snakefiles/rules.html#resources) (optional)
- `mem_gb`: Memory usage (GB, e.g. 10, optional)
- `--use-singularity`: Snakemake option to use Docker containers via [`Singularity`](https://snakemake.readthedocs.io/en/stable/snakefiles/deployment.html) (mandatory)

## Example with the parallel environment (GridEngine)

If the `GridEngine` (`qsub` command) is available in your environment, you can add the `qsub` command. Just adding the `--cluster` option, the jobs are submitted to multiple nodes and the computations are distributed.

```bash
snakemake -j 4 --config input=data/testdata.tsv outdir=output \
seed=123456 rownames="" colnames="" \
--resources mem_gb=10 --use-singularity \
--cluster "qsub -l nc=4 -p -50 -r yes" --latency-wait 60
```

## Example with the parallel environment (Slurm)

Likewise, if the `Slurm` (`sbatch` command) is available in your environment, you can add the `sbatch` command after the `--cluster` option.

```bash
snakemake -j 4 --config input=data/testdata.tsv outdir=output \
seed=123456 rownames="" colnames="" \
--resources mem_gb=10 --use-singularity \
--cluster "sbatch -n 4 --nice=50 --requeue" --latency-wait 60
```

## Example with a local machine with Docker

If the `docker` command is available, the following command can be performed without installing any tools.

```bash
docker run --rm -v $(pwd):/work ghcr.io/chiba-ai-med/landscaper:main \
-i /work/data/testdata.tsv -o /work/output \
--cores=4 --memgb=10
```

# Reference
- [ELAT](https://github.com/tkEzaki/energy-landscape-analysis)

# Authors
- Koki Tsuyuzaki
- Tetsuo Ishikawa
- Eiryo Kawakami


## License
[![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2Fchiba-ai-med%2FLandscaper.svg?type=large)](https://app.fossa.com/projects/git%2Bgithub.com%2Fchiba-ai-med%2FLandscaper?ref=badge_large)