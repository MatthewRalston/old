---
title: On Python Documentation with Sphinx
tags: python sphinx
---

I've been exploring the Python documentation system for ReadTheDocs with Sphinx. It could be that I'm dense, but the `sphinx-quickstart` command seems horribly divorced from most of the automagic documentation goals of the Sphinx project. After following several guides<sup>[1][1], [2][2]</sup> I noticed that only an index.rst had been created in the `_sources` directory. Brandon's guide suggests that adding your module to either your PYTHONPATH or to your conf.py before `make html` would lead to autodoc recognizing your package. I wasn't able to get this to work on OSX Sierra using a virtualenv. After looking through more tutorials, I encountered some suggestions for the `sphinx-apidoc` command, without actual invocation details.


So it seems that to generate the documentation for a new module, you must run `sphinx-apidoc` to generate the module's `.rst` file, and then run `make html` to generate the html. It's remarkably easy when you figure it out, but kind of poorly documented considering the docs come from Sphinx and RTD directly.

I suppose I assumed that `sphinx-quickstart` would have an option to specify existing module/lib directories and automatically bootstrap a Python package/project to make it a simpler experience. I highly recommend sphinx-mode.el for Emacs users, to automatically generate Sphinx-format docstrings and capture most of the function signature.




[1]: https://media.readthedocs.org/pdf/brandons-sphinx-tutorial/latest/brandons-sphinx-tutorial.pdf
[2]: http://www.sphinx-doc.org/en/stable/tutorial.html
