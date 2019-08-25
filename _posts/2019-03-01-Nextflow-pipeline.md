---
title: Pipelines with Nextflow
description: Setting up and running a pipeline with Nextflow
date: 2019-03-03 21:51
image:
  path: /assets/images/categories/nextflow.png
categories:
  - Pipelines
tags:
- Containers
- Docker
- Nextflow
toc: true
toc_sticky: true
author_profile: false
---

Nowadays, workflow management systems have become an integral part of large-scale analysis of biological datasets with multiple software packages and multi-platform language support. These systems enable the rapid prototyping and deployment of pipelines that combine complementary software packages.
Several such systems are already available, such as [Snakemake](https://snakemake.readthedocs.io/en/stable/) and [CWL](https://www.commonwl.org/).

This post will give you an overview of my favourite workflow building system - [Nextflow](https://www.nextflow.io/) - and look at one toy workflow implementation example that will also be used in later posts.

## Nextflow

Here, I will more or less shamelessly copy large parts of the description of Nextflow's [website]((https://www.nextflow.io/)) since it summarises the main features quite neatly.

Up front, the most severe disadvantage for me: Nextflow is written in [Groovy](https://groovy-lang.org/) which is kind of a pain for me, since I am mostly Python, R, C/C++ and Java based, but have never needed to touch any Groovy.

However, with some fiddling around and especially a lot of low-latency community support via the [Nextflow Gitter channel](https://gitter.im/nextflow-io/nextflow), these are hurdles that can be overcome.

Once you lost your fear of Groovy, the advantages of Nextflow are quite convincing.

If you want to read more about Nextflow, [here is the documentation](https://www.nextflow.io/docs/latest/index.html) and [here is the original paper](https://www.nature.com/articles/nbt.3820).

#### Fast prototyping

Nextflow allows you to write a computational pipeline by making it simpler to put together many different tasks.

You may reuse your existing scripts and tools and you don't need to learn a new language or API to start using it.

As an example, look at how easy it is to run code from different languages within Nextflow processes out of the box.

```java
process perlStuff {

    """
    #!/usr/bin/perl

    print 'Hi there!' . '\n';
    """

}

process pyStuff {

    """
    #!/usr/bin/python

    x = 'Hello'
    y = 'world!'
    print "%s - %s" % (x,y)
    """

}
```

#### Portable

Nextflow provides an abstraction layer between your pipeline's logic and the execution layer, so that it can be executed on multiple platforms without it changing.

It provides out of the box executors for SGE, LSF, SLURM, PBS and HTCondor batch schedulers and for Kubernetes, Amazon AWS and Google Cloud platforms.

Again, check the so-called profile configurations one can quite easily setup to enable support for yet another scheduler.

```java
profiles {

    standard {
        process.executor = 'local'
    }

    cluster_sge {
        process.executor = 'sge'
        process.penv = 'smp'
        process.cpus = 20
        process.queue = 'public.q'
        process.memory = '10GB'
    }

    cluster_slurm {
        process.executor = 'slurm'
        process.cpus = 20
        process.queue = 'work'
    }
}
```

With these few lines of code, you can now seamlessly execute your pipeline on your local machine, on PBS and SLURM, even with customized resource settings.

#### Reproducibility

Nextflow supports [Docker](https://www.docker.com/) and [Singularity](https://singularity.lbl.gov/) containers technology.

This, along with the integration of the GitHub code sharing platform, allows you to write self-contained pipelines, manage versions and to rapidly reproduce any former configuration.

This is an especially nice feature, since it also allows to run Nextflow workflows on cloud based platforms such as [Amazon Web Services](https://aws.amazon.com/) which strictly require all software environments supplied in a public [Docker registry](https://www.nextflow.io/docs/latest/awscloud.html#awscloud-batch-config) reachable by ECS batch.

#### Unified parallelism

Nextflow is based on the dataflow programming model which greatly simplifies writing complex distributed pipelines.

Parallelisation is implicitly defined by the processes input and output declarations. The resulting applications are inherently parallel and can scale-up or scale-out, transparently, without having to adapt to a specific platform architecture.

#### Continuous checkpoints

All the intermediate results produced during the pipeline execution are automatically tracked.

This allows you to resume its execution, from the last successfully executed step, no matter what the reason was for it stopping.

#### Stream oriented

Nextflow extends the Unix pipes model with a fluent DSL, allowing you to handle complex stream interactions easily.

It promotes a programming approach, based on functional composition, that results in resilient and easily reproducible pipelines.

## Salmon

Our first small toy Nextflow workflow will be based upon [Salmon](https://combine-lab.github.io/salmon/).

Salmon is a tool for quantifying the expression of transcripts using RNA-seq data. Salmon uses the concept of quasi-mapping coupled with a two-phase inference procedure to provide accurate expression estimates very quickly (i.e. wicked-fast) and while using little memory. Salmon performs its inference using an expressive and realistic model of RNA-seq data that takes into account experimental attributes and biases commonly observed in real RNA-seq data.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/Salmon-pipeline/salmon.png" alt="Salmon overview">

Essentially, Salmon will create a transcript index which it then uses to quantify expression estimates for each of the transcripts from raw fastq reads.

Our goal:

* Obtain those transcript expression estimates for our samples
* Obtain reads mapping to these transcripts via the `--writeMappings` flag as pseudo-bam

If you want to read more on Salmon, [here is the paper](https://www.nature.com/articles/nmeth.4197).

## salmon-nf

So the Nextflow pipeline we will create during this exercise I will call `salmon-nf` and it can be found on my [GitHub page](https://github.com/t-neumann/salmon-nf) as a fully functional repository.

Any standalone Nextflow pipeline will need 2 files to be executable out of the box and also directly [from GitHub](https://www.nextflow.io/docs/latest/sharing.html#running-a-pipeline):

* `main.nf` - This file contains the individual processes and channels
* `nextflow.config` - The configuration file for parameters, profiles etc. For more info read [here](https://www.nextflow.io/docs/latest/config.html#configuration-file)

### Workflow layout

First, we need to get an idea about what the data flow will be and what software and scripts will be run on it. I have outline the basic workflow of `salmon-nf` below:

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/Salmon-pipeline/salmon-nf.png" alt="salmon-nf">

We will only have one single process `salmon` which will use the input `fastq` files and the respective transcriptome `index` file to produce our expression estimates and the pseudo-bam files of aligning reads.

So for our `salmon` process we will have 2 input channels:

* `fastqChannel` - feeding in our raw reads in `fastq` format
* `indexChannel` - providing our transcriptome `index` to which we align the reads to

Our `salmon` process will produce several output files of which we choose to feed 2 file types into output processes as our final results:

* `quant.sf` files via the `salmonChannel` output channel
* `pseudo.bam` files via the `pseudoBamChannel` output channel

Now let's have a look how we can actually realize and implement this on the coding end.

### Docker container

Before we can run anything, we need to provide the software environment containing **all** dependencies and software packages our `salmon` process will run. These days, this is usually done via a [Docker](https://www.docker.com/) container, or a [Singularity](https://singularity.lbl.gov/) container on HPC environments.

Many software packages - including Salmon in our case - usually provide already read-to-use Docker containers (`combinelab/salmon`). But even if they don't, do not despair and brainlessly jump into creating your own containers. If the packages was provided via [BioConda](https://bioconda.github.io/), you will find a Docker container on [BioContainers](https://quay.io/organization/biocontainers). I found this last resort to work in many cases.

Either way, since I wanted to convert the raw `SAM` output from `salmon` into a compressed `BAM` file, I chose to extend their Docker image with adding `samtools` as shown in the [Dockerfile](https://docs.docker.com/engine/reference/builder/) below.

```bash
# Copyright (c) 2019 Tobias Neumann.
#
# You should have received a copy of the GNU Affero General Public License
# along with this program.  If not, see <http://www.gnu.org/licenses/>.

FROM combinelab/salmon:0.12.0

MAINTAINER Tobias Neumann <tobias.neumann.at@gmail.com>

RUN buildDeps='wget ca-certificates make g++' \
    runDeps='zlib1g-dev libncurses5-dev unzip gcc' \
    && set -x \
    && apt-get install -y $buildDeps $runDeps --no-install-recommends \
    && rm -rf /var/lib/apt/lists/* \
    && wget https://github.com/samtools/samtools/releases/download/1.9/samtools-1.9.tar.bz2 \
    && tar xvfj samtools-1.9.tar.bz2 \
    && cd samtools-1.9 \
    && ./configure --prefix=/usr/local/ \
    && make \
    && make install \
    && apt-get purge -y --auto-remove $buildDeps
```

The resulting Docker image was pushed to [Docker Hub](https://hub.docker.com/) and can be pulled via `docker pull obenauflab/salmon:latest`.

### main.nf

Now we are ready to create the central `main.nf` file which contains all processes as well as channels. As mentioned before, you will find the entire code on [GitHub]((https://github.com/t-neumann/salmon-nf)), so here is an excerpt of the important sections.

##### `fastqChannel`

```java
pairedEndRegex = params.inputDir + "/*_{1,2}.fq.gz"
SERegex = params.inputDir + "/*[!12].fq.gz"

pairFiles = Channel.fromFilePairs(pairedEndRegex)
singleFiles = Channel.fromFilePairs(SERegex, size: 1){ file -> file.baseName.replaceAll(/.fq/,"") }

singleFiles.mix(pairFiles)
.set { fastqChannel }
```

This elaborate chunk of code is needed to enable the `fastqChannel` input channel to our `salmon` process to handle both single- and paired-end `fastq` files. As you can see, we created a `pairFiles` channel with a paired-end regex basically assuming that our read-pairs are named `*_1.fq.gz` and `*_2.fq.gz`. In addition, we have a `singleFiles` channel that takes all `fastq` files not following the `_1` and `_2` naming convention and assuming it is single-end read files.

The `fromFilePairs` method creates a channel emitting the file pairs matching the regex we provided. The matching files are emitted as tuples in which the first element is the grouping key of the matching pair and the second element is the list of files (sorted in lexicographical order). For example:

```java
[0399ad16-816f-4824-ae28-7b82e006e7b7_gdc_realn_rehead, [0399ad16-816f-4824-ae28-7b82e006e7b7_gdc_realn_rehead_1.fq.gz, 0399ad16-816f-4824-ae28-7b82e006e7b7_gdc_realn_rehead_2.fq.gz]]
[0ac6634e-00b0-4107-a5d6-db8ffc602645_gdc_realn_rehead, [0ac6634e-00b0-4107-a5d6-db8ffc602645_gdc_realn_rehead_1.fq.gz, 0ac6634e-00b0-4107-a5d6-db8ffc602645_gdc_realn_rehead_2.fq.gz]]
```

As you can see, for the single-end reads channel `singleFiles`, the method is slightly extended:

First, we set an additional parameter `size: 1` to set the number of files each emitted item is expected to hold to 1. In additional, we manually provide the a custom grouping strategy in the closure, which based on the current file as parameter, returns the grouping key. In our case, we simply strip anything from the file name after `.fq` and use this as our grouping key. For example:

```java
[0fdb3d0e-e405-4e8d-8897-4a90ea4fe00c_gdc_realn_rehead, [0fdb3d0e-e405-4e8d-8897-4a90ea4fe00c_gdc_realn_rehead.fq.gz]]
[1916abcd-61c0-4f23-96ac-be70aacb8dc1_gdc_realn_rehead, [1916abcd-61c0-4f23-96ac-be70aacb8dc1_gdc_realn_rehead.fq.gz]]
```

Finally, we combined both channels via a `mix` operator into our final `fastqChannel` input channel to our `salmon` process.

##### `indexChannel`

```java
indexChannel = Channel
	.fromPath(params.salmonIndex)
	.ifEmpty { exit 1, "Salmon index not found: ${params.salmonIndex}" }
```

This input channel is pretty straightforward set up. Only thing we need to do is to precreate our Salmon index (read how to do this [here](https://salmon.readthedocs.io/en/latest/salmon.html#preparing-transcriptome-indices-mapping-based-mode)) and supply it via the `salmonIndex` parameter - how this is done will follow later.

##### Process `salmon`

```java
process salmon {

	tag { lane }

    input:
    set val(lane), file(reads) from fastqChannel
    file index from indexChannel.first()

    output:
    file ("${lane}_salmon/quant.sf") into salmonChannel
    file ("${lane}_pseudo.bam") into pseudoBamChannel

    shell:

    def single = reads instanceof Path

    if (!single)

      '''
      salmon quant -i !{index} -l A -1 !{reads[0]} -2 !{reads[1]} -o !{lane}_salmon -p !{task.cpus} --validateMappings --no-version-check -z | samtools view -Sb -F 256 - > !{lane}_pseudo.bam
	    '''
    else
      '''
      salmon quant -i !{index} -l A -r !{reads} -o !{lane}_salmon -p !{task.cpus} --validateMappings --no-version-check -z | samtools view -Sb -F 256 - > !{lane}_pseudo.bam
	    '''

}
```

Our only process for the `salmon-nf` workflow is the `salmon` process.

You will notice that it has the 2 input channels we previously defined - `fastqChannel` and `indexChannel`. Note, how we have to use the `.first()` method on the `indexChannel` since it is a folder.

In addition, we have defined 2 output channels - `salmonChannel` outputting all `quant.sf` files and `pseudoBamChannel` outputting the corresponding `pseudo.bam` files.

The actual script that is run, is a plain conditional bash script. We have an initial condition that asks whether we have single read files coming in from the `fastqChannel` or paired-end reads - and based on this evaluation will run one or the other script branch.

The bash script itself is then basically only a `salmon` call on the respective input files.


This input channel is pretty straightforward set up. Only thing we need to do is to precreate our Salmon index (read how to do this [here](https://salmon.readthedocs.io/en/latest/salmon.html#preparing-transcriptome-indices-mapping-based-mode)) and supply it via the `salmonIndex` parameter - how this is done will follow later.

### nextflow.config

### Profiles
