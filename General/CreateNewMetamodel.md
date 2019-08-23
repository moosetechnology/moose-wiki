# Create a new Meta-model (Since Moose 7) <!-- omit in toc -->

:no_entry: Work In Progress

In the following, we present how to create your own meta-model or to extend an already existing meta-model.

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

The package name will be used as prefix for the generated classes.
We can customize it with the method `#prefix`.

```st
DemoMetamodelGenerator class >> #packageName

    ^ #'Demo'
```

## Basic meta-model

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

It is also possible to use entities that are already defined in another meta-model (see [submetamodels](#introducing-submetamodels)).

### Define hierarchy

Once the entities are defined, the next step is to specify their hierarchy.
One method and two binary methods can be used in this step.

|       method       | binary |             definition             |
| :----------------: | :----: | :--------------------------------: |
| `#generalization:` | `--|>` | The receiver extends the parameter |
|                    | `<|--` | The parameter extends the receiver |

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
Multiple relations are available in the famix.
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
Some be applied on one side of the relation:

- container _(the side "contains" the other one, see the different keyword)_
- derived _(the relation is not part of the model but is computed)_
- source _(source of an association)_
- target _(source of an association)_

Others can be applied on the relation:

- withoutPrimaryContainer _(Since an element can only have one container, it allows one to create another containment relation. This relation will not set the method `#belongsTo`)_
- withNavigation _(force the relation to appear in the Moose Playground)_

### Define properties

The last step before generating the model is the definition of the properties of the entities.
A property can be of any type.
It is also possible to define a comment for each property.
Let's create the method `#defineProperties` in our generator

```st
DemoMetamodelGenerator>>#defineProperties

    super defineProperties.

   (entity property: #name type: #String)
       comment: 'The name of the entity'.
```

### Generate

We have create our meta-model generator.
The last step is to execute the generation with: `DemoMetamodelGenerator generate`.

If, in the futur, the generator is modified, the generation regenerates only the modified elements and remove the old one. It is possible to execute a full clean of the model before its generation with: `DemoMetamodelGenerator generateWithCleaning`.

## Introducing traits

In addition of the entity, we can use trait to add information in the meta-model.
The traits are defined in the same way as the entities.
In our previous example, the classes are inside a package.
However, we forgot that a package can also contain another package.

First of all we need to define the traits to create the containment relation of a package.
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

    "package <>-* class.
    package <>-* package."
    tWithPackages <>-* tPackageable.

    class <>-* attribute.

    method <>-* localVariable
```

It is also possible to use traits that are already defined in another meta-model (see [submetamodels](#introducing-submetamodels)).

## Introducing submetamodels

One powerful feature of Famix is the possibility to use submetamodels.
It allows one to extend or compose several meta-models.

There are two way of extending of meta-model:

1. Create a generator that extends the first one
2. Create a separate generator that declare another as submetamodel.

Despite the first one can be used, when the meta-model is generated, the entities that comes from the extended generator will be created two times, in the new generator and in the old one.
So, the best way to extends a meta-model is to use the submetamodels.
In the following, we present how to configure a generator for submetamodels.

### Set up submetamodels

In this example, we will create a new generator that will add AST information to the previous entity `method` to represent its body.

First of all, we create a new generator.

```st
FamixMetamodelGenerator subclass: #DemoASTMetamodelGenerator
    slots: { }
    classVariables: { }
    package: 'Demo-ASTModel-Generator'
```

Then we declare our previous generator as submetamodel

```st
DemoASTMetamodelGenerator class >> #submetamodels
    
    ^ { DemoMetamodelGenerator }
```

In our example, we will not only extend the first meta-model, we will create relations between the first one and the new one.
In this case, we have to implement the method `modifyMetamodel: aMetamodel` to correctly create [the extension methods](https://github.com/pharo-open-documentation/pharo-wiki/blob/master/General/Extensions.md).

```st
DemoASTMetamodelGenerator class >> #modifyMetamodel: aMetamodel

    super modifyMetamodel: aMetamodel.

    self fixRemoteMetamodelRelationsIn: aMetamodel.
```

### Define remote entities and traits

- remote Entity
- remote Trait

### Define remote hierarchy

- example simple
- example with Trait, and conflicting trait

## Thanks

This wiki page is inspired by [the FamixNG booklet](https://github.com/SquareBracketAssociates/Booklet-FamixNG).
