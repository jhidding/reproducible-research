---
layout: episode
title: "Workflow management tools"
teaching: 15
exercises: 15
questions:
  - "What are scientific workflow management systems?"
  - "Can such tools be adopted to your research?"
objectives:
  - "Get familiar with Snakemake"
keypoints:
  - "Hundreds of workflow management tools have been developed to make research more replicable and reproducible"
  - "Snakemake is a comparatively simple option to create transferable and scalable data analyses"
   
---

## Scientific workflows

- Home-made workflows: scripts that call in data, programs and other inputs and produce outputs
- Make can be used to manage workflows, but it has limitations
- Many specialized frameworks exist for managing scientific workflows
  - user-friendly environment to create workflows
  - automatic job execution
  - interactive tools to execute workflows and view results in real-time
  - enable sharing and reusing workflows
  - enable tracking the provenance of workflow results 
  - can enable scaling across nodes, clusters, cloud

## Tools

- [Hundreds of workflow tools have been 
  developed](https://github.com/common-workflow-language/common-workflow-language/wiki/Existing-Workflow-systems)
- Each has its own specialities, benefits and user communities
- Some open-source, multi-disciplinary alternatives:
  - [Taverna](https://taverna.incubator.apache.org/):
  "open source multi-platform tool for designing and executing workflows. Taverna is discipline independent and used in many domains, such as bioinformatics, cheminformatics, medicine, astronomy, social science, music, and digital preservation"
  - [Pegasus](https://pegasus.isi.edu/):
    "runs on various environments including personal computers, campus clusters, grids, and clouds. It is quite flexible, but more difficult to learn than Taverna. No graphical design tool is available."
  - [NextFlow](https://www.nextflow.io/):
    "Nextflow enables scalable and reproducible scientific workflows using software containers. It allows the adaptation of pipelines written in the most common scripting languages."
- Different workflow engines are generally not interchangeable -> vendor lock-in
  - A community-led effort to overcome this limitation is the [common workflow language (CWL)](http://www.commonwl.org)

## Managing workflows with [Snakemake](https://snakemake.readthedocs.io/en/stable/index.html)

#### Why Snakemake?
- Gentle learning curve
- Free, open-source, and installs easily via pip
- Cross-platform (Windows, MacOS, Linux) and compatible with all HPC schedulers
  - same workflow works without modification and scales appropriately whether on a laptop or cluster 
- Heavily used in bioinformatics, but is completely general

#### Snakemake vs. make

- Workflows defined in Python scripts extended by declarative code to define rules 
  - anything that can be done in Python can be done with Snakemake
- Rules work much like in GNU Make
- Possible to define isolated software environments per rule
- Also possible to run workflows in Docker or Singularity containers
- Workflows can be pushed out to run on a cluster or in the cloud without modifications to scale up

<br>
<img src="/reproducible-research/img/snakemake.png" style="height: 250px;"/>

> The following material is based on a [HPC Carpentry lesson](https://hpc-carpentry.github.io/hpc-python/)

### Type-along exercise: Snakemake for counting words

> This exercise is based on the [same example project](https://github.com/coderefinery/word-count) as in the previous episode

##### Defining rules

We create a file called `Snakefile` with the following contents:
```python
# Count words.
rule count_words:
    input: 'data/isles.txt'
    output: 'processed_data/isles.dat'
    shell: 'python source/wordcount.py data/isles.txt processed_data/isles.dat'
```
and run it with
```bash
$ snakemake
```
and get
```bash
Building DAG of jobs...
Using shell: /bin/bash
Provided cores: 1
Rules claiming more threads will be scaled down.
Job counts:
    count	jobs
    1		count_words
    1

rule count_words:
    input: data/isles.txt
    output: processed_data/isles.dat
    jobid: 0

Finished job 0.
1 of 1 steps (100%) done
```

What just happened? 
The rule told Snakemake how to build the **target** `processed_data/isles.dat` using the **action** `source/wordcount.py` 
and the **dependency** `data/isles.txt`.

Let's try to build another target by adding a new rule (with a unique name) to the Snakefile:

```python
rule count_words_abyss:
     input:  'data/abyss.txt'
     output: 'processed_data/abyss.dat'
     shell:  'python source/wordcount.py data/abyss.txt processed_data/abyss.dat'
```
and try running `snakemake` again. It gives

```bash
Nothing to be done.
```

This is because, just like make, snakemake only tries to build the first rule in the Snakefile. 
But we can build the new target by

```bash
$ snakemake processed_data/abyss.dat
```

##### Building all targets

To generalize, we need a rule to create all the data files. 
This rule should be at the top of the Snakefile so that it is the default target:

```python
rule alldata:
     input:
         'processed_data/isles.dat',
         'processed_data/abyss.dat'
```

- Dependencies of this rule are targets of other rules. Snakemake will 
  check to see if the dependencies exist and, if not, will check if rules 
  are defined that will create them and invoke those first. 
- An example of a rule that has no actions - used only to trigger the build of its dependencies if needed
- Dependencies must form a directed acyclic graph (DAG) - cyclic dependencies will not work

##### Cleaning up

It's useful to have a rule to remove all data files to be able to explicitly recreate them. 
It can be called `clean`, as this is a common name for rules that delete auto-generated files:

```python
rule clean:
    shell: 'rm -f processed_data/*.dat'
```

This rule can be run by `$ snakemake clean`.

##### Visualizing the workflow

We can visualize the DAG of our current Snakefile using the `--dag` option, which will output the DAG 
in `dot` language (a plain-text format for describing graphs used by [Graphviz software](https://www.graphviz.org/), 
which can be installed by `conda install graphviz`)

```bash
$ snakemake --dag | dot -Tpng > dag.png
```
Rules that have yet to be completed are indicated with solid outlines, while already completed rules are indicated with dashed outlines.

<img src="/reproducible-research/img/snakemake_simpledag.png" style="height: 150px;"/>

##### Testing before running

There is also an option to print out all commands that will be run (`-p`), and 
another to perform a dry-run (`-n`):

```bash
$ snakemake clean
$ snakemake -n -p
```

```bash
Job counts:
    count	jobs
    1		alldata
    1		count_words
    1		count_words_abyss
    3

rule count_words:
    input: data/isles.txt
    output: processed_data/isles.dat
    jobid: 2

python source/wordcount.py data/isles.txt processed_data/isles.dat

rule count_words_abyss:
    input: data/abyss.txt
    output: processed_data/abyss.dat
    jobid: 1

python source/wordcount.py data/abyss.txt processed_data/abyss.dat

localrule alldata:
    input: processed_data/isles.dat, processed_data/abyss.dat
    jobid: 0

Job counts:
    count	jobs
    1		alldata
    1		count_words
    1		count_words_abyss
    3
```

### Exercise


1. Write a new rule for `last.dat,` created from `data/last.txt`
2. Update the `alldata` rule with this target.
3. Write a new rule for `results.txt,` which creates a table from the Zipf analysis. The rule needs to:
  - Depend upon each of the three .dat files.
  - Invoke the action:
  ```python
python source/zipf_test.py processed_data/abyss.dat processed_data/isles.dat processed_data/last.dat > results/results.txt
  ```
4. Put this rule at the top of the Snakefile so that it is the default target.
5. Update clean so that it removes results.txt.
6. Now run `snakemake` (you can test it with a dry-run first)
7. Questions
    - What steps does Snakemake perform if you now do the following steps?
    ```bash
    $ touch processed_data/*.dat
    $ snakemake 
    ```
    - What if you instead do this?
    ```bash
    $ touch data/*.txt
    $ snakemake 
    ```
    - Are the following three commands equivalent?
    ```bash
    $ snakemake
    $ snakemake zipf_test
    $ snakemake results/results.txt
    ```
    - What happens if you do the following? (and should we do something about that?)
    ```bash
    $ touch source/wordcount.py
    $ snakemake
    ```
8. If you get stuck, you can have a look at the [final version of our Snakefile below](#finalversion)


### Wildcards 

The Snakefile created above contains a lot of unnecessary 
repetition which requires more typing and is more error-prone.
*Wildcards* can be used to avoid this situation.
For example, the following code block:
```python
rule zipf_test:
     input:
         'processed_data/isles.dat',
         'processed_data/abyss.dat',
         'processed_data/last.dat'
     output:
         'results/results.txt'
     shell:
         'python source/zipf_test.py processed_data/isles.dat processed_data/abyss.dat processed_data/last.dat > results/results.txt'
```

can be replaced by 
```python
rule zipf_test:
     input:
         'processed_data/isles.dat',
         'processed_data/abyss.dat',
         'processed_data/last.dat'
     output:
         'results/results.txt'
     shell:
         'python source/zipf_test.py {input} > {output}'
```

Let's test if this works:
```bash
$ snakemake clean
$ snakemake 
```

```bash
... [more output] ...
rule zipf_test:
    input: processed_data/isles.dat, processed_data/abyss.dat, processed_data/last.dat
    output: results/results.txt
    jobid: 0

Finished job 0.
4 of 4 steps (100%) done
```

It worked.

#### Naming dependencies

Sometimes one needs to treat different dependencies of a rule differently. This can be done in two ways, either by 
enumeration:
```python
rule count_words:
    input: 'python source/wordcount.py', 'data/isles.txt'
    output: 'processed_data/isles.dat'
    shell: '{input[0]} {input[1]} processed_data/isles.dat'
```
or by naming:
```python
rule count_words:
    input:
           wc = 'source/wordcount.py',
           book = 'data/isles.txt'
    output: 'processed_data/isles.dat'
    shell: 'python {input.wc} {input.book} processed_data/isles.dat'
```

Note that here the source file `wordcount.py` has been made a dependency. This is 
important since any changes of the source code should trigger a rebuild of all
targets that depend on it!

### Pattern rules

The Snakefile at this point looks something like the following:

```python
rule zipf_test:
     input: 'processed_data/isles.dat', 'processed_data/abyss.dat', 'processed_data/last.dat'
     output: 'results/results.txt'
     shell: 'python source/zipf_test.py {input} > {output}'

rule alldata:
     input: 'processed_data/isles.dat',	'processed_data/abyss.dat', 'processed_data/last.dat'

# Count words.
rule count_words:
    input:
           wc = 'source/wordcount.py',
           book = 'data/isles.txt'
    output: 'processed_data/isles.dat'
    shell: 'python {input.wc} {input.book} {output}'

rule count_words_abyss:
     input:  'data/abyss.txt'
     output: 'processed_data/abyss.dat'
     shell:  'python source/wordcount.py data/abyss.txt processed_data/abyss.dat'

rule count_words_last:
     input:  'data/last.txt'
     output: 'processed_data/last.dat'
     shell:  'python source/wordcount.py data/last.txt processed_data/last.dat'

rule clean:
    shell: 'rm -f processed_data/*.dat results/results.txt'
```
 
It still contains lots of repetition, i.e. there's a separate rule for each `.dat` target. 
We can replace all these rules with a single *pattern rule* which can be used to build 
any `.dat` file from a `.txt` file in `data/`:
```python
rule count_words:
    input:
           wc = 'source/wordcount.py',
           book = 'data/{file}.txt'
    output: 'processed_data/{file}.dat'
    shell: 'python {input.wc} {input.book} {output}'
```

This general rule uses the wildcard `{file}` as a placeholder for any book in the `data/` directory.


### OPTIONAL: further topics

#### Python functions

Snakefiles are more or less just Python code, and we can add Python code anywhere:
```python
# at the top of the file
import os
import glob

# add this wherever
rule print_book_names:
    run:
        print('These are all the book names:')
        for book in glob.glob('data/*.txt'):
            print(book)
```

The `snakemake.io` modules comes with a set of useful functions. To generalize our Snakefile further,
we can use the `glob_wildcards()` an `expand()` functions:
```python
DATA = glob_wildcards('data/{book}.txt').book
...
rule zipf_test:
    input:
        zipf='source/zipf_test.py',
	books=expand('processed_data/{book}.dat', book=DATA)
    output: 'results/results.txt'
    shell:  'python {input.zipf} {input.books} > {output}'
```
This is particularly useful if a rule has lots of dependencies.

#### Resources and parallelism

Just like GNU Make, Snakemake can run in parallel:
```bash
$ snakemake -j 4
```
which gives the output:
```bash
Provided cores: 4
Rules claiming more threads will be scaled down.
... more output ...
```

This will allow Snakemake to execute multiple instances of rules at the same time 
and speed up the run.

Apart from `input`, `output` and `shell`, rules can have a number of other keywords.
Among other things, one can explicitly specify how many cores (or threads) a rule needs:
```python
# count words in one of our "books"
rule count_words:
    input:
        wc='source/wordcount.py',
        book='data/{file}.txt'
    output: 'processed_data/{file}.dat'
    threads: 4
    shell: 'python {input.wc} {input.book} {output}'
```
This will run each instance of the rule using 4 theads.

Non-CPU resources are handled with the `resources` keyword. Pretending that we have a plotting 
routine which can use a GPU, this can be specified in the Snakefile as:
```python
# create a plot for each book
rule make_plot:
    input:
        plotcount='source/plotcount.py',
        book='processed_data/{file}.dat'
    output: 'results/{file}.png'
    resources: gpu=1
    shell: 'python {input.plotcount} {input.book} {output}'
```

The workflow can now be executed on 4 cores and one GPU by:
```bash
$ snakemake clean
$ snakemake -j 4 --resources gpu=1
```

Note however that `gpu` does not actually represent a GPU, it is simply an arbitrary limit 
used to prevent multiple tasks that use a `gpu` from executing at the same time.

#### Running on a cluster

Moving a workflow to a cluster or between clusters can be a tedious task; batch scripts need
to be written and adapted to possibly different job schedulers, jobs that depend on each 
other need to be executed in the right order, etc. 
But Snakemake can manage this for you - it writes the batch scripts, submits and monitors jobs!

To facilitate the transfer of a workflow to a cluster, Snakemake has the `--archive` flag 
which will archive all files under version control (scripts, Snakefile, config files, 
conda environment file), and in addition all input files, to a specified tar archive file. 
On the cluster, Snakemake can then use the conda package manager to recreate the software
environment:
```bash
$ snakemake --archive myworkflow.tar.gz
$ scp myworkflow.tar.gz <some-cluster>
$ ssh <some-cluster>
$ tar zxf myworkflow.tar.gz
$ cd myworkflow
$ snakemake -n --use-conda
```

Snakemake uses a JSON configuration file to specify cluster-specific parameters. 
An example file for a SLURM system is:
```bash
{
    "__default__":
    {
        "account": "a_slurm_submission_account",
        "mem": "1G",
        "time": "0:5:0"
    },
    "count_words":
    {
        "time": "0:10:0",
        "mem": "2G"
    }
}
```

In this case, rules will by default use 5 minute jobs requiring 1 GB, while the `count_words` rule requires
more time and memory. 

Some rules do not need to run in a separate job on the cluster (since they take only seconds to complete),
and should rather be completedd locallywhere the `snakemake` command is run (e.g. login node).
One can add such exceptions with the `localrules` rule:
```python
localrules: all, clean, make_archive
```

The workflow can now be executed by:
```bash
snakemake -j 100 --cluster-config cluster.json --cluster "sbatch -A {cluster.account} --mem={cluster.mem} -t {cluster.time} -c {threads}"
```

Note that in this case `-j` does not correspond to the number of cores used, instead it represents the maximum 
number of jobs that Snakemake is allowed to have submitted at the same time.  
The `--cluster-config` flag specifies the config file for the particular cluster, and the `--cluster` flag specifies
the command used to submit jobs on the particular cluster.

#### GUI

Snakemake has an experimental GUI feature which can be invoked by:
```bash
$ snakemake --gui
```

#### Final version of our Snakefile {#finalversion}
```python
# a list of all the books we are analyzing
DATA = glob_wildcards('data/{book}.txt').book

# this is for running on HPC resources
localrules: all, clean, make_archive

# the default rule
rule all:
    input:
        'zipf_analysis.tar.gz'

# delete everything so we can re-run things
rule clean:
    shell:
        '''
        rm -rf source/__pycache__
        rm -f zipf_analysis.tar.gz processed_data/* results/*
        '''

# count words in one of our books
# logfiles from each run are put in .log files"
rule count_words:
    input:
        wc='source/wordcount.py',
        book='data/{file}.txt'
    output: 'processed_data/{file}.dat'
    threads: 4
    log: 'processed_data/{file}.log'
    shell:
        '''
        echo "Running {input.wc} with {threads} cores on {input.book}." &> {log} &&
            python {input.wc} {input.book} {output} >> {log} 2>&1
        '''

# create a plot for each book
# shows example usage of the resources keyword
rule make_plot:
    input:
        plotcount='source/plotcount.py',
	book='processed_data/{file}.dat'
    output: 'results/{file}.png'
    resources: gpu=1
    shell: 'python {input.plotcount} {input.book} {output}'

# generate summary table
rule zipf_test:
    input:
        zipf='source/zipf_test.py',
        books=expand('processed_data/{book}.dat', book=DATA)
    output: 'results/results.txt'
    shell:  'python {input.zipf} {input.books} > {output}'

# create an archive with all of our results
rule make_archive:
    input:
        expand('results/{book}.png', book=DATA),
        expand('processed_data/{book}.dat', book=DATA),
        'results/results.txt'
    output: 'zipf_analysis.tar.gz'
    shell: 'tar -czvf {output} {input}'
```

<img src="/reproducible-research/img/snakemake_dag.png" style="height: 300px;"/>

#### Further documentation

Visit [snakemake.readthedocs.io](https://snakemake.readthedocs.io/en/stable/).