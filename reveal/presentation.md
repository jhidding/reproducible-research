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

# The Reproducibility Crisis

## Why all the talk?
![](./img/reproducibility_nature.jpg)

::: {.notes}
A 2016
[survey](http://www.nature.com/news/1-500-scientists-lift-the-lid-on-reproducibility-1.19970)
in Nature revealed that irreproducible experiments are a problem across all
domains of science.
:::

## Factors behind irreproducible research

- Not enough documentation on how experiment is conducted and data is generated
- Data used to generate original results unavailable
- Software used to generate original results unavailable
- Difficult to recreate software environment (libraries, versions) used to generate original results
- Difficult to rerun the computational steps

## Or in short
![](./img/Miracle.jpg){.stretch}

---

## Levels of reproducibility
::: {.notes}
A published article is like the top of a pyramid. It rests on multiple
levels that each contributes to its reproducibility.
:::

![](./img/repro-pyramid.png){.stretch}

# Reproducible, Replicable, Robust, Generalisable

## Semantics
![CC-BY Scriberia](./img/turing-way/39-reproducible-replicable-robust-generalisable.jpg){.stretch}

::: {.notes}
(This image was created by [Scriberia](http://www.scriberia.co.uk) for [The
Turing Way](https://the-turing-way.netlify.com) community and is used under a
CC-BY licence. The image was obtained from [https://zenodo.org/record/3332808](https://zenodo.org/record/3332808).)

While reproducibility is the minimum requirement and can be solved with "good enough" computational practices, replicability/robustness/generalisability of scientific findings are an even greater concern involving research misconduct, questionable research practices (p-hacking, HARKing, cherry-picking), sloppy methods, and other conscious and unconscious biases. 
:::

## Discuss with your neighbors or among all participants
> Computer programs are expected to produce the same
> output for the same inputs. Is
> that true for research software?
>
> Can you give some examples? What can we do about it?
