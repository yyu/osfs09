Operating System From Scratch
-----------------------------

Step 09: File System
````````````````````

We will design and implement a highly simplified FS.

Read/Write the hard disk
''''''''''''''''''''''''

We will use a HD Driver, which is a TASK, to read/write the hard disk (see ``a/``, ``b/``, ``c/``).

Orange'S FS
'''''''''''

Let's think about what is necessary in an FS:

1. we need some place to store the information of each file, such as file name, size, which sectors belong to this file, etc.
2. we need some place to store the index of the files
3. we need some place to store the usage of each sector
4. we need some place to store the metadata

These are the minimal needs. It can't be less.
We don't need a product quality FS, so let's make it as simple and as stupid as possible.

Here's my design (see ``d/``):

1. We use *i-nodes* (just like that in \*nix, but highly simplified) to store the information of files.
2. We use a special file to store the index of all other files.
   This special file is called *root directory* (denoted as ``/``), which is the only directory in the FS.
   In other words, this is a *flat FS*.
3. We use a *sector-map* to store the usage of all sectors.
4. We use a special sector, which is called a *superblock*, to store all the metadata,
   such as where the sector-map is, where the root directory is, where the i-nodes are, etc.

::

        Orange'S FS illustration
        ========================
                                                       files data
                                                           |
                                                 /------------------...
        +---+---+---+---+-...-+---+---+-...-+---+---+-...-+---+---+-...
        |   |   |   |   | ... |   |   | ... |   |   | ... |   |   | ...
        +---+---+---+---+-...-+---+---+-...-+---+---+-...-+---+---+-...
         |    |   |  \-----------/ \-----------/ \-----------/
         |    |   |        |             |             |
         |    |   |        |             |            root dir
         |    |   |        |            inode_array
         |    |   |       sector-map
         |    |  inode map (sect2)
         |   superblock (sect1)
        boot sector (sect0)

System calls
''''''''''''

We will add several more system calls so that the TASKs and PROCs can use the FS:

+ ``open()`` (see ``e/``)
+ ``close()`` (see ``e/``)
+ ``read()`` (see ``f/``)
+ ``write()`` (see ``f/``)
+ ``unlink()`` (see ``h/``)


TTY and FS
''''''''''

You know that everything in \*nix is file.
Let's adopt this idea and treat TTYs as files.
See ``i/``.

Remember to modify ``printf()`` when TTY is changed (see ``j/``).

`‹prev`_   `next›`_

.. _`‹prev`: https://github.com/yyu/osfs08
.. _`next›`: https://github.com/yyu/osfs10
