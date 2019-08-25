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

### Workflow layout

### main.nf

### nextflow.config

### Dockerfile

### Profiles
