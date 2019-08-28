# Importing and exporting models

The first step in the process of analysis is the generation of a model of a given target system or set of data. Moose can handle multiple types of data and data sources. This chapter provides a short guide for how to deal with these.

## Importing and exporting with MSE

The preferred way to load a model in Moose is via an MSE file. To load an MSE file, all you have to do is to press the ``Import from MSE'' button in the Moose Panel and indicate the file to load. This creates a model, populates it with the entities from the file and adds the model to the repository. Visually, the model appears in the list of models from the Moose Panel.

But what exactly is MSE? MSE is the default file format supported by Moose. It is a generic file format and can describe any model. It is similar to XML, the main difference being that instead of using verbose tags, it makes use of parentheses to denote the beginning and ending of an element.

The following snippet provides an example of a small model:

```
( (FAMIX.Namespace (id: 1)
    (name 'aNamespace'))
  (FAMIX.Package (id: 201)
    (name 'aPackage'))
  (FAMIX.Package (id: 202)
    (name 'anotherPackage')
    (parentPackage (ref: 201)))
  (FAMIX.Class (id: 2)
    (name 'ClassA')
    (container (ref: 1))
    (parentPackage (ref: 201)))
  (FAMIX.Method
    (name 'methodA1')
    (signature 'methodA1()')
    (parentType (ref: 2))
    (LOC 2))
  (FAMIX.Attribute 
    (name 'attributeA1')
    (parentType (ref: 2)))
  (FAMIX.Class (id: 3)
    (name 'ClassB')
    (container (ref: 1))
    (parentPackage (ref: 202)))
  (FAMIX.Inheritance
    (subclass (ref: 3))
    (superclass (ref: 2))))
```

The file defines 8 entities: 1 Namespace, 2 Packages, 2 Classes, 1 Method, 1 Attribute and 1 Inheritance. For each of these entities it provides a unique identifier (e.g., (id: 1)) and it defines properties. In general, properties can be either primitive, like (name 'aNamespace'), or they can point to another entity, like in the case of (container (ref: 1)) which denotes that the container property of ClassA points to the instance of Namespace named aNamespace.

The overall object graph can be seen graphically below.

![MSE example](./img-ImportingAndExportingModels/mse-graph.png)

A more complex MSE example is available for download as described in .

Once a model is loaded, it can be easily exported as an MSE file. This can be done via the contextual menu of the model. By choosing the ``Export to MSE'' menu item you will be prompted to indicate the desired file name and location, and the result is an MSE file saved on the disk containing the entities in the model.

## Importing Pharo code

Moose comes with a built-in importer for Pharo code. The prerequisite for using this importer is that you first need the target source code present in the Pharo image.

Once the code is present, simply press on the ``Load from Pharo'' item from the menu of the Moose Panel, and follow the steps from the opening wizard.

This importer works out of the box for code built for Pharo. For code written in other Smalltalk dialects, the code must first be made loadable into Pharo. Moose does not offer ready made solutions for these other languages, but for most known dialects, like VisualWorks, you can often find solutions for exporting the code in a file format loadable in Pharo. An important note is that the code does not have to be fully functioning. It merely needs to be loadable in the Pharo image.

## Importing Java code

Moose supports the analysis of Java systems by means of external parsers and MSE file for model interchange. The general process can be seen in the picture below.

The first step is to use the external parser to process the Java sources. Afterwards, the external tool exports the model in MSE. And finally, this MSE file is imported in Moose.

