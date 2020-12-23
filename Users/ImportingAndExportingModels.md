# Importing and exporting models

The first step in the process of analysis is the generation of a model of a given target system or set of data.
Moose can handle multiple types of data and data sources.
This chapter provides a short guide for how to deal with these.

## Importing and exporting with MSE

The preferred way to load a model in Moose is via an MSE file.
To load an MSE file, all you have to do is to press the _``Import from MSE''_ button in the Moose Panel and indicate the file to load.
This creates a model, populates it with the entities from the file and adds the model to the repository.
Visually, the model appears in the list of models from the Moose Panel.

Another way is to import the model from a playground by executing (in the case of a FamixJava model):

```st
FamixJavaModel importMSEFromStream: ('./path/to/string' asFileReference readStream).
```

More information about MSE are available [here](./fileFormat.md#MSE).

Once a model is loaded, it can be easily exported as an MSE file.
This can be done via the contextual menu of the model.
By choosing the _``Export to MSE''_ menu item you will be prompted to indicate the desired file name and location, and the result is an MSE file saved on the disk containing the entities in the model.

It is also possible to do it programmatically by executing: 

```st
"will ask where to create the mse file"
model exportToMSE.

"will write the mse in mseFile.mse"
model exportToMSEStream: ('path/to/new/mseFile.mse' asFileReference writeStream).
```

## Importing Pharo code

Moose comes with a built-in importer for Pharo code. The prerequisite for using this importer is that you first need the target source code present in the Pharo image.

Once the code is present, simply press on the ``Load from Pharo'' item from the menu of the Moose Panel, and follow the steps from the opening wizard.

This importer works out of the box for code built for Pharo.
For code written in other Smalltalk dialects, the code must first be made loadable into Pharo.
Moose does not offer ready made solutions for these other languages, but for most known dialects, like VisualWorks, you can often find solutions for exporting the code in a file format loadable in Pharo.
An important note is that the code does not have to be fully functioning.
It merely needs to be loadable in the Pharo image.

## Create mse file for other language

To create mse files for other programming languages please refer to the [parser section](./../README.md#Parser).
