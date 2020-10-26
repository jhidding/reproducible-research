---
title: Reproducible Research
---

# Motivation

## A typical sitation
![PhD Comic](./img/research_comic_phd.gif)

## Discussion: A scary anecdote
> - A group of researchers obtain great results and submit their work to a high-profile journal.
> - Reviewers ask for new figures and additional analysis.
> - The researchers start working on revisions and generate modified figures, but find inconsistencies with old figures.
> - The researchers can't find some of the data they used to generate the original results, and
>   can't figure out which parameters they used when running their analyses.

## Discussion: continued ...
> - The manuscript is still languishing in the drawer ...

# What is reproducible research?

## Definition
::: {style="font-size: smaller"}
> "reproducibility refers to the ability of a researcher to **duplicate the
> results** of a prior study using the same materials as were used by the
> original investigator. That is, a second researcher might use the same raw
> data to build the same analysis files and implement the same statistical
> analysis in an attempt to yield the same results. Reproducibility is a
> **minimum necessary condition** for a finding to be believable and informative."
>
> -- <cite> U.S. National Science Foundation (NSF) subcommittee on replicability in science</cite>
:::

## Definition
- For any research project, an independent researcher should be able to replicate an experiment:
  - the same results should be obtained under the same contitions
  - it should be possible to recreate the same conditions
- "Experiment" is interpreted in a wide sense, encompassing also computational work

## The Reproducibility Crisis
![](./img/reproducibility_nature.jpg){.stretch}

