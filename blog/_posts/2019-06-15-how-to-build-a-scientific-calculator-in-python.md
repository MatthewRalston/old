---
title: How to build a scientific calculator in Python - Part 1.
tags: prose beginner education python
toc: true
---

About a 45 min read.

## Introduction

Bioinformatics research in 2019 relies on high-performance, compiled C, C++, or Java programs to solve alignment and assembly problems efficiently. However, interpreted scripting languages, such as Python, Perl, and shell languages (bash, csh, zsh, fish), are needed afterwards for further calculation, quality control, visualization, and more. 

Novel metrics, normalizations, and even complex algorithms are often prototyped in 'beginner' languages. Some metrics or algorithms prototyped in this way are so useful that a company or the community may optimize the implementation further with a compiled language. Nearly every bioinformatician can write data analysis pipelines, web applications, or basic calculations in scripting languages, but code quality and reproducibility have been highlighted as widespread issues. The application/industry side of bioinformatics relies upon scripting languages for basic data processing, collation, analysis, and presentation. But advice on essentials for building your own novel tool seems to be rare and it's a skill that's expected to be learned through practice and experience.

This article is part 1 of a two part series on the topic. In this article, I'll discuss repository structure, installation procedure, CLI/SDK best practices, `README.md` conventions, and other user-friendly goodies that may give your tool (or suite) and associated reports or publications better visibility and adoption rate by the scientific community. Part 2 will describe other software engineering considerations including benchmarking the various parts of the tool and including test datasets or KnitR `.Rmd` reports to describe what questions or assumptions were investigated for each tool, and how the metrics were tested for performance and accuracy on simulated and/or real-world datasets.

## Finding a problem

There are so many areas where bioinformatics theory can be approached with simple languages! I don't think bioinformatics education should focus on "how to create a data processing pipeline," though it is certainly an employable skill. But somewhere along the way, you'll find a problem where there aren't many theoretical metrics or quality control procedures available, and you'll have to invent your own.

But how *do* you make a novel command-line metric, algorithm, or program that scientists will actually want to use? What is the difference between a one-off script and a mature suite? Scientists that may have rudimentary command-line abilities often complain about the quality of tools available for mainstream use. A mature tool anticipates the size or quality of input datasets, provides an easy installation process, and answers questions about the derivation or implementation without demanding users browse the source code. Easier said than done.

Here are some patterns that you can follow to use Python to develop a novel calculator for a class project, graduate programs, for an employer, or to support the open source community. If you prefer another language to Python, you may still find this article useful for development and testing patterns. I'll begin this article by describing what I think is a solid repository structure for a Python package and key files or entry points that you'll need to emphasize to the end-user. 

## Repository structure

### Directories

```
bin/
docs/
mymodule/
test/
test/data/
```

Your directory structure and files in the root of your repository are the first things users see. You'll want your code, documentation, and tests in predictable locations so anyone can clearly understand what directories are meant for developers, and which contain code they might need to read when identifying bugs or asking questions on an issue tracker. A little bit of directory nesting can help you orgranize and group your content, but too much may confuse users or developers who might actually care about navigating your repository. Let's start with an overview of this pretty simple directory structure before we consider specific files or entry points that you'll need to explicitly emphasize in the `README`.

But before we begin, I'd like to differentiate between command-line tools and APIs/modules/libraries that users would integrate into their own codebases. In this guide, I'll suggest that the reader build both a CLI and a module, so that other developers could build on top of their codebase, something we should all hope for our software.

#### `bin/`

Include a `bin/` directory for your main executable and any accessory scripts that could be considered portable, if the user's `PYTHONPATH` variable is set properly. I'll talk about the portability component very shortly.

#### `docs/`

