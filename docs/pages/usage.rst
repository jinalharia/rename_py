Usage
========

.. role:: bash(code)
   :language: bash

Command line interface
-----------------------

Enter :bash:`py-rename -h` for help

.. code-block:: bash

    $ py-rename -h

    py-rename - Python rename tool for multiple files

    Usage:
    py-rename [OPTIONS] COMMAND [COMMAND-OPTIONS]

    Positional arguments:
        rename              rename files based on regex pattern
        match               rename files based on regex pattern
        prefix              prefix filenames with prefix string
        postfix             postfix filenames with prefix string
        lower               convert filenames to lowercase
        replace             replace spaces in filenames to _
        camelcase           convert filenames to camel case

    Options:
      -h, --help            show this help message and exit
      -V, --version         show program version number and exit
      -s, --silent          do not print output (default: False)
      -n, --dryrun          Dry run: print names of files to be renamed, but do not
                            rename (default: False)
      -f, --full            Match only full filename against pattern (default:
                            False)

Subcommand usage example:

.. code-block:: bash

    $ py-rename rename -h

    Positional arguments:
      pattern      regex pattern to match filenames
      replacement  replacement regex pattern for renamed files

    Options:
      -h, --help   show this help message and exit

Examples
----------

Example 1 - Renaming based on regex
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Imagine you have some files awfully named like this:

- ``ab12+Red+(0000).txt``
- ``ab12+Red+(0001).txt``
- ``ab12+Red+(0002).txt``

and you want to rename all of them in manner ``01-Red.txt`` (extracting number from the end and put it at the beginning and shortening it to 2 digits).

Step 1: Test matching pattern
++++++++++++++++++++++++++++++++

Regex pattern to match those files and extract 2 digit number should be like this: `.+\(00(\d{2})\).+`

.. code-block:: bash

    $ py-rename match ".+\(00(\d{2})\).+"
    ('matched ab12+Red+(0000).txt',)
    ('matched ab12+Red+(0001).txt',)
    ('matched ab12+Red+(0002).txt',)
    ('files matched: 3',)


Step 2: Test replacement pattern using dryrun flag
++++++++++++++++++++++++++++++++++++++++++++++++++++

.. code-block:: bash

    $ py-rename -n rename ".+\(00(\d{2})\).+" "\1-Red.txt"
    Performing DryRun: No actions will be taken
    ('renaming: ab12+Red+(0000).txt --> 00-Red.txt',)
    ('renaming: ab12+Red+(0001).txt --> 01-Red.txt',)
    ('renaming: ab12+Red+(0002).txt --> 02-Red.txt',)
    ('files matched: 3',)

Step 3: Actual renaming
++++++++++++++++++++++++++++++

.. code-block:: bash

    $ py-rename rename ".+\(00(\d{2})\).+" "\1-Red.txt"
    ('renaming: ab12+Red+(0000).txt --> 00-Red.txt',)
    ('renaming: ab12+Red+(0001).txt --> 01-Red.txt',)
    ('renaming: ab12+Red+(0002).txt --> 02-Red.txt',)
    ('files matched: 3',)

Example 2 - Add prefix string or postfix string to files
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Imagine you have some files named like this:

- ``00-Red.txt``
- ``01-Red.txt``
- ``02-Red.txt``

Add prefix string:

.. code-block:: bash

    $ py-rename prefix "test_"
    ('renaming: 00-Red.txt --> test_00-Red.txt',)
    ('renaming: 01-Red.txt --> test_01-Red.txt',)
    ('renaming: 02-Red.txt --> test_02-Red.txt',)
    ('files matched: 3',)


Add postfix string:

.. code-block:: bash

    $ py-rename postfix "_test"
    ('renaming: 00-Red.txt --> 00-Red_test.txt',)
    ('renaming: 01-Red.txt --> 01-Red_test.txt',)
    ('renaming: 02-Red.txt --> 02-Red_test.txt',)
    ('files matched: 3',)

