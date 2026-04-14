EuroBioc 2026 Hackathon
================

- [Introduction](#introduction)
- [Documents and communication](#documents-and-communication)
  - [Remote participation](#remote-participation)
  - [Location](#location)
  - [Cloud compute resources](#cloud-compute-resources)
- [Projects](#projects)
  - [R/CUDA bindings](#rcuda-bindings)
  - [Commandline executables for bioconductor functions and
    scripts](#commandline-executables-for-bioconductor-functions-and-scripts)
  - [Lightweight container generation for bioconductor
    packages](#lightweight-container-generation-for-bioconductor-packages)
  - [Tidyomics / Spatialomics](#tidyomics--spatialomics)
  - [General bughunt](#general-bughunt)
- [Other things!](#other-things)

**Bioconductor centric hackathons!**

# Introduction

A general bioconductor-centric hackathon is being planned for June 1st
and 2nd before [EuroBioc2026](https://eurobioc2026.bioconductor.org/) in
Turku, Finland. A few projects are already planned, but participants are
welcome to bring their own as long as those ideas have been communicated
to organizers beforehand. General bug hunting or feature additions in
existing packages that are willing to accept pull requests will also be
supported.

Participants will be expected to adhere to the [Bioconductor code of
conduct](https://bioconductor.github.io/bioc_coc_multilingual/).

If you are interested in participating, [please fill out this
form](https://forms.gle/wX8bzq1jdtPuZ1yX6).

# Documents and communication

This README page will serve as a hub for documents, repos, and links
associated with the event.

## Remote participation

Information on remote participation will be included here.

## Location

The event will take place at [BioCity Turku](https://biocityturku.fi/),
the same venue as EuroBioc2026. Room numbers TBA.

## Cloud compute resources

We have credits on [NVIDIA’s brev](https://developer.nvidia.com/brev)
cloud compute resource for the hackathon. These resources will help
facilitate development for NVIDIA devices for those of us who don’t
already have easy access them. A guide for accessing brev and doing
remote development through RStudio will included here before the event
begins.

# Projects

## R/CUDA bindings

The inaccessibility of alternative compute devices in the R language has
been a missing feature that most R users have been content to live
without. The rugged landscape of alternative compute frameworks and
hardware providers have also made it difficult for the R community to
coalesce on a paradigm for access that would fit well within R as a
language and R as a research ecosystem. Bringing alternative compute
access into R, particularly with CUDA and NVIDIA devices would improve
the value of R as a skill for graduates leaving academia, increase the
throughput and efficiency that research computing in R can accomplish,
and expand the versatility of R generally.

The goal of this project is to build infrastructure for developers who
want their computational tools to be able to leverage available CUDA
devices on systems where they are present. Examples of programmatic
access to
[OpenCL](https://cran.r-project.org/web/packages/OpenCL/index.html) and
[Metal](https://github.com/npcooley/ACFmetal) in R already exist and
contain lessons in terms of design paradigms, reasonable expectations
for users / developers, and maintainability. Ideally this package will
be submitted to CRAN upon completion.

This is not untrodden ground. Attempts have
[been](https://github.com/yuanli22/RCUDA)
[made](https://cran.r-project.org/web/packages/gmatrix/index.html)
[at](https://cran.r-project.org/web/packages/gpuR/index.html)
[providing](https://cran.r-project.org/web/packages/cudaBayesreg/index.html)
[GPU](https://cran.r-project.org/web/packages/gputools/index.html)
support in R before, and of those only Simon Urbanek’s
[OpenCL](https://cran.r-project.org/web/packages/OpenCL/index.html) R
package seems to have survived the test of time as R specific
infrastructure. Several
[existing](https://cran.r-project.org/web/packages/torch/index.html)
[packages](https://cran.r-project.org/web/packages/tensorflow/index.html)
[allow](https://cran.r-project.org/web/packages/keras/index.html)
[GPU](https://cran.r-project.org/web/packages/h2o4gpu/index.html)
support through tools they wrap around, but do not facilitate R
development. Therefore this project will likely be as much about
planning and design as it will be about writing code.

Key considerations:

- What infrastructure is necessary for developers to be able call CUDA
  code in a function when a device is available?
- When should kernel functions be compiled? Package installation?
  Package loading? On request by the user?
- If the end goal is for *developers* to be able to enable alternative
  compute use through a function argument, what do *users* supply to
  that argument, and what do users need to know - or have done - ahead
  of time for this kind of behavior to be reliable.
- The underlying structure of the package must be both maintainable.
- Implementations within the package must also be flexible. We
  realistically shouldn’t expect to find NVIDIA hardware on an Apple OS,
  so developers should be able to rely on tooling within this package
  exiting or warning gracefully in cases where a user asks for resources
  that don’t exist. Developers should also be comfortable having their
  own packages rely on this package, most likely through the `Suggests`
  field in the description file.

## Commandline executables for bioconductor functions and scripts

This project was originally envisioned as a package or function for
converting package maintainer or user supplied scripts into commandline
executables. The R and Bioconductor paradigms of interactivity through a
responsive and informative REPL have been a formula for success for
academic users for a long time. But as computational biology becomes
increasingly interdisciplinary and increasingly relies on high
performance compute, high throughput compute and experimentation, and
cloud compute, formalizing portable and flexible implementations of R
and Bioconductor tooling is an imperative to ensure that that work, and
Bioconductor itself maintain relevancy.

There already exist at least two tools that do ‘app’ style script
conversion; [R2G2 - Galaxy specifc
integration](https://www.biorxiv.org/content/10.64898/2025.12.22.695980v1.full)
and [Rapp - commandline
executables](https://cran.r-project.org/web/packages/Rapp/index.html).
So developing a package for this project may not be necessary. Long
term, the goal of this project is to lay the groundwork for programmatic
generation of tooling within Bioconductor packages that can be slotted
into modern workflow management systems, or other interactive platforms
like Galaxy.

Key Considerations:

- Where is the right place for ‘app-ification’ of a script to take
  place? Should it happen upon package installation?
- Overhead checking of things like package versions or argument types
  are mostly cheap, how much of those checks should we enforce?
- What do graceful failures for these scripts look like?

## Lightweight container generation for bioconductor packages

Containerization of workflow components has become a popular and
successful tool for managing moving parts within complex research
pipelines. When performed appropriately it improves portability,
reliability, and reproducibility. Bioconductor currently supplies a
small set of interactive containers, however that only covers some
container use cases. Being able to offer simple and automatic container
generation for workflow management would help easy the scale up
transitions from small scale and exploratory analyses to large-scale
analytical workflows.

Key considerations:

- How would / should users call on these containers?
- Its likely that containers built on specific r-base versions won’t
  need to be maintained in perpetuity as long as the construction path
  and components remain exposed.

## Tidyomics / Spatialomics

TBA

## General bughunt

Bring your own bugs and fixes.

# Other things!

If you have further questions, please contact Nick Cooley at
Nicholas(dot)cooley(at)ul(dot)ie.
