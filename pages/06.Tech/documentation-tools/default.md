---
title: 'Documentation Tools'
---

!! This has some work needed to make corrections and update what I've learned.

# Preamble & Objective:

Given the many options for putting word-to-page, documentation is often a difficult by-product of complex projects. In software, it is often the most cast-off segment of work that has some of the most time-constrained effort. The traditional cycle of define, create, test, then document is never optimal. Add on top the need to iterate using tools that are designed for the static document, particulary the "word processor", and you end up in a cycle that keeps documentation away from those creating and playing a game of "telephone" with the Technical Writing team.

In our team, this was the end goal:

* Get developers to make the first pass at documents, since they know what they are doing
* Get the technical writer to be a curator and formatter, not a creator of content
* Create a system where the documentation can live alongside features, stored within the code files
* Generate the latest documentation using the software build process
* Make the documents available offline, in HTML and PDF format

This certainly seems like a lofty goal, and one that could become unweildy without a good working system. We looked long and hard at online systems as a service, and in the end found that our organisation wanted the flexibility to write without the dependency of a third party, and the lack of control that offered. If we were going to tackle all of these goals, we needed to do them with tools we control. The funny part is that while the budget wasn’t much of an object, we found what we needed for free.

Ya, really. $0.

# What’s the Toolbox?

The first question of every project is to decide what you’re going to plumb together and create. It took some maturisation, but when we got there, it was pretty simple.

### Sphinx + Atom, with Github, Ubuntu, and VirtualBox.

Our code team already uses Github, so the thinking was that files and version control could already be handled if we start with Github. This makes sense because of a second reason, and that’s the concept that Github offers review tools for text, and the Pull Requests and Merging needed. Then, where features are located, we could separate out that “section” of a document and “assemble” those “sections” back into a cohesive document.

Sphinx is a documentation system that is built with Python, and uses reStructuredText for content. It can also use MarkDown, but it’s limited. That doesn’t mean it can’t play an important role, however. If MarkDown is a must for your documents, it’s still generally available this way.

Sphinx acts very similarly to a static HTML generator, but it does some special formatting related to documentation using RST, and it also has the power of includes, something MarkDown doesn’t have.

Atom is a text editor, free from Github. The main benefit is that one of the packages available in Atom is an RST rendering preview to go alongside its built-in Github tools. This means you can mostly work from Atom, and for those that sling text, that’s not a bad option.

We chose to use Ubuntu in a virtual environment to isolate our build process and keep it super-clean, and portable, up until it was turned into a Docker. While it can certainly be done, Docker is a bit too build oriented here. We’ll get it built using a networked VM, but you can take it as far as you want. For us, it was key because we use Jenkins to build with Dockers. This allows us to build on-demand, and then take the artifacts directly into the build output.

If the above paragraph didn’t make sense, it’s ok. It just means that your IT Admins will be able to help you implement this system into a more automated manner if that’s what you need. If not, you can generate your documents locally, all just as easily.

Process
=======

The start of the process is to put together the local environment for converting text into shippable documentation. The way to put this together is OS-agnostic if you want. We used a VirtualBox environment to install a desktop (UI-yes) version of Ubuntu linux. We all work on all three major OS versions, so creating an environment that was separate and accessible was what made sense. The alternative is obviously to create a hardware box with linux, or run everything in a different OS. It’s preference; there is no requirement to run Sphinx on linux, but for us it made sense.

Once the linux environment exists, it’s time to install Sphinx and its parts. Fear not; here’s a script. To run a script in Ubuntu, you’re going to have to go into a command line, navigate, and ./filename

```sh
# Update the system from the command line
sudo apt-get update -y
sudo apt-get upgrade -y
sudo apt-get dist-upgrade -y

# Hang on a moment
sleep 4s

# Install the Sphinx parts

# Installs Python PIP
sudo apt-get install python-pip

# Updates PIP
sudo pip install --upgrade pip

# Install latest Sphinx version
sudo pip install sphinx

# Install a handy template
sudo pip install sphinx_rtd_theme

# Install the pdf creation tool
sudo apt-get install texlive-full -y

# Install the MarkDown converter
sudo pip install recommonmark
```

