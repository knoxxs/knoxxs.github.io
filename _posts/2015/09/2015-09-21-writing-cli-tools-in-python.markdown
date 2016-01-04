---
layout: post
title:  "Writing cli tools in python"
date:   2015-09-21 20:13:00
categories: programming cli
tags: cli command-line scripts python
---

Its been long that I want to automate many things but the creepy and complex syntax of shell scripts doesn't let me do it. Somedays before while writing a script to monitor the memory usage of any process I decided to learn line scripts in python but never actually did it. Yesterday I saw [an article](http://nvie.com/posts/writing-a-cli-in-python-in-under-60-seconds/) from [Vincent](http://nvie.com/about/) about the same topic. So finally I am doing it :)

One more thing Vincent is great as in the end I completely copied this article. There is no addition needed from my side.

There is a good comparison done by Vincent:

1. **Language**: Shell has a creepy syntax and python is much more expressive.
2. **Distribution**: Python comes with its own set of problems, though. Python runtime environments are typically a mess which may pollute people's already cluttered global Python environments. With Python, installing a package is typically just a `pip install <pkg>` away, but it requires another tedious step: writing a `setup.py`.
    
    If it comes to distributing the script, a shell script may be much easier. With shell scripts it's either a single file that needs to be copied somewhere. Manually, or via a `make install` command, which involves adding a `Makefile` and dealing with subtle differences for each Unix platform, not to even mention trying to run it on Windows machines. 
3. **Argument Parsing**: Each script will at some stage require some options or arguments. How should we do the argument parsing? Do I use [`getopt` or `getopts`](http://unix.stackexchange.com/questions/62950/getopt-getopts-or-manual-parsing-what-to-use-when-i-want-to-support-both-shor)? Does it even matter? Can it take --long-form-options?                

## Solution
Lately, a few fantastic projects have taken away most of the tedious work surrounding the building of command line tools, and almost make it trivial now.

### Click 
[Click](http://click.pocoo.org/) is a Python library written by [Armin Ronacher](https://twitter.com/mitsuhiko) that deals with all the handling of command line option and argument parsing and comes with fantastic defaults. This project is a great step towards more consistent and standard CLI interfaces. Besides solving the options and argument parsing, it also has a ton of useful features packaged, like smart colorized terminal output, file abstractions, subcommands, and rendering progress bars.

### pipsi
Using [pipsi]() (also by [Armin](https://twitter.com/mitsuhiko)), users can install any Python command line script into an isolated Python runtime environment, so it solves the global cluttered Python environment problem entirely.

### Cookiecutter

[Cookiecutter](http://cookiecutter.readthedocs.org/) (by the awesome [Audrey Roy Greenfield](https://twitter.com/audreyr)) is a project generator, based on a predefined project template. It will read the template, ask the user a few questions to fill in the blanks, and generates a new project for you.

### cookiecutter-python-cli

[cookiecutter-python-cli](https://github.com/nvie/cookiecutter-python-cli) is one such Cookiecutter template [Vincent](http://nvie.com/about/) wrote that uses all of the above: it sports a predefined setup.py, a package structure that's extensible, and test cases and a test runner to get you started.

## Example

1. First, install pipsi and follow its instructions:
    
    ```bash
    $ curl https://raw.githubusercontent.com/mitsuhiko/pipsi/master/get-pipsi.py | python
    ```
2. Next, using pipsi, install Cookiecutter in its own isolated runtime environment:
    
    ```bash
    $ pipsi install cookiecutter
    ```
3. Now use Cookiecutter to create your brand new project, based on my CLI template:
    
    ```bash
    $ cookiecutter https://github.com/nvie/cookiecutter-python-cli.git
    Cloning into 'cookiecutter-python-cli'...
    remote: Counting objects: 107, done.
    remote: Total 107 (delta 0), reused 0 (delta 0), pack-reused 107
    Receiving objects: 100% (107/107), 12.82 KiB | 0 bytes/s, done.
    Resolving deltas: 100% (51/51), done.
    Checking connectivity... done.
    full_name (default is "Vincent Driessen")? Abhishek Gupta
    email (default is "vincent@3rdcloud.com")? abhi.bansal21@gmail.com
    github_username (default is "nvie")? knoxxs
    project_name (default is "My Tool")? sample_python_cli
    repo_name (default is "python-mytool")? sample_python_cli
    pypi_name (default is "mytool")? sample_python_cli
    script_name (default is "my-tool")? sample_python_cli
    package_name (default is "my_tool")? sample_python_cli
    project_short_description (default is "My Tool does one thing, and one thing well.")?
    release_date (default is "2015-04-15")? 2015-09-20
    year (default is "2015")?
    version (default is "0.1.0")?
    ```
4. When you're done, you'll have a project where you can run tox to run your test suite on all important Python versions. If you don't need the test cases, simply remove the tests/ directory.
    
    ```bash
    $ tox
    ```
5. Let's install and run it without further modifications:
    
    ```bash
    $ pipsi install --editable .
    Running virtualenv with interpreter /Users/aapa/.local/venvs/pipsi/bin/python
    Using real prefix '/System/Library/Frameworks/Python.framework/Versions/2.7'
    New python executable in /Users/aapa/.local/venvs/sample-python-cli/bin/python
    Installing setuptools, pip, wheel...done.
    Obtaining file:///Users/aapa/Projects/sample_python_cli
    Collecting click (from sample-python-cli==0.1.0)
      Using cached click-5.1-py2.py3-none-any.whl
    Installing collected packages: click, sample-python-cli
      Running setup.py develop for sample-python-cli
    Successfully installed click-5.1 sample-python-cli
      Linked script /Users/aapa/.local/bin/sample_python_cli
    Done.
    ```
6. Check it
    
    ```bash
    $ sample_python_cli
    Hello, world.
    $ sample_python_cli --as-cowboy
    Howdy, world.
    $ sample_python_cli --as-cowboy aapa
    Howdy, aapa.
    ```
    

## Reference

1. [Writing a Command-Line Tool in Python](http://nvie.com/posts/writing-a-cli-in-python-in-under-60-seconds/)
2. [getopt, getopts or manual parsing - what to use when I want to support both short and long options?](http://unix.stackexchange.com/questions/62950/getopt-getopts-or-manual-parsing-what-to-use-when-i-want-to-support-both-shor)
3. [Click](http://click.pocoo.org/)
4. [Armin Ronacher](https://twitter.com/mitsuhiko)
5. [pipsi](https://github.com/mitsuhiko/pipsi#readme)
6. [Cookiecutter](http://cookiecutter.readthedocs.org/)
7. [Audrey Roy Greenfield](https://twitter.com/audreyr)
8. [cookiecutter-python-cli](https://github.com/nvie/cookiecutter-python-cli)