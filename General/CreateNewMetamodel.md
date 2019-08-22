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

- Define Entities
  - with comments

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