Once this is all put together, you can start using Sphinx from the command line. It’s not as scary as it seems.

The easiest thing to do is to set up a project folder inside /Documents. Don’t worry, Sphinx will do everything for you. All you have to do is answer a few questions. In order to get started, start Terminal, and go to the folder you want to use. Assuming it is Documents, then you’ll just run the Quickstart. Important thought, though. If you are publishing ***more than one document* **the best option is to make a directory in /Documents based on its title, and go into it. For each document, repeat the Quickstart.

```sh
$ sphinx-quickstart
```

You don’t have to follow all of these answers, but these are what I recommend for the easiest, most flexible environment:

* Source & Build - Yes
* Underscore prefix - accept
* Project Name - Whever
* Author Name - That’s you
* Version = x.x, Release = x.x.x whatever makes sense
* Language /[en] - accept
* Source suffix - accept
* Special document "index" - accept
* Add ePub - yes
* Autodoc - yes
* Doctest - yes
* Intersphinx - yes
* Todo - yes
* Coverage - yes
* imgmath - no
* Mathjax - yes
* Ifconfig - yes
* Viewcode - yes
* Githubpages - no
* Create makefile - yes
* Windows Command file - no

Again, you want to run this Quickstart for EACH of the documents you want. This creates all of the initial files and instructions necessary for Sphinx to generate static HTML and a PDF per document.

Once you finish this, you can immediately create a blank-ish document based on these settings very quickly.

```sh
make html
```

This quick command will take the source and other files and create a mostly empty run of the document, placing the assets into the /build directory. The reason you want to select yes “Source & Build” in the Quickstart is that you will separate your generated content from the source files by directory. The build folder can always be deleted! In reality, every time you run a command to create documents, Sphinx will replace the content with the current sources.

HELLO WORLD
===========

Now that you have some output, what you need to check is the mechanics of getting content into your document. There are just some very basic principles:

index.rst is the list of where Sphinx draws its content when writing

Files that are named in the list within index.rst should be listed without their file extension

Sphinx will combine all of the files in that list and stack them together

The key concepts here are that Sphinx is designed to create a document that you chop up any way you wish. This means you can order by chapter, part, or just make one monsterous file. The split should help if you plan properly, since you might want to re-order chapters or whatnot. Just make sure that when you create file\_one.rst and file\_two.rst, you list them as:

```sh
file_one
file_two
```

The order matters, as Sphinx compiles in order, top-down. This allows you to segment your content into logical groupings. The individual .rst files can be handled by different writers as needed, and the document will compile without having to do any kind of copy-paste exercise you might otherwise have to do on a single document file.

CREATE CONTENT
==============

Creating content is always going to be the part of the equation that is associated with whomever is making that material. With that in mind, there are several formatting conventions that will help to organise and lay out the content to be readable and quickly absorbed by your reader.

To start with, as mentioned above, there are two markup languages that Sphinx can read. MarkDown and reStructuredText. I am going to strongly recommend using RST for a few reasons, but, if your shop is MarkDown 100% all the time, you can certainly do that. Some things to consider:

If you write in RST, tables will render. As of this writing, MarkDown tables are… unfinished.

If you use RST, it offers more style choices, providing a better experience.

If you use MarkDown, you will still have to use RST in order to include .md into the system.

Let’s go through these…

Tables are complicated beasts for text tools to render. ReCommonMark is the module you can add to your system, and if you do, you can end up using .md files. However, table creation under .md/reCommonMark, table creation hasn’t been consistent.

RST offers more choice. How so? Two very important examples… First, admonitions. Essentially a sort of call-out box, it clearly calls into focus important, style-specific information. Second, it offers includes. Just as with other web languages, when rendering, RST can grab and insert a text file. This means that repetitive content can be reused, and written less often. This is a critical concept for any documentation that needs the same content in multiple areas, or multiple projects.

Very technically, you can get away with 100% MarkDown by using a listing of .md files within index.rst, but you cannot do any including when using MarkDown. That requires maintenance with the index file, and no include options.

Follow the guidelines provided by Sphinx for creating your documents, all within the /source folder.