::: {.notes}
A 2016
[survey](http://www.nature.com/news/1-500-scientists-lift-the-lid-on-reproducibility-1.19970)
in Nature revealed that irreproducible experiments are a problem across all
domains of science.
:::

## Irreproducible research: why?
- Not enough documentation on how experiment is conducted and data is generated
- Data used to generate original results unavailable
- Software used to generate original results unavailable
- Difficult to recreate software environment (libraries, versions) used to generate original results
- Difficult to rerun the computational steps

## Or in short
![](./img/Miracle.jpg){.stretch}

## Levels of reproducibility
::: {.notes}
A published article is like the top of a pyramid. It rests on multiple
levels that each contributes to its reproducibility.
:::

![](./img/repro-pyramid.png){.stretch}

## Semantics
![CC-BY Scriberia](./img/turing-way/39-reproducible-replicable-robust-generalisable.jpg){.stretch}

::: {.notes}
(This image was created by [Scriberia](http://www.scriberia.co.uk) for [The
Turing Way](https://the-turing-way.netlify.com) community and is used under a
CC-BY licence. The image was obtained from [https://zenodo.org/record/3332808](https://zenodo.org/record/3332808).)

While reproducibility is the minimum requirement and can be solved with "good enough" computational practices, replicability/robustness/generalisability of scientific findings are an even greater concern involving research misconduct, questionable research practices (p-hacking, HARKing, cherry-picking), sloppy methods, and other conscious and unconscious biases. 
:::

## Discussion
> Computer programs are expected to produce the same
> output for the same inputs. Is
> that true for research software?
>
> Can you give some examples? What can we do about it?

# Organization Projects
## Lesson page
[Code Refinery/Reproducible Research/Organizing Projects](https://coderefinery.github.io/reproducible-research/02-organizing-projects/)

# Recording Dependencies
## Dependencies
- **Reproducibility**: We can control our code but how can we control dependencies?
- **10-year challenge**: Try to build/run your own code that you have created 10 (or less) years ago. Will your code
  from today work in 5 years if you don't change it?
- **Dependency hell**: Different codes on the same environment can have conflicting dependencies.

## Dependency Hell
[![](./img/python_environment.png)](https://xkcd.com/1987/)

## Lesson page
[Code Refinery/Reproducible Research/Recording Dependencies](https://coderefinery.github.io/reproducible-research/03-dependencies/)

# Recording Environments
## Containers
- Containers can be built to bundle *all the necessary ingredients* (data, code, environment).
- A container provides operating-system-level virtualization, sharing the host system’s kernel with other containers.
- Popular container implementations are [Docker](https://www.docker.com/) and [Singularity](http://singularity.lbl.gov/).

## Docker
- Available for most common operating systems.
- A mechanism to "send the computer to the data" when data is too
  large or too sensitive to travel over network.
- [DockerHub](https://hub.docker.com/) is a platform to share Docker images (stored in repositories - similar to a Git repository).
- Public Docker images available on [DockerHub](https://hub.docker.com/).

## Warning
> Not all images can be trusted! There have been examples of contaminated
> images so investigate before using images blindly. Apply same caution as installing
> software packages from untrusted package repositories.

## Semantics
- Image is like a blueprint. It is immutable.
- Container is an instance of an image.
- Dockerfile is a recipe which creates a container based on an image and applies small changes to it.

## Pros
- Allow for seamlessly moving workflows across different platforms.
- Much more lightweight than virtual machines.
- Eliminates the "works on my machine" situation.
- For software with many dependencies, in turn with its own dependencies,
  containers offer possibly the only way to preserve the
  computational experiment for future reproducibility.

## Cons
- Containers can have security vulnerabilities which can be exploited.
- Can be used to hide away software installation problems and thereby
  discourage good software development practices.
- It may not be clear whether to record the environment in the image part or the recipe part.

## Discussion: reproducibility
> - Do you think containers contribute to reproducible research?
> - Do you see a use case for your own work?

<!-- ## Lesson page
[Code Refinery/Reproducible Research/Recording Environments](https://coderefinery.github.io/reproducible-research/04-environments/) -->

# Recording Computational Steps
## Lesson page
[Code Refinery/Reproducible Research/Recording Computational Steps](https://coderefinery.github.io/reproducible-research/05-workflow-management/)

# Open Science
## Sharing research data
The Open Science movement encourages researchers to share research output beyond the contents of a published academic article (and possibly supplementary information).

## In favor
Arguments in favor [(from Wikipedia)](https://en.wikipedia.org/wiki/Open_science):

- Open access publication of research reports and data allows for rigorous peer-review
- Science is publicly funded so all results of the research should be publicly available
- Open Science will make science more reproducible and transparent
- Open Science has more impact
- Open Science will help answer uniquely complex questions

## Against
Arguments against [(from Wikipedia)](https://en.wikipedia.org/wiki/Open_science):

- Too much unsorted information overwhelms scientists
- Potential misuse
- The public will misunderstand science data
- Increasing the scale of science will make verification of any discovery more difficult
- Low-quality science

## Data cycle
![CC-BY Scriberia](./img/turing-way/36-data-research-cycle.jpg){.stretch}

::: {.notes}
(This image was created by [Scriberia](http://www.scriberia.co.uk) for [The
Turing Way](https://the-turing-way.netlify.com) community and is used under a
CC-BY licence. The image was obtained from [https://zenodo.org/record/3332808](https://zenodo.org/record/3332808).)
:::

# FAIR 
## FAIR principles
![CC-BY Scriberia](./img/turing-way/8-fair-principles.jpg){.stretch}

::: {.notes}
(This image was created by [Scriberia](http://www.scriberia.co.uk) for [The
Turing Way](https://the-turing-way.netlify.com) community and is used under a
CC-BY licence. The image was obtained from [https://zenodo.org/record/3332808](https://zenodo.org/record/3332808).)

"FAIR" is the current buzzword for data management. You may be asked
about it in, for example, making data management plans for grants:
:::

## Findable
- Will anyone else know that your data exists?
- Solutions: put it in a standard repository, or at least a
  description of the data. Get a digital object identifier (DOI).

## Accessible
- Once someone knows that the data exists, can they get it?
- Usually solved by being in a repository, but for non-open data,
  may require more procedures.

## Interoperable
- Is your data in a format that can be used by others, like csv
  instead of PDF?
- Or better than csv. Example: [5-star open data](https://5stardata.info/en/)

## Reusable
- Is there a license allowing others to re-use?

## FAIR `!=` Open
Even though this is usually referred to as "open data", it means
considering and making good decisions, even if non-open.

Note that FAIR principles do not require data/software to be open.

## FAIR Software
FAIR principles are usually discussed in the context of data,
but they apply also for research software.

See: [5 recommendation for FAIR software: fair-software.nl](http://fair-software.nl/)

## Discussion: Discuss open science
- Do you share any other research outputs besides published articles and possibly source code?
- Discuss pros and cons of sharing research data.

## Exercise: Get a DOI at Zenodo
1. Go to [https://sandbox.zenodo.org/login/](https://sandbox.zenodo.org/login/) and sign in with your GitHub account.
2. Go to [https://sandbox.zenodo.org/account/settings/github/](https://sandbox.zenodo.org/account/settings/github/).
3. Find the repository you wish to publish, and flip the switch to ON.
4. Go to GitHub and create a **release**.
5. Add the DOI badge to your README.

::: {.notes}
Digital object identifiers (DOI) are the backbone of the academic
reference and metrics system. In this exercise we will see how to
make a GitHub repository citable by archiving it on the
[Zenodo](http://about.zenodo.org/) archiving service. Zenodo is a
general-purpose open access repository created by OpenAIRE and CERN.
:::

## See lesson page
[Code Refinery: Reproducible Research](https://coderefinery.github.io/reproducible-research/06-sharing/#services-for-sharing-and-collaborating-on-research-data)