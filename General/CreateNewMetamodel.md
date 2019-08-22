# Create a new Meta-model (Since Moose 7)

:no_entry: Work In Progress

In the following, we present how to create your own meta-model or to extend an already existing meta-model.

## Set up

First of all, we need to [download Moose](../Beginners/InstallMoose.md) version 7 or higher.

The first step is to create a FamixMetamodelGenerator.
It will describe our meta-model.

```st
FamixMetamodelGenerator subclass: #DemoMetamodelGenerator
    slots: { }
    classVariables: { }
    package: 'Demo-Model-Generator'
```

Then we need to configure the generator.
We have to specify the package in which the meta-model will be generated.

```st
DemoMetamodelGenerator class >> packageName

    ^ #'Demo-Model-generated'
```

The package name will be used as prefix for the generated classes.
We can customize it with the method `#prefix`.

```st
DemoMetamodelGenerator class >> packageName

    ^ #'Demo'
```

## Basic Meta-model

In this section, we will see how to create a simple meta-model.

To design a meta-model, we need to specify its entities, theirs relations and theirs properties.

### Define entities

A meta-model is composed of entities.
Those entities represent the elements of the model we will manipulate.
To define a new entity in the generator, we extend the method `#defineClasses`.
Then, we use the generator builder (provided by `FamixMetamodelGenerator`) with the method `#newClassNamed:`.

```st
DemoMetamodelGenerator>>#defineClasses
    super defineClasses.
    entity := builder newClassNamed: #Entity.
    package := builder newClassNamed: #Package.
    class := builder newClassNamed: #Class.
    method := builder newClassNamed: #Method.
    variable := builder newClassNamed: #Variable.
    localVariable := builder newClassNamed: #LocalVariable.
    attribute := builder newClassNamed: #Attribute.
```

It is important to comment the entities to help the other developers understand our meta-model.
So, we use the method `#newClassNamed:comment:`

```st
class := builder newClassNamed: #Class comment: 'I represent a Smalltalk class'.
```

### Define Relations

- Explication relations
  - with comments
  - with container
- Define Relations

### Define properties

- Define Properties
  - with comments
  - supported properties (String, Number)

## Introducing Traits

- Define Traits
- Good Practice

## Introducing Submetamodels

- Different kind of submetamodels
  - by extension
  - by defining submetamodels

- Define submetamodels
  - Example
- Submetamodels and traits

## Thanks

This wiki page is inspired by [the FamixNG booklet](https://github.com/SquareBracketAssociates/Booklet-FamixNG).
