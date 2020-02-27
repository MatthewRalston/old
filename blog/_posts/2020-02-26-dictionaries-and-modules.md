---
title: Sorting Out Modules in Python
subtitle: Modules and importing for beginners
image: /assets/img/python_import_stringio.jpg
hero_image: /assets/img/xvideos_python_programming.png
tags: python beginner noob
toc: true
---


<div style='position:relative; padding-bottom:calc(64.71% + 44px)'><iframe src='https://gfycat.com/ifr/OfficialFlatAnnelida' frameborder='0' scrolling='no' width='100%' height='100%' style='position:absolute;top:0;left:0;' allowfullscreen></iframe></div><p> <a href="https://gfycat.com/officialflatannelida">via Gfycat</a></p>

<!-- <video controls autoplay>
	<source src="/assets/img/LearnSortingWithKronk.mp4" type="video/mp4">
	Your browser does not support the video tag.
</video> -->

## Oh yeah, it's all coming together

If you're comfortable with the differences between dictionaries and modules, skip ahead to the section about modules where I explain directory layout and importing and contrast modules with object hierarchies.

## What are dictionaries again?

Imagine a magical backpack that can store as much information as you can fit in memory. The one rule of using the magic backpack, is that **each time you need a new pocket or location to store something in it, you have to give the backpack a name** to reference the new information with.

```python
hashmap           = dict() # or {}
hashmap["along"]  = "hello world!"
hashmap["a_list"] = ['a', 'b', 'c'] 
# Try not to use a row id as the key to the record!
#hashmap["someId"] = {"dont": "do", "this": "here"}
hashmap["DND"]    = {"id": "someId", "try": "this", "here": "instead"}
# This is *less* efficient if you're repeatedly 'searching' for records.
# But most scripts will process all or groups of records equally, 
# and that doesn't require searching in this way
```


### Issues with Nesting

