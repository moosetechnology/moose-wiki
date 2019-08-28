# VerveineJ

VerveineJ is a tool written in java that create a _mse_ file from Java source code.

## Installation

To install VerveineJ, you need to clone it form [VerveineJ github repositiory](https://github.com/moosetechnology/VerveineJ).

```bash
# https
git clone https://github.com/moosetechnology/VerveineJ.git

# ssh
git clone git@github.com:moosetechnology/VerveineJ.git
```

## Usage

To use VerveineJ, you can use [Famix Maker](https://github.com/moosetechnology/Moose-Easy) ![External documentation](https://img.shields.io/badge/-External%20Documentation-blue) or from a terminal.

In the following, we describe the option of VerveineJ.

Usage:

`VerveineJ [-h] [-i] [-o <output-file-name>] [-summary] [-alllocals] [-anchor (none|default|assoc)] [-cp CLASSPATH | -autocp DIR] [-1.1 | -1 | -1.2 | -2 | ... | -1.7 | -7] <files-to-parse> | <dirs-to-parse>"`


|    VerveineJ options    | description                                                                                                                                                                                                                                                                                   |
| :---------------------: | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|          -h           | prints this message                                                                                                                                                                                                                                                                           |
|          -i           | toggles incremental parsing on (can parse a project in parts that are added to the output file)                                                                                                                                                                                               |
| -o <output-file-name> | specifies the name of the output file (default: "+VerveineParser.OUTPUT_FILE+")")                                                                                                                                                                                                             |
|       -summary        | toggles summarization of information at the level of classes. Summarizing at the level of classes does not produce Methods, Attributes, Accesses, and Invocations. Everything is represented as references between classes: e.g. \"A.m1() invokes B.m2()\" is uplifted to \"A references B\". |
|                         |                                                                                                                                                                                                                                                                                               |
|      -alllocals       | Forces outputing all local variables, even those with primitive type (incompatible with \"-summary\")")                                                                                                                                                                                       |
|     -anchor (none\|entity\|default\|assoc) | options for source anchor information: - no entity - only named entities \[default\] - named entities+associations (_i.e._ accesses, invocations, references) |
| -cp CLASSPATH | classpath where to look for stubs)|
| -autocp DIR |  gather all jars in DIR and put them in the classpath |
| -filecp FILE | gather all jars listed in FILE (absolute paths) and put them in the classpath |
| -excludepath GLOBBINGEXPR | A globbing expression of file path to exclude from parsing |
| -1.1 \| -1 \| -1.2 \| -2 \| ... \| -1.7 \| -7 | specifies version of Java |
| \<files-to-parse>\|\<dirs-to-parse> | list of source files to parse or directories to search for source files |