A mark of a solid repository is well-developed, up-to-date documentation that supplements a `README` file. In 2019, I suggest the [Sphinx](https://www.sphinx-doc.org/en/master/index.html) documentation engine for Python code, which absorbs docstrings from the source code with some fairly simple configurations. The result is clean, simple HTML documentation with a familiar "ReadTheDocs" style layout that can be served on a separate domain or as part of a Github pages front-end for your codebase. The number one reason why developers don't document their code is because they already know how to use it. The Sphinx documentation system provides a convenient "auto" documentation system if you are already using [docstrings](https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html) to document your classes and functions, so you never have to write advanced documentation if you're already doing it according to some PEP standards!

#### `mymodule/`

Create a module directory (and a usually empty `__init__.py` file) for the key functions and classes. You'll just import these into your CLI later. Keep the name of your module simple. If your application is called `mondo`, the command-line interface could be a shebang'd script at `bin/mondo` and an associated module directory `mondo/`. I am aware that class names should be camel-case and most other content should be underscored, I am not aware of a convention for module names, and a short simple name or initialism without hyphens or underscores prevents ambiguity.

#### `test/` and `test/data`

Create a test directory to house <a href="https://stackoverflow.com/questions/5357601/whats-the-difference-between-unit-tests-and-integration-tests">unit, integration</a>, and <a href="https://dzone.com/articles/untangling-concepts-unit-tests">acceptance tests</a> for all levels and interactions of our codebases, so users can determine for themselves if the code should run properly on their system. These tests will run on a <a href="https://en.wikipedia.org/wiki/Continuous_integration">continuous integration</a> server and ideally *during* the installation process, to alert users of any issues on their system. 

Create a data subdirectory for minimal test datasets with your repository. The input to most scientific calculators is most likely an input dataset and some parameter values, and you'll want users to be able to essentially substitute their dataset for a command run by your acceptance tests. Your test datasets should span a good range of common artifacts, misformating, and other scenarios.


### Essential files

Okay! Now that the directories have been summarized, let's talk about some specific files that you'll want to include with your repository.

```
bin/myprogram
docs/Makefile
Makefile
README.md
requirements.txt
requirements-dev.txt
setup.py
test/Makefile
.travis.yml
```

#### Your primary entry point: the command-line interface (CLI)

A portable script located in the `bin/` directory is the ideal entry point for your program. It should have [argparse](https://docs.python.org/3/library/argparse.html) or similar argument parsing with a thorough description of formats, parameter types, and effects those parameters will have on the corresponding output. 

What should this script have or *not have*? This script won't contain many function definitions, instead it merely sources the Python files (a.k.a. submodules) with the functions and classes, and runs them in the appropriate order with inputs passed in from argparse. I like to use the [logging](https://docs.python.org/3/library/logging.html) module in the Python standard library to provide more detail to users and developers with a `-v, --verbose` option. Also, I like to do type-checking in those submodules themselves, rather than the CLI, to keep the logic and validations outside of its scope.

How do I make it portable? You'll want to double-check that your `setup.py` is [configured properly](https://python-packaging.readthedocs.io/en/latest/command-line-scripts.html) to move this script to the appropriate location on the user's filesystem. After installation, users should be able to invoke `myprogram` easily, since it will be conveniently in their `PATH`; similarly, your `module/` will be importable thanks to standard [pip/PyPI](https://pypi.org/) conventions.


#### [Makefiles](https://gist.github.com/MatthewRalston/4f858bf8bfab9e244ae66faa6b6b3223)

As mentioned earlier, the Sphinx documentation system provides a convenient "auto" documentation system if you are already using [docstrings](https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html) to document your classes and functions. The `sphinx-quickstart` process will typically generate a `Makefile` to accompany your documentation process, that can be run from the master `Makefile` in your repository root. When properly configured, you'd like to run a simple `make docs` right before you commit, to update your documentation automatically from the docstrings of your source code.

The master `Makefile` can also run your test `Makefile` with `make test`, so that users or your continuous-integration process can easily invoke your test suite of choice. Your `test/Makefile` will just have the commands required for coverage reports, unit tests, integration tests, and acceptance tests.

#### `requirements.txt` files

Some styles of pip/PyPI packaging suggest that all dependencies be specified in a [`requirements.txt`](https://pip.pypa.io/en/stable/user_guide/#requirements-files) file. All PyPI packages should have dependencies also declared in the `setup.py` as well. I like to separate development dependencies in a separate `requirements-dev.txt` file, and read both of these into my `setup.py` file to easily declare and update dependencies while supporting both conventions for dependency declarations. 

#### `setup.py`

An essential file for a Python package, this contains instructions for `pip` to understand how to install your package: dependencies, license information, supported Python versions, etc. Note below that `requirements.txt` and the optional `requirements-dev.txt` are essentially snapshots of your development virtual environment; you only need to update those in order to update the `setup.py` file. Second component of note is `scripts=['bin/myscript']`, which specifies the executable that should be available in the users `PATH` after installation. 

<script src="https://gist.github.com/MatthewRalston/0049d7fe72629f2e874ea0e23086c6e3.js"></script>

#### `.travis.yml`

The final ingredient is a Travis-CI configuration file, which lets you specify supported operating systems and language versions. Travis takes this file and launches several containers to run your test suite, providing an easy to use report letting users know if their platform is supported, providing a convenient badge letting users know your tests are currently passing.


## Installation procedure

If you followed my suggestions above for the setup.py file, PyPI provides a convenient installation experience for your users. You can test your installation locally by running `pip install -e .` in the root of your repository. Ideally, your installation process should run unit tests during installation, to give users an idea if your package is working as expected during installation. Eventually, you could include a badge with a link to the package in PyPI at the top of your `README` with an installation section with a single line: `pip install mypackage`. If your code works well and is tested for their platform, it would be hard for most non-developer scientists to find your package difficult to install.


## What does a good CLI look like?

I typically suggest that users use `git` and `aws-cli` as good references for high-quality command-line suites. Both possess a variety of subcommands, and the simple 'verbs' or subcommands are easy to remember. But there's more to it than just mimicing existing tools. You want users to feel comfortable knowing what is happening in your codebase, what parameters are required, what their types are, and how your tools can be combined into other workflows. 

I'm not a great coder, but I have run in to most of the basic pitfalls possible in simple languages, most common were TypeErrors because I passed it a string `'5'` instead of the integer `5`. Also I ran in to IOErrors from passing an empty list (read from an empty file) to a function, which can be prevented by verifying that the file exists, is readable, and isn't empty. But how should you treat those errors? Throw an error if the file is empty? Throw an error if the list read from the file is empty? How would you validate a tsv file? Does it have a header? A more complex format? Are existing parsers available? Should you use argparse to validate that the file exists and then use `args.infile.close()` to close the handle, and reopen it with a parsing library? There's no one way to build a good script or CLI, but here are my suggestions for Python projects.

### TLDR: argparse

The long and short of this is that almost all developers should spend some time working on a project, evolving their style, and reading the [argparse](https://docs.python.org/3/library/argparse.html) manual. It includes information about parameter [types](https://docs.python.org/3/library/argparse.html#type), [flags](https://docs.python.org/3/library/argparse.html#action) vs [parameters/options (--option) vs positional arguments](https://docs.python.org/3/library/argparse.html#name-or-flags), parameter [defaults](https://docs.python.org/3/library/argparse.html#default), [required](https://docs.python.org/3/library/argparse.html#required) parameters, parameter [choices](https://docs.python.org/3/library/argparse.html#choices), parameters that should be [files](https://docs.python.org/3/library/argparse.html#filetype-objects), and even [subcommands](https://docs.python.org/3/library/argparse.html#sub-commands).  Most other languages have similar argument parsing libraries available ([commander.js](https://github.com/tj/commander.js), [OptionParser (Ruby)](https://ruby-doc.org/stdlib-2.6.3/libdoc/optparse/rdoc/OptionParser.html), [getopt](https://cran.r-project.org/web/packages/getopt/getopt.pdf) and [argparseR](https://cran.r-project.org/web/packages/argparser/argparser.pdf) for R) to make easy-to-use CLIs.

### Conventions for input/output

When I first started writing scripts, I'd include a `--infile` option so that users could pass an input file. Sometimes you'll see the input file as an positional argument. This is a great convention; it makes it easy to understand the difference between parameters/options, prefixed with a double hyphen (like `--verbose`), vs. positional arguments that are usually filepaths. However, what if your users want to pipe output from another program directly in to yours? How can you tell argparse that you'd like the CLI to read from STDIN by default? `argparse.ArgumentParser.add_argument()` supports a `default=` keyword argument, where you can specify `default=sys.stdin` to read from STDIN by default. Using STDIN/STDOUT conventions makes it much easier to construct pipelines with individual tools or subcommands. Also, you should print all debugging to STDERR by default and the `logging` module referenced earlier does this by default. 

Adding in support for compressed inputs is surprisingly simple in Python. I've uploaded a [gist](https://gist.github.com/MatthewRalston/6641f45bdce19341f568264132b794de) from a unpublished side project called ngsci. The gist has some code that validates compressed and uncompressed fasta files with BioPython, ensures that an alignment file is sorted/indexed, and even has support for data hosted on S3. Note that in order to validate a S3-hosted dataset, you could stream the file and validate in flight, but it would be wasteful to have to download the file again to process it; it makes sense to download the file to a temporary file, as long as the rest of your application can remove the file when it's done.

### Verbosity and debugging

I actually didn't start with Python. I started with Ruby and became heavily interested in the `log4r` framework, which can show a timestamp, the line number of the file producing the logging, and colorizes output so developers can easily distinguish between info and debug level logging. The older Log4j framwork was a customizable Apache and Java logger that inspired `log4r`, `log4js` (NodeJS), and Python's `logging` module. If you're building web applications, you might want different levels of verbosity printed to the console vs a log file vs an online log archive, and these frameworks support inheritance and multiple outputs. 

Basically, here are the different types of errors: `FATAL` errors are errors where the program cannot continue. `ERROR`s and `WARNING`s should almost always be relayed to the user, and as a script-kiddie I consider most errors fatal. In contrast, `INFO` messages are high-level and let the user know if the process is parsing, iterating over input, or simply still calculating. Finally, developers use `DEBUG` messages for extremely verbose information about each calculation, so that a near continuous stream of information is printed to STDERR. They can also include the details of specific iterations in case of a fatal error. I like to use the following in my CLI to let users toggle between info and debug level verbosity (`WARNING` by default) by specifying one or two `-v, --verbose` flags (`INFO` and `DEBUG`, respectively) and the logger in `mymodule/submodule.py` inherits settings from the root logger environment. 

```python
# bin/myprogram

# globals
logger=None

def get_root_logger(loglevel):
    if loglevel > 2:
        loglevel=2
    levels = [logging.WARNING, logging.INFO, logging.DEBUG]
    logging.basicConfig(level=levels[loglevel],format="%(levelname)s: %(asctime)s %(funcName)s L%(lineno)s| %(message)s",datefmt="%Y/%m/%d %I:%M:%S %p")
    root_logger = logging.getLogger()
    return root_logger

if __name__ == '__main__':
    parser = argparse.ArgumentParser()
    parser.add_argument('-v', '--verbose', default=0, action='count', help='Controls verbosity. Default: WARNING')
    parser.add_argument('fasta', type=argparse.FileType('r'), help='A genomic fasta file')
    args = parser.parse_args()
    # Set up the root logger
    logger=get_root_logger(args.verbose)
    # Main routine
    main(args)
	
# mymodule/submodule.py
import logging
logger = logging.getLogger(__name__)
```


## How to build an algorithm in a Python module

Your module *can* be referred to as the API or SDK, although those usually have a different context of web applications or novel platforms (IOS, android), but I digress. Just know that if I use the phrase API, I'm referring to your module that contains the functions, classes, and logic of your calculation. In contrast, the CLI I discussed above might seem pretty boring, and it should be. It's just a wrapper to make your module accessible and easy-to-use from the command line. 

Even the argparse validations are a formalism: <em>you should include type-checking in all of the code of your module: your functions or your class `__init__` </em>.

So, all of the work that needs to be done is in the [module](https://docs.python.org/3/tutorial/modules.html). I usually need to do 3 things with the input dataset(s) and parameters:

1. validate and iterate over some input file

2. perform the calculations, creating multiple resulting records

3. print the resulting records to STDOUT. 

### Key questions for the implementation

Your strategy for addressing the first challenge of input format parsing effects the whole problem. Sometimes, you can just read the whole dataset into memory if it's sufficiently small. Sometimes this works well because the calculation actually cross-references multiple reads, records, basepairs, etc. Other times it can be faster to perform the calculation one record at a time, *while* you're reading the file. Data is flowing out of the file on your storage device into the computer's memory and then fed to the CPU cache. It isn't sitting in some list waiting for the rest of the file to be read before the actual calculation begins; it can flow through the function that actually calculates what you care about as the disk continues to read data in the same way. So even though the first topic of validating/parsing input files is supposed to be about *just* that, it plays a considerable role in making a performant implementation. The best overall algorithmic strategy and even your I/O strategy depends on what your calculation actually does.

#### Are all the possible input datasets small enough so that the calculation won't be bottlenecked by memory or I/O speed?

If not, that's okay. But if it is, this *could* be a good strategy if you need to calculate something *between* the records, lines, or individual pieces of your dataset. 

Suppose your calculation works on subsets of gtf/bed records, grouping them by location in the genome. Each time you create a subset, you're filtering all `n` records down for your calculation (unless you have an index present). Or suppose that you need to do all pairwise comparisons between the `n` inputs, and maybe filter or sort those comparisons with some criterion at the end. That's O(n<sup>2</sup>) comparisons plus a average O(n*log(n)) quicksort or a O(n) filtering. At this point, you can see why these inter-record comparisons are more costly in memory and time than simply calculating one metric for each input, which should have fairly linear O(n) complexity. 

The real key here is making a function that does a single comparison. Instead of telling you *how* to make that function, I've instead focused on how important it is to assess the computational complexity of your calculation in terms of dataset size (`n`) before you decide what the best impementation for your problem really is. If you can't fit the dataset in memory, you'll need a sorting/indexing or chunking strategy with more frequent and expensive I/O operations to address the problem. *But* if you *can* process the data one record at a time (i.e. a mostly linear calculation), you can avoid reading all the records into memory and stream the data through the calculation (disk => RAM => CPU cache) and back to STDOUT very quickly.

#### What if I *can* process the input dataset one line/record at a time?

If there is an avaialble format validator/parser, I'd suggest using it *if* it returns something that behaves like a filehandle/io object or uses a generator pattern. If the module you're trying to use suggests using something like the loop below, it's clear to most beginners that the result of the `ParseFile` method below is 'iterable'; it likely it parses and validates each record on the fly. Great! While it isn't a list, it 'yields' results one at a time until it's completely parsed the file. This is called a 'generator' pattern, since it doesn't load all records into memory as a list. Instead it just keeps a pointer at the current line/record in the file, parses it, and 'yields' it to the outside scope that is invoking the iteration.

```python
for record in somemodule.ParseFile(yourfile):
   print(record)
```

If there isn't a parser available, you'll want to implement your own record validation function that takes a line (or several lines) as input, validates the record, and returns it. This is very easy if you're just checking column types for a tsv, since you can just use the file `open()` generator and pass each line to a simple function that splits strings, converts integers for certain columns, etc. But if it requires loading several lines at a time, you'll need to build your own 'generator' to define how many lines to read, whether or not those lines consitute a valid record, the data structure for the individual records, etc.

After you've figured out how to validate the input dataset's format, you'll need to pass each record to your function that performs the calculation on a single record. It might look like the following. You'll notice that there is a parser module that contains one or more parsing functions, and a calculator module with one or more fancy metrics that you can calculate from a single record.

```python
import myparser
import mycalculator


def calculate_something(filename, var1, var2):
	""" Takes a file and calculates a metric """
	for record in myparser.ParseFile(filename):
		print(mycalculator.SomeMetric(record, var1, var2))

```

#### Gotta go fast! How can I make it as fast as possibble?


At this point you have a better idea of the key considerations of parsing+validation and the calculation itself; earlier I contrasted non-linear inter-record or inter-group comparisons vs one metric for each input record. The actual 'atomic' (i.e. indivisible) calculation on a record, pairs of records, or groups of records may *also* be linear or non-linear in addition. Fortunately, some of the things you may be doing may be quite simple conditionals, additions, multiplications, or grabbing the i<sup>th</sup> column and using that in an equation. There are efficient ways and inefficient ways to do even these very basic operations in Python. Though I haven't covered some more specifc tricks yet, I have a nice introduction to <a href="http://matthewralston.github.io/blog/beginner-python-arrays">benchmarking Python list methods</a>. 

So far, we've talked about how a parallelization strategy is heavily dependent on whether or not the dataset can be loaded into memory, and what the nature of the metric or comparisons really is. Let's discuss it in the context of time complexity.

##### O(n) in the number of inputs

Before we get more complex, let's assume for now that you're calculating one metric for each line of a file. This can be sped up considerably by using parallel processing with Python's `multiprocessing` module, part of its standard library. If there is just one metric for each line, you might not need to even add parallel processing unless your file is extremely large and waiting a few minutes is unacceptable. You can read a 'chunk' of a file at a time, maybe several hundred lines or several thousand, and this may be a parameter you'll need to experiment with and optimize. Essentially, each process gets a chunk of data read into memory, calculates the metric for each record in the chunk, and returns the output when the processes 'join'. Then you'll just print all of them out and start reading new chunks until the file is fully processed.

Believe it or not, if your algorithm is linear, there isn't a huge amount of performance gain possible with clever software techniques. Most metric calculations can't be sped up further like linear search vs binary tree search. In fact, the overhead of the `multiprocessing` module could hurt performance if the implementation isn't smooth. That said, congrats on having a linear metric!

##### Naive pairwise comparisons: O(n<sup>2</sup>)

For pairwise comparisons between the `n` inputs, the situation becomes more complex, especially if the whole dataset can't be read into memory on the hardware available to you.

Now, let's imagine that you've sorted and indexed your dataset, and can read arbitrary groups in, one at a time, in a similar manner. Maybe you just want to do pairwise comparisons O(n*n), or maybe the number of records in a chunk isn't uniform (groups/clusters). But, you've figured out that much on your own. Each group can be compared to other groups or each subset of the pairwise comparisons can be run in parallel. The challenge is similar for both, the difference being that the number of records analyzed by each processor in the pairwise comparison is typically a fixed number (100s or thousands), while group size might not be uniform for the former.


Either way, here is a strategy and some considerations for doing the more complex comparisons. Suppose you read the first 100 records into memory consituting group 'A'. Now you want to compare each record of group A to each other record of the file which will get you the first set of pairwise comparisons: group A vs all. You could compare group A to each record one line at a time, or you could also read those in 'chunks' it doesn't really matter how you end up doing it. 

Let's assume for now that you read the second 100 records constituting group B into memory, then group C, and so on until the file is exhausted for the first group 'A'. Meanwhile, a different processor reads group B (the second 100 records) into memory and compares them to the rest of the file as well. *But* you're making redudant comparisons (A<->B and B<->A). Approximately half of the pairwise comparisons could be considered redundant, depending on the calculation. What you ultimately want is the non-redundant set of comparisons of AA, AB, AC, AD etc. then adding BB, BC, BD etc. while avoiding the BA comparison. See what I mean? It's like a triangular matrix. 

Okay but what *is* the naive implementation? The naive strategy is to simply remove duplicates later; there are two strategies for parallelization of such a complex comparison that come to mind (there are solutions in between), the first of which is memory intensive. Say that you have 4 processors. Each reads 25% of the dataset into memory, which could be prohibitive if the dataset is large. Then each processor *streams* the file, producing pairwise comparisons for the 25% of the dataset it was allotted. The second strategy is similar to the first, but is more I/O intensive. Each processor reads a chunk of lines (or a group of records) into memory that is typically a few hundred or a few thousand rows, likely less than 5% of the dataset at a time, and again streams the file, producing the pairwise comparisons. These naive solutions should be considered wasteful, but easy for most beginners to implement while still receiving a speedup from parallelization. Note that the number of lines read in memory at a time is a flexible parameter that you can consider in the context of your storage speed.

#### Is faster always better?

Now if I was a good instructor I'd leave the non-redundant implementation to the reader and ask how they'd eliminate the redundant comparisons for a nearly 100% speedup, in theory, over the naive. At this point it's worth emphasizing that in the real world, no one is going to give you the correct answer, and sometimes you can code the naive prototype faster than it would take to get the algorithm perfect the first time. I'll take it one step further and say this *is* what you should do. You should usually use the naive implementation, especially if the speedup might only be 2x. This means you can deliver something to your boss or advisor rapidly, and express your desire for the time required to improve the implementation! 

Most 'completely rational' scientists or professional types need to see how the prototypes perform before they decide whether the time required to implement a 'correct' solution can be justified or expensed properly. Over time you'll develop a sense that 'less is more' when it comes to developing professionally, especially if communication is routine and your benchmarking qualities are good. As scientists, sometimes giving a manager better confidence in the numerical precision or accuracy of a metric is more important (at first) than the best software implementation long term. Remind yourself here that your time in grad school or in industry is valuable. And most scientists or companies value the effort in benchmarking the performance and computational cost of a naive prototype more than they will likely value the most efficient implementation. I'll discuss benchmarking in the next post of this 2 part series.

### Specific examples

Now that I've introduced some considerations I'd like to give some specific examples of how they can be used to design prototypes in the open source world. The process usually starts with one or two seemingly simple questions or ideas about blindspots in current analyses. Over time, you'll decide that there are multiple metrics, normalizations, or algorithmic strategies that need to be tryed and tested. Putting effort and commitment into the design and optimization of precision/speed is what makes your project live or die, even in academic settings. Taking some time to develop and refine your software is like writing an essay for class: you wouldn't hand the teacher your outline as a finished product, and you know that 'A' papers involve lots of revision, rewording, efficiency, and referencing of ideas/inspirations for key concepts of the paper. 

The first time I saw the SAM/BAM spec, the CLI rose above *just* well-written software. It was a thorough and evolving discussion of the community's needs for representing the various nuances of the alignment process. Sure the attention to the community is important (lit review, issue tracking, evolving specification, etc.) but software quality is what brings people towards the community to acquire feedback, developers, features, and bugfixes.

I didn't get much formal advice or education on software engineering when I started in bioinformatics. There were lots of name drops about tools you can use in the open source world, but like most graduate students, my focus was on producing results for my professor, rather than perfecting each tool I made. Another thing that I was 'open' to, was advice on how complex software systems can be, and how some of the most useful software these days is software that you can deploy to broad audiences. Web applications were part of a project for a database systems course I took at the University of Delaware, an aside that reflects the primary application of databases. Very few scientists feel comfortable interacting directly with a database, and often require a web-form or query interface that is... easier.

Anyways, software engineering can go a long way towards making software useful to many (if it's worth the development cost, where there's no clear answer or heuristic presented here) or even... just *palatable* to a few. Here are a few examples of side projects I've worked on, and what inspired them, and some of the considerations for efficiency improvements I've made (even though I haven't implemented them in most cases, yet).

#### NGS-CI

The first project I started came around during graduate school, where I was working on a transcriptome assembly project for *C. acetobutylicum*. Like any bioinformatics project, the resulting assembly is only as good as the data it's given, regardless of how elegant the algorithm may be. After assembling the transcriptome, I needed something to *validate* that *most* of the transcripts should be considered reasonable. And due to the biological conditions spanned by the experiment, I could only see transcripts expressed under those specific conditions. However, my professor was a veteran of microbiology and had extensive literature background in the organism under study, and was familiar with studies of transcription start-site identification with Northern and 5` RACE-PCR that could validate a handful of transcripts. Thankfully I validated a few, but I was still in search of a metric for assessing transcriptome quality focusing on alternative transcription start-sites. 

A big issue is that while RNA-Seq is highly sensitive, many transcripts are expressed at very low levels. How could I tell if there was sufficient data at the *ends* of a transcript versus the sequencing reads resulting from un-degraded DNA and amplification artifacts? Most of us are familiar with the duplicate read issue for variant detection, and my vague awareness of that concept (no experience), led me to wonder how sequencing complexity is being assessed in the field. To my surprise, there weren't any metrics from my review that estimated this at a single-base resolution.

"But I'm not an algorithm developer," I said, "I'm just here to run the RNA-Seq experiment and try to run a few Northern blots." There was no way my professor, who wasn't a bioinformatician by *modern* standards, would be interested in investing my time in the development of an algorithm with little basis in the literature. Thankfully, during my second year, the results of my initial assembly and differential expression analyses were promising, and between breaks writing my thesis, I decided to think a little bit more about the problem I'd found. That's how these things go.

It became clear to me that *after* eliminating duplicate reads, there could only be so many unique reads aligned to a single base under two assumptions. First, that read-size is fairly fixed with Illumina technology; second, consider a base in a genome 'i'. Read A is a full-length read aligned to base `i-x`, where 'x' is the offset from base i necessarily less than the read length. Read B is a full lenth read aligned to base `i-x+1`, a read aligning to the neighboring base. The second assumption is a statement about read uniqueness: if both reads are full length, then it is likely the reads originate from independent amplification events during library preparation. In an ideal situation, the maximum number of reads that could 'cover' base `i`is equal to the fixed read length. 

After I got that far, it was off to the races. I just needed to design a metric that expressed the observed reads as a fraction of the maximum possible of deduplicated/unique reads that could possibly cover a base. The tougher part was the implementation. I started the initial project as a Ruby Gem that was a fully serial algorithm that read blocks of reads, identified their 5` base and length, and deduplicated the reads for a single base before calculating the metric. The formulation of the metric took some time, but it was simple algebra. But my first prototype didn't consider every possible combination of sequencing technologies, mate-pair strategies, or strand-specificity. It wasn't very fast and didn't support parallelism. But now that I had a formula that seemed mathematically sound, the rest were implementation details.

Lately, I've been rewriting this algorithm as a Python package that I hope to distribute on PyPI some day. Implementation comes first, optimization comes second, and benchmarking against simulated and real-world data leads to more optimization. I'm really happy that I have no conflicts related to this package, it was developed during my academic period *entirely*, and didn't have a meaningful impact on my thesis. In fact, I was in committe meetings and in the revision cycle well before the first prototype was implemented in early 2015. You'll see why this matters so much to me in a moment.

#### Kmer.js

My second side-project was a toy I intended to work on for a friend. It was a pretty basic concept and there were several competing implementations of k-mer libraries in a variety of languages, including Javascript. But few if any exisitng libraries supported arbitrary number of k-mer profile comparisons at once, fastq and fasta input, or discussed normalization of k-mer profiles. There were a few other goodies I had in mind and I started working on a NodeJS library that I called kmer.js.

There were three related calculations I needed. The first and most challening at my level was the most efficient way to store a k-mer profile in memory. It turns out that when you get to k > 20, most languages have a problem storing 4<sup>20</sup> = 1.1 quadrillion integers in memory on consumer hardware, let alone all of the k-mer strings. I used the [kPAL](https://doi.org/10.1186/s13059-014-0555-3)(<sup>1</sup>) library as inspiration for the encoding strategy to partially address the memory issue. A better solution still could be to use a file of bytes, but I digress. Each calculation had a different complexity, and the accuracy or utility of each needed to quantified. I'll talk briefly in the next part about the other two calculations and how difficult it was to begin benchmarking.

When you're starting out here, k-mer profiles may seem like a problem that is already solved and that is partially why I took the chance to write my own implementation. But what I didn't expect was that even some fairly basic questions should have formal treatment with the metrics. The first question was, "do the correlations of the normalized k-mer profiles reflect existing phylogenetic knowledge?" Addressing this in a basic way is rather simple, but can require dozens of sequences from a genus you're familiar with to properly validate. At this point, I still haven't compared existing distance matrices from traditional methods (like 16S) vs the k-mer method.

This library, though it's written in Javascript, was developed off-hours during my former job. Sadly, I can't publish the source code, but I will try my best to show some of the graphs of analyses that I did to further the development and continue to approach the original questions.

#### What was the point of the examples?

Instead of going in to detail about the implementations I've been benchmarking, I focused instead on how a biological question or software roadblock inspired me to develop one novel algorithm and one competing k-mer library... which I can't legally pushlish... cause science is open and capitalism doesn't dominate the decision making process of leadership. Rather than be pessimistic or point fingers, I'd like to say that sometimes science can be stifled by process, but process gives rise to employability. So if you're interested in bioinformatics, what do you want to do with it?

The point of the examples was to explain that with some basic software development abilities, you'll be able to build a tool that others can use. Many universities don't explicitly teach methods development, but scientific progress lives and breathes on work that is open and willing to be criticized. You don't have to have a masters in CS and you don't have to be overwhelmed by the complexity e-commerce necessitates in the majority of tutorials available for novice programmers. Instead, you can restrict the 'type' of tool you develop to command line tools, obviously harder for most scientists to use...

The specific examples were mostly just... different programming scenarios for you to imagine what the complexity could look like. A per-base metric like my complexity index seems like a fairly linear algorithm, but it's heavily driven by I/O. A kmer profile is a 'simple' bioinformatics problem, but storing the data for larger choices of k can be prohibitive in terms of memory. My naive first guess was to store the k-mers and their counts in a hashmap. After I ran out of memory for fairly low choices of k, I had to look to competing algorithms for ideas as a first choice. Eventually I chose a `Uint32Array` for javascript, which can store a large number of k-mer counts. Restricting the unsigned integer choice limits the maximum number of reads that can constitute that array, but would provide a better memory profile. But is there a maximum array size? Before we go, I'd like to talk about a key thing for beginners to emphasize, maybe even above testing.


## Documentation



There are usually two or three components of documentation for mature Python packages and/or command-line tools. The first that users see is the markdown format `README.md`. This contains basic, high-level information about the repository: description, installation, CLI usage, basic API usage, information on expanded documentation, and licensing information. Several guides have been written for essential components of `README`s , and I prefer an [older Medium article](https://medium.com/@meakaakka/a-beginners-guide-to-writing-a-kickass-readme-7ac01da88ab3) as a guide for most cases (credits to Ryan G. for showing me this). Without repeating the contents of those guides, I'd like to mention that a build, coverage, chat, and other 'badges' are a nice 'dynamic' addition to a static `README.md` that provides additional information about code quality or resources for users to look at, and they're usually in a single line at the top of the file.

The next piece of documentation is contained in the repository itself. argparse `-h, --help` and/or manpages are usually sufficient for most command line interfaces. Sphinx docstrings and the auto-documentation described previously and/or vignettes on key classes and function usage can be helpful for developers supporting the project, for end-users, and to prevent redundant questions on the issue tracker.

Good documentation goes a long way to making your code high-quality for users to consider developing the project, reporting bugs, and providing feedback. You can't entirely prevent users from finding bugs or blindspots in your codebases, but type-checking and format validation can go a long way towards making sure that only essential issues remain on the issue tracker.

## Summary

To wrap up, building your own scientific command-line tool can be considerably challenging. As your software development skills grow, you'll find it easier to think of metrics and implementations for simple biological or chemical analyses. However, I think you'll be surprised at how many assumptions or questions users may ask about the implementation, and you can anticipate those questions by building a KnitR report to answer even seemingly simple questions and demonstrate how useful your program is. In the next part, I'll talk about a KnitR report I've been building for kmer.js and the steps I'm using to benchmark performance for each of the three calculations, in addition to producing some graphs of outputs on simulated and real-world data. I hope you'll remember some of my suggestions for best practices in CLI development. Thanks for reading!


## References


1. Anvar, Seyed Yahya, et al. "Determining the quality and complexity of next-generation sequencing data without a reference genome." Genome biology 15.12 (2014): 555.
