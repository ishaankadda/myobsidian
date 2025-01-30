# `pip install` - what happens exactly?
First, pip needs to decide on the distribution to install. there are 7 kinds of distributions, but the most common these days are *source distributions* and *binary wheels*. A source distribution contains a `setup.py` file along with raw Python and C extension code. A binary wheel contains compiled C extension code, pre-built and specific to the architecture and often the OS it was compiled on. 

`pip` selects the distribution based on the text of the links inside `https://pypi.org/simple/<somepackage>`. There, the text of the links is the filename of the distribution, encoding the version, kind of distribution, and architecture and OS.




