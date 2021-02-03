# ![nf-core/hlatyping](docs/images/nf-core-hlatyping_logo.png)

[![GitHub Actions CI Status](https://github.com/nf-core/hlatyping/workflows/nf-core%20CI/badge.svg)](https://github.com/nf-core/hlatyping/actions)
[![GitHub Actions Linting Status](https://github.com/nf-core/hlatyping/workflows/nf-core%20linting/badge.svg)](https://github.com/nf-core/hlatyping/actions)
[![Nextflow](https://img.shields.io/badge/nextflow-%E2%89%A520.04.0-brightgreen.svg)](https://www.nextflow.io/)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.1401039.svg)](https://doi.org/10.5281/zenodo.1401039)

[![install with bioconda](https://img.shields.io/badge/install%20with-bioconda-brightgreen.svg)](https://bioconda.github.io/)
[![Docker](https://img.shields.io/docker/automated/nfcore/hlatyping.svg)](https://hub.docker.com/r/nfcore/hlatyping)
[![Get help on Slack](http://img.shields.io/badge/slack-nf--core%20%23hlatyping-4A154B?logo=slack)](https://nfcore.slack.com/channels/hlatyping)

## Introduction

The pipeline does next-generation sequencing-based Human Leukocyte Antigen (HLA) typing using [OptiType](https://github.com/FRED-2/OptiType). OptiType is a HLA genotyping algorithm based on integer linear programming. Reads of whole exome/genome/transcriptome sequencing data are mapped against a reference of known MHC class I alleles. To produce accurate 4-digit HLA genotyping predictions, all major and minor HLA-I loci are considered simultaneously to find an allele combination that maximizes the number of explained reads.
The pipeline is built using [Nextflow](https://www.nextflow.io), a workflow tool to run tasks across multiple compute infrastructures in a very portable manner. It comes with docker containers making installation trivial and results highly reproducible.

## Quick Start

1. Install [`nextflow`](https://nf-co.re/usage/installation)

2. Install any of [`Docker`](https://docs.docker.com/engine/installation/), [`Singularity`](https://www.sylabs.io/guides/3.0/user-guide/) or [`Podman`](https://podman.io/) for full pipeline reproducibility _(please only use [`Conda`](https://conda.io/miniconda.html) as a last resort; see [docs](https://nf-co.re/usage/configuration#basic-configuration-profiles))_

3. Download the pipeline and test it on a minimal dataset with a single command:

    ```bash
    nextflow run nf-core/hlatyping -profile test,<docker/singularity/podman/conda/institute>
    ```

    > Please check [nf-core/configs](https://github.com/nf-core/configs#documentation) to see if a custom config file to run nf-core pipelines already exists for your Institute. If so, you can simply use `-profile <institute>` in your command. This will enable either `docker` or `singularity` and set the appropriate execution settings for your local compute environment.

4. Start running your own analysis!

    ```bash
    nextflow run nf-core/hlatyping -profile <docker/singularity/podman/conda/institute> --input '*_R{1,2}.fastq.gz'
    ```

See [usage docs](https://nf-co.re/hlatyping/usage) for all of the available options when running the pipeline.

## Documentation

The nf-core/hlatyping pipeline comes with documentation about the pipeline which you can read at [https://nf-co.re/hlatyping](https://nf-co.re/hlatyping).

### Pipeline DAG

The hlatyping pipeline can currently deal with two input formats: `.fastq{.gz}` or `.bam`, not both at the same time however. If the input file type is `bam`, than the pipeline extracts all reads from it and performs an mapping additional step with the `yara` mapper against the HLA reference sequence. Indices are provided in the `./data` directory of this repository. Optitype uses [razers3](https://github.com/seqan/seqan/tree/master/apps/razers3), which is very memory consuming. In order to avoid memory issues during pipeline execution, we reduce the mapping information on the relevant HLA regions on chromosome 6.

#### DAG with `.fastq{.gz}` as input

Creates a config file from the command line arguments, which is then passed to OptiType. In parallel, the fastqs are unzipped if they are passed as archives. OptiType is then used for the HLA typing.

![DAG with `fastq.{gz}` files](docs/images/hlatyping_dag_fastq.png)

#### DAG with `.bam` as input

Creates a config file from the command line arguments, which is then passed to OptiType. In parallel, the reads are extracted from the bam file and mapped again against the HLA reference sequence on chromosome 6. OptiType is then used for the HLA typing.

![DAG with `.bam` file](docs/images/hlatyping_dag_bam.png)

## Credits

nf-core/hlatyping was originally written by [Christopher Mohr](https://github.com/christopher-mohr) from [Institute for Translational Bioinformatics](https://kohlbacherlab.org/team_tbi/) and [Quantitative Biology Center](https://uni-tuebingen.de/forschung/forschungsinfrastruktur/zentrum-fuer-quantitative-biologie-qbic/),  [Alexander Peltzer](https://github.com/apeltzer) from [Böhringer Ingelheim](https://www.boehringer-ingelheim.de), and [Sven Fillinger](https://github.com/sven1103) from [Quantitative Biology Center](https://uni-tuebingen.de/forschung/forschungsinfrastruktur/zentrum-fuer-quantitative-biologie-qbic/).

We thank the following people for their extensive assistance in the development
of this pipeline:

<!-- TODO nf-core: If applicable, make list of people who have also contributed -->

## Contributions and Support

If you would like to contribute to this pipeline, please see the [contributing guidelines](.github/CONTRIBUTING.md).

For further information or help, don't hesitate to get in touch on the [Slack `#hlatyping` channel](https://nfcore.slack.com/channels/hlatyping) (you can join with [this invite](https://nf-co.re/join/slack)).

## Citations

If you use  nf-core/hlatyping for your analysis, please cite it using the following doi: [10.5281/zenodo.1401039](https://doi.org/10.5281/zenodo.1401039)

You can cite the `nf-core` publication as follows:

> **The nf-core framework for community-curated bioinformatics pipelines.**
>
> Philip Ewels, Alexander Peltzer, Sven Fillinger, Harshil Patel, Johannes Alneberg, Andreas Wilm, Maxime Ulysse Garcia, Paolo Di Tommaso & Sven Nahnsen.
>
> _Nat Biotechnol._ 2020 Feb 13. doi: [10.1038/s41587-020-0439-x](https://dx.doi.org/10.1038/s41587-020-0439-x).
> ReadCube: [Full Access Link](https://rdcu.be/b1GjZ)

In addition, references of tools and data used in this pipeline are as follows:

<!-- TODO nf-core: Add bibliography of tools and data used in your pipeline -->
