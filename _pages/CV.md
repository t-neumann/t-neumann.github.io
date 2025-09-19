---
layout: single
author_profile: true
permalink: /CV/
order: 1
---
<div class="card-columns only-one-column">

  <div class="card">
    <div class="card-text text-muted alert alert-dark">
      <a href="#experience">Experience</a> •
      <a href="#education">Education</a> •
      <a href="#skills">Computer skills</a> •
      <a href="#languages">Languages</a> •
      <a href="#interests">Interests</a>
    </div>
  </div>

  <div class="card">
    <div class="card-text text-muted alert alert-dark">
      <i class="fa fa-download" aria-hidden="true"></i> <a href="/assets/mycv/TobiasNeumann_CV_latest.pdf" target="_blank">Download a pdf version</a>
    </div>
  </div>

  <div class="card">
    <a name="experience"></a>
    <div class="card-header h4">
      <i class="fa fa-building" aria-hidden="true"></i> Experience
      <div class="float-right"><a href="#top">&uarr;</a></div>
    </div>
    <div class="container">
    <div class="row">
      <div class="col-md-2">
        <img src="{{ site.url }}{{ site.baseurl }}/assets/images/QUANTRO_LOGO.png" alt="Quantro Logo">
        From April 2024
      </div>
      <div class="col">
    <strong>Group Leader Computational Science</strong><br>
    <em>Vienna, Austria</em><br>
    <p class="text-muted" style="font-size: 90%;">
      Building and leading the multidisciplinary Computational Science team with PhDs in Physics and Computer Science. 
      Focusing on computational platform and infrastructure setup, statistics method development, and AI/ML strategy of the drug discovery R&amp;D and chemistry department.
    </p>
    <ul>
      <li><strong>Team Leadership &amp; Agile Working:</strong> Introduced agile principles and SCRUM-inspired workflows to improve delivery and collaboration.
        <ul>
          <li>Built and led a multidisciplinary team of PhDs in Computer Science and Physics.</li>
          <li>Reorganized workflows into sprint-based iterations, allowing faster adjustment of research directions.</li>
          <li>Established a blueprint framework with training pipelines, tracking, and deployment templates to standardize development and ensure production-ready code.</li>
        </ul>
      </li>
      <li><strong>AI/ML Platform &amp; Strategy:</strong> Shaped and executed the company’s AI/ML strategy, transforming QUANTRO's drug discovery platform.
        <ul>
          <li>Conceptualized a hybrid <em>in silico</em> drug discovery platform hosting models that support both the time-resolved HTS platform and MedChem parameters.</li>
          <li>Enabled the transition from traditional DMTA cycles to DMTL cycles, where predictive computational models accelerate discovery and reduce cost.</li>
          <li>Prototyped and directed development of deep learning models predicting time-resolved transcriptomic responses from chemical structures, leveraging large-scale proprietary datasets.</li>
        </ul>
      </li>
      <li><strong>Computational Infrastructure &amp; CI/CD:</strong> Built and optimized QUANTRO’s computational ecosystem to ensure reliability, scalability, and automation.
        <ul>
          <li>Standardized CI/CD pipelines across all major computational workflows.</li>
          <li>Enabled 24/7 automated testing and monitoring, reducing risk of downtime and errors.</li>
          <li>Delivered faster development cycles with frequent code integration, early testing, and rapid rollback capability.</li>
          <li>Lowered operational costs by freeing data scientists from maintenance through automated deployment.</li>
        </ul>
      </li>
      <li><strong>Data Analysis Life Cycles:</strong> Designed reproducible data analysis frameworks ensuring transparency and rapid insight generation.
        <ul>
          <li>Established artifact-tracked life cycles with QR code–based traceability.</li>
          <li>Integrated version-controlled releases to distinguish pilot (unreviewed) from production (reviewed) results.</li>
          <li>Reduced turnaround time for R&amp;D insights from weeks to days, improving scientific decision-making speed.</li>
        </ul>
      </li>
    </ul>
  </div>
    </div>
    <hr>
    <div class="row">
      <div class="col-md-2">
      <img src="{{ site.url }}{{ site.baseurl }}/assets/images/IMP_Logo.png" alt="IMP Logo">
        2014-2020
      </div>
      <div class="col">
        Senior Bioinformatician at the <a href="https://www.imp.ac.at/" target="blank">IMP Research Institute for Molecular Pathology</a>
        <div style="line-height:100%;">
          <br>
        </div>
        <ul>
        <li><span><i class="mdi mdi-clipboard-check-outline"></i></span>Lead bioinformatics method and algorithm development for the <a href="https://www.nature.com/articles/nmeth.4435">SLAM-seq</a> sequencing technology.</li>
        <li><span><i class="mdi mdi-clipboard-check-outline"></i></span>Development a cloud-computing <a href="https://github.com/ObenaufLab/virus-detection-nf">virus screening pipeline</a> on <a href="https://aws.amazon.com/">Amazon Web Services (AWS)</a> that scales to big data, analysing the entire 11,000 patient samples (100 TB raw data) of <a href="https://www.cancer.gov/about-nci/organization/ccg/research/structural-genomics/tcga">The Cancer Genome Atlas</a> <i class="fab fa-aws" aria-hidden="true"></i>.</li>
        <li><span><i class="mdi mdi-finance"></i></span>Design of the genome-wide <a href="https://static-content.springer.com/esm/art%3A10.1038%2Fs41592-020-0850-8/MediaObjects/41592_2020_850_MOESM3_ESM.xlsx" target="blank">Vienna sgRNA libraries</a> for pooled CRISPR screens.</li>
        <li><span><i class="mdi mdi-finance"></i></span>Co-development of the <a href="https://www.vbc-score.org" target="blank">VBC score</a> for selection of sgRNAs that effectively produce loss-of-function alleles.</li>
        <li><span><i class="mdi mdi-server"></i></span>Technology development on the <a href="https://nanoporetech.com">Oxford Nanopore Sequencing</a> platform to detect translocations at single-base pair resolution genome-wide.</li>
        <li><span><i class="mdi mdi-finance"></i></span>Member of the
        <a href="https://www.maxperutzlabs.ac.at/vcdi" target="blank">Vienna Covid-19 Detection Initiative (VCDI)</a> performing comparative genomics of &gt; 5M sequenced SARS-CoV-2 genomes worldwide to identify conserved domains for sensitive primer design to detect COVID-19 infections, de-novo assembly and quantification of viral RNAs for primer target selection for sensitive SARS-CoV-2 detection.</li>
        <li><span><i class="mdi mdi-finance"></i></span>Project supervision and mentoring of Bioinformatics Master students:</li>
        <ul>
        <li><span><i class="mdi mdi-clipboard-check-outline"></i></span>CRISPRepo: database and front-end development for integrating, visualizing and querying CRISPR screening data and metadata.</li>
        <li><span><i class="mdi mdi-clipboard-check-outline"></i></span>Characterization of replication origins: Method development for and integration of DNA replication origin mapping (SNS-seq), replication timing sequencing (RepliSeq), genome-wide chromatin contact mapping (HiC) and ChIP-Seq data.</li>
        </ul>
        </ul>
      </div>
    </div>
    <hr>
      <div class="row">
        <div class="col-md-2">
          <img src="{{ site.url }}{{ site.baseurl }}/assets/images/lexogen_logo.png" alt="Lexogen Logo">
          2017-2019
        </div>
        <div class="col">
          Contractor for <a href="https://www.lexogen.com/" target="blank">Lexogen GmbH</a>
          <div style="line-height:100%;">
            <br>
          </div>
          <ul>
          <li><span><i class="mdi mdi-clipboard-check-outline"></i></span>Lead development of the <a href="https://www.lexogen.com/store/slamdunk-data-analysis-pipeline">SLAMdunk</a> product backend.</li>
          <li><span><i class="mdi mdi-clipboard-check-outline"></i></span>Optimizing and porting established variant-callers for nucleotide-conversion detection.</li>
          <li><span><i class="mdi mdi-finance"></i></span>Containerization of so ware packages using Docker.</li>
          <li><span><i class="mdi mdi-finance"></i></span>Deployment of Docker containers on the <a href="https://www.bluebee.com/">Bluebee</a> private cloud platform <i class="fas fa-cloud" aria-hidden="true"></i>.</li>
          <li><span><i class="mdi mdi-server"></i></span>License assessment and resource benchmarking <i class="fas fa-balance-scale" aria-hidden="true"></i>.</li>
          </ul>
        </div>
      </div>
      <hr>
      <div class="row">
        <div class="col-md-2">
          <img src="{{ site.url }}{{ site.baseurl }}/assets/images/sophia_genetics_logo.png" alt="Sophia Genetics Logo">
          2012-2014
        </div>
        <div class="col">
          Bioinformatician at <a href="https://www.sophiagenetics.com" target="_blank">Sophia Genetics SA</a>
          <div style="line-height:100%;">
            <br>
          </div>
          <ul>
          <li><span><i class="mdi mdi-clipboard-check-outline"></i></span>Lead development, maintenance and optimization of a targeted gene sequencing and exome-sequencing pipeline building system forming the bioinformatics backend of the Sophia DDM &reg; SaaS platform <i class="fas fa-diagnoses" aria-hidden="true"></i>.</li>
          <li><span><i class="mdi mdi-clipboard-check-outline"></i></span>Tailoring pipelines for diagnostic gene panel kits on various sequencing platforms (Roche 454 / ionTorrentPGM / Illumina MiSeq) for major hospitals and labs in Switzerland and across Europe <i class="fas fa-hospital" aria-hidden="true"></i>.</li>
          <li><span><i class="mdi mdi-finance"></i></span>Conducting genetic test kit validation studies with key diagnostic kit developer sat European level <i class="fas fa-globe" aria-hidden="true"></i>.</li>
          <li><span><i class="mdi mdi-finance"></i></span>Product presentations for prospective and established customers and on conferences <i class="fab fa-slideshare" aria-hidden="true"></i>.</li>
          </ul>
        </div>
      </div>
      <hr>
      <div class="row">
        <div class="col-md-2">
          <img src="{{ site.url }}{{ site.baseurl }}/assets/images/MPL_logo.jpg" alt="MPL Logo">
          2012
        </div>
        <div class="col">
          Bioinformatician at the <a href="https://www.maxperutzlabs.ac.at/" target="_blank">Max Perutz Laboratories</a>
          <div style="line-height:100%;">
            <br>
          </div>
          <ul>
          <li><span><i class="mdi mdi-clipboard-check-outline"></i></span>De-novo genome assembly of <i>Clunio marinus</i>:</li>
          <ul>
            <li><span><i class="mdi mdi-clipboard-check-outline"></i></span>Contig assembly, filtering and completeness assessment</li>
            <li><span><i class="mdi mdi-clipboard-check-outline"></i></span>Setup of the de-novo gene annotation pipeline and genome browser (<a href="http://www.yandell-lab.org/software/maker.html">MAKER</a>,<a href="http://gmod.org/wiki/Main_Page">GMOD</a>)</li>
          </ul>
          <li><span><i class="mdi mdi-clipboard-check-outline"></i></span>Scaffold N50 of 1.9Mb, 98 &percnt; completeness.</li>
          <li><span><i class="mdi mdi-finance"></i></span>Better assembly quality than honeybee, <i>Tribolium</i> or the monarch butterly.</li>
          <li><span><i class="mdi mdi-finance"></i></span>Served as basis to unravel the genomic basis of circadian and circalunar timing adaptations.</li>
          <li><span><i class="mdi mdi-finance"></i></span>Hosted at <a href="http://cluniobase.cibiv.univie.ac.at/">ClunioBase</a> and published in <i>Nature</i>.</li>
          </ul>
        </div>
      </div>
      <hr>
      <div class="row">
        <div class="col-md-2">
          <img src="{{ site.url }}{{ site.baseurl }}/assets/images/CIBIV_logo.png" alt="CIBIV Logo">
          2010-2012
        </div>
        <div class="col">
          Project student at the <a href="http://www.cibiv.at/" target="_blank">Center for Integrative Bioinformatics Vienna</a>
          <div style="line-height:100%;">
            <br>
          </div>
          <ul>
          <li><span><i class="mdi mdi-clipboard-check-outline"></i></span>Massive parallelization of sequence  alignments on the CPU and cluster <br/>
          <i class="fab fa-github" aria-hidden="true"></i> <a href="https://github.com/t-neumann/versalignLib">https://github.com/t-neumann/versalignLib</a>
          </li>
          <li><span><i class="mdi mdi-clipboard-check-outline"></i></span>Software development of an evaluation framework for reference-mapping software.</li>
          <li><span><i class="mdi mdi-finance"></i></span>De-novo transcriptome assembly of RNA-seq data for <i>Idiosepius pygmaeus</i>.</li>
          </ul>
        </div>
      </div>
      <hr>
      <div class="row">
        <div class="col-md-2">
          <i class="fa fa-calendar" aria-hidden="true"></i> 2009
        </div>
        <div class="col">
          Research intern at the <a href="http://www.sbc.su.se/" target="_blank">SBC Stockholm Bioinformatics Center</a>
          <div style="line-height:100%;">
            <br>
          </div>
          <ul>
          <li><span><i class="mdi mdi-clipboard-check-outline"></i></span>Network analysis of the gene interaction database <a href="http://funcoup.sbc.su.se/search">FunCoup</a> to in-silico identify and evaluate novel neurodegenerative disease candidate genes.</li>
          </ul>
        </div>
      </div>
    </div>
  </div>

  <div class="card">
    <a name="education"></a>
    <div class="card-header h4">
      <i class="fa fa-university" aria-hidden="true"></i> Education
      <div class="float-right"><a href="#top">&uarr;</a></div>
    </div>
    <div class="container">
      <div class="row">
        <div class="col-md-2">
          <i class="fa fa-calendar" aria-hidden="true"></i> 2020-2023
        </div>
        <div class="col">
          PhD in Molecular Biosciences, <a href="http://www.cibiv.at" target="_blank">Arndt von Haeseler Group</a>, University Vienna
        </div>
      </div>
      <hr>
      <div class="row">
        <div class="col-md-2">
          <i class="fa fa-calendar" aria-hidden="true"></i> 2009-2012
        </div>
        <div class="col">
          Master’s Degree in Bioinformatics, <a href="https://www.meduniwien.ac.at" target="_blank">Medical University Vienna</a>
        </div>
      </div>
      <hr>
      <div class="row">
        <div class="col-md-2">
          <i class="fa fa-calendar" aria-hidden="true"></i> 2009
        </div>
        <div class="col">
          Erasmus Semester at the <a href="http://www.sbc.su.se" target="_blank">Stockholm Bioinformatics Center</a>, Stockholm, Sweden
        </div>
      </div>
      <hr>
      <div class="row">
        <div class="col-md-2">
          <i class="fa fa-calendar" aria-hidden="true"></i> 2006-2009
        </div>
        <div class="col">
          Bachelor’s Degree in Bioinformatics, <a href="https://www.fh-ooe.at/campus-hagenberg" target="_blank">University for Applied Sciences Hagenberg</a>
        </div>
      </div>
    </div>
  </div>

  <div class="card">
    <a name="skills"></a>
    <div class="card-header h4">
      <i class="fa fa-laptop" aria-hidden="true"></i> Computers skills
      <div class="float-right"><a href="#top">&uarr;</a></div>
    </div>
    <div class="container">
      <div class="row">
        <div class="col-md-3">
          • Programming
        </div>
        <div class="col-md-3">
          C/C++, Java
        </div>
        <div class="col-md-3">
          • Scripting
        </div>
        <div class="col-md-3">
          Python, R/Bioconductor, Bash, Perl
        </div>
      </div>
      <hr>
      <div class="row">
        <div class="col-md-3">
          • Versioning
        </div>
        <div class="col-md-3">
          Git, SVN
        </div>
        <div class="col-md-3">
          • Containers
        </div>
        <div class="col-md-3">
          Docker, Singularity
        </div>
      </div>
      <hr>
      <div class="row">
        <div class="col-md-3">
          • Platforms
        </div>
        <div class="col-md-3">
          Linux, Mac OS X, AWS, Windows
        </div>
        <div class="col-md-3">
          • Building
        </div>
        <div class="col-md-3">
          CMake, Maven
        </div>
      </div>
      <hr>
      <div class="row">
        <div class="col-md-3">
          • Frameworks
        </div>
        <div class="col-md-3">
          EJB / CDI, Hibernate / JPA, JSF + PrimeFaces
        </div>
        <div class="col-md-3">
          • Parallelization
        </div>
        <div class="col-md-3">
          SSE/AVX, OpenCL, OpenMP, MPI
        </div>
      </div>
    </div>
  </div>

  <div class="card">
    <a name="languages"></a>
    <div class="card-header h4">
      <i class="fa fa-comment" aria-hidden="true"></i> Languages
      <div class="float-right"><a href="#top">&uarr;</a></div>
    </div>
    <div class="container">
      <div class="row">
        <div class="col-md-2">
          • German
        </div>
        <div class="col-md-2">
          Native
        </div>
        <div class="col-md-2">
          • English
        </div>
        <div class="col-md-2">
          Full professional proficiency
        </div>
        <div class="col-md-2">
          • Spanish
        </div>
        <div class="col-md-2">
          Elementary proficiency
        </div>
      </div>
    </div>
  </div>

  <div class="card">
    <a name="interests"></a>
    <div class="card-header h4">
      <i class="fa fa-info-circle" aria-hidden="true"></i> Interests
      <div class="float-right"><a href="#top">&uarr;</a></div>
    </div>
    <div class="container">
    <div class="row">
      <div class="col-md-2">
        Sports
      </div>
      <div class="col">
        Beachvolleyball, Cycling, Bouldering, Slacklining, Badminton
      </div>
    </div>
    <hr>
    <div class="row">
      <div class="col-md-2">
        Music
      </div>
      <div class="col">
        Piano, Guitar, Drums
      </div>
    </div>
    <hr>
    <div class="row">
      <div class="col-md-2">
        Other
      </div>
      <div class="col">
        Reading news, languages, travelling
      </div>
    </div>
    </div>
  </div>

</div>