Rather than imposing a fixed number and order of section title adornment styles, the order enforced will be the order as encountered. The first style encountered will be an outermost title (like HTML H1), the second style will be a subtitle, the third will be a subsubtitle, and so on.

Below are examples of section title styles:

===============
 Section Title
===============

---------------
 Section Title
---------------

Section Title
=============

Section Title
-------------

Section Title
`````````````

Section Title
'''''''''''''

Section Title
.............

Section Title
~~~~~~~~~~~~~

Section Title
*************

Section Title
+++++++++++++

Section Title
^^^^^^^^^^^^^

CHANGES
=======

Some of the changes you’ll need to make follow:

Using the ReadTheDocs template -

There is no requirement for you to change templates, but it is common that you’ll want to not use the basic “Alabaster” theme that is installed within Sphinx. Each template may have some different requirements, but for the RTD theme, follow these steps:

Copy this into the HTML Output section of the project’s conf.py file, replacing as appropriate

​```python
# -- Options for HTML output ----------------------------------------------

# The theme to use for HTML and HTML Help pages.  See the documentation for
# a list of builtin themes.
#
import sphinx_rtd_theme

html_theme = "sphinx_rtd_theme"

html_theme_path = [ "_templates", ]

#html_theme_path = [sphinx_rtd_theme.get_html_theme_path()]

html_theme_options = {
    "display_version": "false",
    "collapse_navigation": "true",
}

html_logo = "pathtoyourlogo.png" # <- change as appropriate

# Theme options are theme-specific and customize the look and feel of a theme
# further.  For a list of options available for each theme, see the
# documentation.
#
# html_theme_options = {}

# Add any paths that contain custom static files (such as style sheets) here,
# relative to this directory. They are copied after the builtin static files,
# so a file named "default.css" will overwrite the builtin "default.css".
html_static_path = ['_static']
​```

Then add sphinx\_rtd\_theme into the /\_templates folder.

For the MarkDown parser to get used, substitute this text within your conf.py file

​```python
# import sys
# sys.path.insert(0, os.path.abspath('.'))

from recommonmark.parser import CommonMarkParser

source_parsers = {
    '.md': CommonMarkParser,
}

# -- General configuration ------------------------------------------------

​```

You might have noticed that the last line of the configuration installs this parser. But there’s one more step, and that is to ensure you can use .md as an acceptable file extension for your index.rst. Substitute the following as shown:

​```python
# The suffix(es) of source filenames.
# You can specify multiple suffix as a list of string:
#
source_suffix = ['.rst', '.md'] # <- or just add '.md' here


# The master toctree document.
master_doc = 'index'

​```

If you don’t use MarkDown, you don’t need to do this. Sphinx would only look at .rst files in that case. If you don’t want to change templates, you don’t need to.

CONCLUSION
==========

Now that you have the mechanics available to create a documentation system, it’s up to you to fill in the content. Make sure to name your files in a topology that is readable individually.

Two functions that you may find useful after this is created\>

First, there is an option to do a “GLOB” within Sphinx. This means that if you name your files in a method that is either alphabetical or reverse, you won’t even need to fill them into index.rst - simply name/number them in order and they will move around according to that name. This has been helpful in the process of creating a Technical Notes project. Each is independent, but when they are written, they get an “index number”, like TN-012\. These are permanent, and don’t require index maintenance.

This can also be handy with release notes. If your release notes follow a semantic numbering system, you might find it’s easier to simply create the numbered files for each version. Make sure your toctree is at a proper level based on your approach.

The other concept you may find helpful is to do . . include filename.rst :: within a “master” document, and pass out the sections of your documentation to the features. If you’re working with software, the documentation about a specific feature may be held within Git or some other file control system. You can then assign and review changes within the documentation at the time that features are updated. This allows developers the ability to amend documents without having to go into yet-another-system. If you can make sure that the includes get pulled in using your build process, then your build process can run and script and build your docs at build time. This makes the process of publishing trivial, and allows you to get on with your work!

Hopefully this provides some starting information into making a simple, web + pdf documentation system within your own organisation’s tooling. Enjoy!


`````````````