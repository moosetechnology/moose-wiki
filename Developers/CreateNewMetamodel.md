# Create a new Meta-model (FamixNG: since Moose 7) <!-- omit in toc -->

To analyse a system in a given programming language, Moose must have a meta-model for that language.
For example for Java, the meta-model defines that Java programs have classes, containing methods, invoking other methods, etc.
The meta-model describes the entities that compose a program in the given language and how they are related.

In the following, we describe how to create a new meta-model or extend an existing one.
Moose being more specifically dedicated to source code analysis, there are a number of pre-set entities/traits that should help one define new meta-models for a given programming language.
These are described in [another page](predefinedEntities.md) ![Unfinished](https://img.shields.io/badge/Progress-Unfinished-yellow.svg?style=flat).


- [Set up](#set-up)
- [Basic meta-model](#basic-meta-model)
  - [Define entities](#define-entities)
  - [Define hierarchy](#define-hierarchy)
  - [Define relations](#define-relations)
  - [Define properties](#define-properties)
  - [Generate](#generate)
- [Introducing traits](#introducing-traits)
- [Introducing submetamodels](#introducing-submetamodels)
  - [Set up submetamodels](#set-up-submetamodels)
  - [Define remote entities and traits](#define-remote-entities-and-traits)
  - [Define remote hierarchy](#define-remote-hierarchy)
  - [Define remote relations](#define-remote-relations)
  - [Complementary information](#complementary-information)
- [Thanks](#thanks)

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
DemoMetamodelGenerator class >> #packageName

    ^ #'Demo-Model-generated'
```

By default the package name will be used as a prefix for the generated classes.
But we can specify a custom prefix by defining the method `#prefix`.

```st
DemoMetamodelGenerator class >> #prefix

    ^ #'Demo'
```

## Basic meta-model

In this section, we will see how to create a simple meta-model.

To design a meta-model, we need to specify its entities, their relations and their properties.

You may also consult a [presentation of Famix generator](https://www.slideshare.net/JulienDelp/famix-nextgeneration) from Julien Delplanque.

### Define entities

A meta-model is composed of entities.
These entities represent the elements of the model we will manipulate.
To define entities in the generator, we extend the method `#defineClasses`.
Each entity is defined using the generator builder (provided by `FamixMetamodelGenerator`) to which we send the the message `#newClassNamed:`.

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

It is important to comment the entities to help other developers understand our meta-model.
This can be done with the `#newClassNamed:comment:` method.

```st
class := builder newClassNamed: #Class comment: 'I represent a Smalltalk class'.
```

It is also possible to use entities that are already defined in a [library of predefined entities](predefinedEntities.md) or in another meta-model (see [submetamodels](#introducing-submetamodels)).

### Define hierarchy

Once the entities are defined, the next step is to specify their hierarchy.

| binary |             definition             |
| :----: | :--------------------------------: |
| <code>--&#124;></code> | Left entity extends (inherits from) the right one |
| <code><&#124;--</code> | Right entity extends (inherits from) the left one |

Note that these symbols are actually Pharo binary methods.
One can also use a Pharo keyword method: `#generalization:` defining that the receiver extends the parameter (i.e., similar to <code>--&#124;></code>).

The hierarchy is defined in the generator with the method `#defineHierarchy`.

```st
DemoMetamodelGenerator>>#defineHierarchy

    super defineHierarchy.
    package --|> entity.
    class --|> entity.
    method --|> entity.

    variable <|-- localVariable.
    variable <|-- attribute.
```

### Define relations

Then we will define the relations between the entities.
Multiple relations are available in Famix.
In the following we present the relations and the keywords to define them.

|      method       | binary |
| :---------------: | :----: |
|   `#oneToOne:`    |  `-`   |
|   `#oneToMany:`   |  `-*`  |
|   `#manyToOne:`   |  `*-`  |
|  `#manyToMany:`   | `*-*`  |
|  `#containsOne:`  | `<>-`  |
| `#containsMany:`  | `<>-*` |
| `#oneBelongsTo:`  | `-<>`  |
| `#manyBelongsTo:` | `*-<>` |

We can now define the relations between the entities of our meta-model in the method `#defineRelations`.

```st
DemoMetamodelGenerator>>#defineRelations
    super defineRelations.
    package <>-* class.
    class <>-* attribute.
    method <>-* localVariable
```

As for the definition of the entities, it is possible to define a comment for each side of the relation and to use a custom name for the accessors.

```st
DemoMetamodelGenerator>>#defineRelations
    super defineRelations.
    ((package property: #classes) comment: 'The classes inside the package')
        <>-*
    ((class property: #package) comment: 'The package that contains this class').
```

Finally, it is possible to set several other properties.
Some may be applied on one side of the relation:

- container _(the side "contains" the other one, see the different keyword)_
- derived _(the relation is not part of the model but is computed)_
- source _(source of an association)_
- target _(source of an association)_

Others can be applied on the relation itself:

- withoutPrimaryContainer _(Since an element can only have one container, it allows one to create another containment relation. This relation will not set the method `#belongsTo`)_
- withNavigation _(force the relation to appear in the Moose Playground)_

### Define properties

The last step before generating the model is the definition of the properties of the entities.
A property can be of any type.
It is also possible to define a comment for each property.
Let's create the method `#defineProperties` in our generator:

```st
DemoMetamodelGenerator>>#defineProperties

    super defineProperties.

   (entity property: #name type: #String)
       comment: 'The name of the entity'.
```

### Generate

We have described our meta-model.
The last step is to actually generate it with: `DemoMetamodelGenerator generate`.

Later, if the description of the meta-model is modified, the generation will only regenerate the modified elements and remove the old ones.
It is possible to force a full (clean) generation of the model with: `DemoMetamodelGenerator generateWithCleaning`.

## Introducing traits

In addition to the entities, we can use traits to add information to the meta-model.
Traits are a flexible tool to avoid problems with multiple inheritance.
Traits are defined in the same way as entities.
In our previous example, classes are inside a package.
However, we forgot that a package can also contain another package, and we need to model that.

First of all, we need to define the traits to create the containment relation of a package.
We declare the trait in the method `#defineTraits`.

```st
defineTraits

    super defineTraits.

    tPackageable := builder newTraitNamed: #TPackageable comment: 'I can be inside a Package'.
    tWithPackages := builder newTraitNamed: #TWithPackages comment: 'I can contains packageable elements'.
```

Then, we have to change the hierarchy of our entity to add the traits.

```st
DemoMetamodelGenerator>>#defineHierarchy

    super defineHierarchy.

    package --|> entity.
    package --|> tWithPackages.
    package --|> tPackageable.

    class --|> entity.
    class --|> tPackageable.

    method --|> entity.

    variable <|-- localVariable.
    variable <|-- attribute.
```

Finally, we define the relations between the traits.

```st
DemoMetamodelGenerator>>#defineRelations
    super defineRelations.
    package <>-* class.
    package <>-* package.
    tWithPackages <>-* tPackageable.
    class <>-* attribute.
    class <>-* method.
    method <>-* localVariable
```

It is also possible to use traits that are already defined in another meta-model (see [submetamodels](#introducing-submetamodels)).

## Introducing submetamodels

One powerful feature of Famix is the possibility to use submetamodels.
It allows one to extend or compose several meta-models.

There are two way of extending of meta-model:

1. Create a generator that extends the first one.
2. Create a separate generator that declares another as submetamodel.

Although the first one can be used, when the meta-model is generated, the entities that come from the extended generator will be created two times, in the new generator and in the old one.
So, the best way to extend a meta-model is to use the submetamodels.
In the following, we present how to configure a generator for submetamodels.

### Set up submetamodels

In this example, we will create a new generator that will add the interface entity that will be use to represent an interface.
The interface is packageable and contain methods.

> Note that it is done in an example. The best way in this case would be to modify the previous generator.

First of all, we create a new generator:

```st
FamixMetamodelGenerator subclass: #DemoInterfaceMetamodelGenerator
    slots: { }
    classVariables: { }
    package: 'Demo-InterfaceModel-Generator'
```

Then we declare our previous generator as submetamodel:

```st
DemoInterfaceMetamodelGenerator class >> #submetamodels

    ^ { DemoMetamodelGenerator }
```

> We also have to define the `#prefix` and the `#packageName`.

### Define remote entities and traits

Once the generator is configured we can define the entities of the AST meta-model and the entities that come from the Demo meta-model.
To define an entity of another meta-model we used the method `#remoteEntity:withPrefix:`.
The prefix is then the prefix defined for the submetamodel.

```st
DemoInterfaceMetamodelGenerator>>#defineEntities

    super defineEntities.
    interface := builder newClassNamed: #Interface.

    method := self remoteEntity: #Method withPrefix: #Demo.
```

> In some cases, it may be necessary to define remote traits. Use the method `#remoteTrait:withPrefix:` on the generator.

### Define remote hierarchy

To represent the relation of containment of a package on an interface, we use the trait `#TPackageable` (see [Introducing traits](#introducing-traits)).
Because there is only one trait with this name in the submetamodels, we can use the notation with the symbol instead of defining it in a variable in the "defineEntities" section:

```st
DemoInterfaceMetamodelGenerator>>#defineHierarchy

    super defineHierarchy.
    "use the trait of the other metamodel"
    interface --|> #TPackageable.
```

### Define remote relations

Finally, we create the relations between the interface and the methods:

```st
DemoInterfaceMetamodelGenerator>>#defineRelations
    super defineRelations.

    ((interface property: #methods) comment: 'The methods of the interface')
        <>-*
    ((method property: #interface) comment: 'The interface that own me').
```

### Complementary information

In our example, an interface contains methods and a class contains methods, too.
However, it is not possible to have *two* main containers for an entity (see [Define relations](#define-relations)).
In this case, we can either declare the relation "interface <>-* method" without a primary container, or define
two Traits in the first meta-model.
One would be `#TMethod` and the other `#TWithMethods`.

## Thanks

This wiki page is inspired by [the FamixNG booklet](https://github.com/SquareBracketAssociates/Booklet-FamixNG).