Forgetting the name, or a `KeyError`, is a common error for beginners to struggle with. To minimize the number of KeyErrors, it is common to store the key in a variable or list for convenient access (imagine reading a table's column headers with `.readline()`). 

Another cause of the `KeyError` is a complex nested structure of lists and dictionaries with a complex accessing strategy. To DRY up your code a little, a lambda expression might be the right tool to access your buried data!

```python
>data[0]["records"][1]["id"]  # Too complex! Frequent KeyErrors

# Instead, hardcode the path you know exists in every record as a lambda
>get_first_id_from_datum = lambda x: x["records"][1]["id"] 
>for d in data:
>  get_first_id_from_datum(d) # Much cleaner! Less KeyErrors
```

Remember that even though the `KeyError` may be simple to resolve, *discovering* an error is the expensive part, it's not just about how extreme or how deleterious the error might seem (like with respect to closed threads/processes/connections).


### What's with the brackets?

The bracket syntax reminds us that finding one object out of many with a dictionary is a lot like finding something in a list (or an array)! Instead of using a numerical index to reference a data element, the hashmap, or dictionary, puts a string through a special type of mathematical function called a hash function. 

The hash function can digest keys (strings of different lengths) into entropically distinct strings that can be used with a standard lookup table. But remember that the computer doesn't need to store the key, but you need to keep both the keys and the values in mind while you're programming.

{% include note.html content="Having predictable names for dictionary members is a lot like naming variables in your code. Why?" %}

Also, dictionaries can be useful when storing system-wide configurations. Alternatives include using a module (or similarly an `.ini`), or using [global variables](/assets/img/pitbull_worlwide_global_variable.jpg). Some variables or configurations might not be available until runtime, and passing them into functions together prevents the need for complicated function signatures. If none of the previous statement made sense, I'd look up Python3's signature feature: type hints in function signatures!

## Modules

Ah, so now that we've touched the basics on accessing data with hashed strings or 'keys' with dictionaries, what are the biggest differences between dictionaries and modules?

Well, modules (and the submodule) are often pieces of Python code that can be loaded into memory with a `import` statement. Occassionally, they can be config files (`config.py` is a common names) that store global, program-wide configurations for the way the program functions. Dictionaries store data. Modules often store code.

{% include tip.html content="If modules store code, that means they can be installed with <code>pip</code>. <a href='https://realpython.com/what-is-pip/'>Click here</a> to learn more about module installion." %}


Also, modules have a very different syntax for accessing data. They use a dot syntax similar to what some readers may have seen in object/class hierarchies in other languages. For example, BioPython users may need to familiarize themselves with `Seq` and `SeqRecord` objects. Unless you import those modules directly, you'll have to access the classes for those objects from their submodules underneath the main module.

### Modules, submodules, classes, and objects

That last sentence was confusing! What did it mean?

Okay, so let's take the BioPython module `Bio` for example and let's see how importing different submodules works. Below, the for loop is 'iterating' or looping through records being read from a fasta file 'example.fasta'. The `record` object in this case, is a `SeqRecord` object. But wait, we didn't load the `SeqRecord` submodule at all! How is the parsers making those records if we haven't loaded it?


```python
# This is how you would import the whole BioPython module
# import Bio

# Instead, we'll just import a submodule
from Bio import SeqIO # Import the SeqIO submodule

for record in SeqIO.parse("example.fasta", format="fasta"):
  print(record) # Prints a SeqRecord object
  print(record.seq) # Prints a Seq object
# If instead we use import Bio, we'd have to use the much longer dot syntax
import os

extension = os.path.splitext()[-1] # Wow that's getting complicated too!
```

It turns out that the submodule `SeqIO` has already loaded the `SeqRecord` submodule for us, which in turn has loaded the `Seq` submodule. How confusing! Behind the scenes, there is a lot of importing going on, and we got a lot of mileage out of the much simpler submodule! We have access to the `Seq` objects, including reverse complementation methods, for free!

### module.function or class.method

Okay, so now we know that importing submodules can be just as effective as loading the whole module. Now what? Well, did you notice how we didn't need to 'call' `SeqIO()` or assign its return to an instance or variable name like below?

```python
from bio import SeqIO
# This would have been more tedious.
# SeqIO would be the name of a class.
# seq_parser *would* be the name of the instance/object.
seq_parser = SeqIO() 
# The following *would* be an example of a 'method' call
for record in seq_parser.parse("example.fasta", format="fasta"):
```

That means we are working instead with a module, because we could use the function `parse` under the `SeqIO` 'namespace' (a place to store names of constants/variables, functions, and classes) without building an object first. While we're on the subject of modules vs classes/objects, the last line above would have been an example of a 'method' call. A method is a fancy name for a function that is tied to an instance or object created from a class. 

But notice how the call to `SeqIO.parse` in the first for loop example is similarly structured to how the method call would be structured in the second example. The function or method `.parse` follows the module or object's name. In the first example, we reference the submodule directly without assignment. In the second example, `seq_parser` is a variable that we have deliberately chosen to have a helpful name. It could just as well be called `foo` and the method call would be `foo.parse`. 

This syntax doesn't have a formal name but in this blog I'll refer to the concept of `module.submodule`, `module.function`, `class.method`, or `module.class` as the dot syntax. The dot syntax results from where in the module's hierarchy of modules and submodules a piece of code (the class or function) is defined. You might note that the module hierarchy can be described as a tree.

In short, importing code in Python can seem intimidating at first without a primer on the foundations of the hierarchical system. But for students of bioinformatics and biology, the hierarchy and inheritance properties might seem extremely logical. 

### How to build your first module

From the [Python documentation](http://docs.python.org/3/tutorial/modules.html):

>If you quit from the Python interpreter and enter it again, the definitions you have made (functions and variables) are lost. Therefore, if you want to write a somewhat longer program, you are better off using a text editor to prepare the input for the interpreter and running it with that file as input instead. This is known as creating a script. As your program gets longer, you may want to split it into several files for easier maintenance. You may also want to use a handy function that you’ve written in several programs without copying its definition into each program.

Modules are incredibly useful. You've probably seen some modules that can even be downloaded to your computer through a tool called `pip` via the Python Package Index (PyPI). These packages are registered with the index and are typically importable, with BioPython and boto3 being excellent examples.

But how do we make a module? What files are necessary to compose a module? A module can be *either* a single file **or** a directory. That's it! It's that simple. A module in Python is either a single file `mymodule.py` becomes `import mymodule` and you're ready to go! Or if your module is a directory, you would still use `import mymodule` to import the code in `mymodule/`. 

Whatever the case may be, the code of your module (either the file **or** the directory) must be known by the `PYTHONPATH` variable, which typically points to a standard location on Linux like `path/to/python/lib/python3.7/site-packages/`.  So `$PYTHONPATH` is a shell variable (like `$PATH`) that controls how Python is able to find a module on the filesystem. Okay so far?

{% include note.html content="To read the preceeding path, note first that Python can be installed in principle in several locations on your machine. I use a version manager called <a href='https://github.com/pyenv/pyenv'>pyenv</a> to manage my Python versions and virtual environments. On my machine, my path is <code>home/matt/.pyenv/versions/python3.7</code>. In this case, that takes the place of the phony <code>path/to/python</code>. So therefore modules are installed into the <code>lib/</code> directory of the python installation, under the category of <code>python3.7/</code>-associated libraries and under the subdirectory <code>site-packages/</code>. Wow!" %}

{% include warning.html content="Note that <strong>language version managers</strong> like rvm and pyenv are different types of software from <strong>version control systems</strong> like csv, svn, and git." %}

So what goes in the single-file module? Anything. No really. You can put whatever you want into a single file. I'd suggest making the major methods well documented and including them in your `README.md`, wiki, or Sphinx documentation if you can.

How about in a directory-style module? This is where it gets one step trickier than **anything** lol. Uh, you have to put this file called `__init__.py` in the directory named `mymodule` and that's basically it. Everything else that goes into the directory becomes code associated with `mymodule` and can be source like this: `from module import submodule`.

The directory structure looks roughly like this:

```bash
mymodule/
    __init__.py
    submodule1.py
    submodule2.py
README.md
setup.py
```


## Review

Okay! We covered more material in this post than you might realize. We learned some dos and don'ts of dictionaries. We learned some basic principles of modules and the import system. Then we learned that module/submodule, superclass/class, and module/function relationships can be defined with hierarchical tree structures. And finally, we learned how to build a module from either a simple file or from a directory with an `__init__.py` file in it. 

I'm glad you read this article! If you enjoyed the content, be sure to check out the beginner tag and upvote the article on social media! Thanks for your support.
