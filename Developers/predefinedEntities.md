# Predefined Entities / Traits in FamixNG <!-- omit in toc -->

To analyse a system in a given programming language, Moose must have a meta-model for that language.
For example for Java, the meta-model defines that Java programs have classes, containing methods, invoking other methods, etc.
The meta-model describes the entities that compose a program in the given language and how they are related.

In another page, we explain how to [define a meta-model](CreateNewMetamodel.md): create new entities, define relationships, properties, etc.
In this page, we present the library of predefined entities that is part of FamixNG and helps declaring new entities by offering typical properties / relationships needed.

In FamixNG, recurrent properties are modelled into traits.
New entities are created as classes composed from these existing traits.
Some common entities (like Packages) are also proposed, pre-composed with common traits.
We list here all currently traits available.

FamixNG is still under developement and the library of available traits is not completely stabilized.
The following should nevertheless help users make sense of the more than one hundred traits available.

## Categories of traits

First, one can divide the set of traits into four categories:
- [Categories of traits](#categories-of-traits)
- [Association Traits](#association-traits)
- [Technical Traits](#technical-traits)
- [Property Traits](#property-traits)
- [Terminal Traits](#terminal-traits)

They are described as follows:

## Association Traits

They model the fact that an entity is used (referred to) in the source code.
Such a reference creates an association between the entity that uses (refers to) and the one that is used (is referred to).
This includes the four associations of the old Famix: Inheritance, Invocation (of a function or a method), Access (to a variable), and Reference (to a type).

Associations should be thought of as n-to-m relationships between entities.
For that reason, they are reified into their own traits.
Associations define two roles (`from` and `to`) to identify both ends of the association.
Using an association involves:
- defining a class representing the association (for example, Access, using the trait FamixTAccess)
- defining two classes at each end of the association (for example, Function using the trait FamixTWithAccess, and Variable using the trait FamixTAccessible).

There are five full-fledged associations in FamixNG:

| Association            |UML|
|:-|:-|
|`FamixTAccess`, from: `FamixTWithAccess`, to: `FamixTAccessible`|<details><summary>Click ![UML](https://img.shields.io/badge/external-UML-green)</summary><p>![PlantUML Image](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.githubusercontent.com/moosetechnology/moose-wiki/master/Developers/Diagrams/access.puml&fmt=svg)</p></details> |

- `FamixTInheritance`, from: `FamixTWithInheritance`, to: `FamixTWithInheritance`
  [![UML](https://img.shields.io/badge/external-UML-green)](Diagrams/inheritance.png)
- `FamixTInvocation`, from: `FamixTWithInvocation`, to: `FamixTInvocable`, for OO programs, there is an extra receiver: `FamixTInvocationReceiver`
  [![UML](https://img.shields.io/badge/external-UML-green)](Diagrams/invocation.png)
- `FamixTReference`, from: `FamixTWithReferences`, to: `FamixTReferenceable`
  [![UML](https://img.shields.io/badge/external-UML-green)](Diagrams/reference.png)
- `FamixTTraitUsage`, from: `FamixTWithTrait`, to: `FamixTTrait`
  [![UML](https://img.shields.io/badge/external-UML-green)](Diagrams/usetrait.png)


To these five we added two more specialized "associations":
`DereferencedInvocation` (call of a pointer to a function in C) and `FileInclude` (also in C).
These do not reify the association as a separate entity, but they might do so in the future.
For now there are only two traits to put at each end of the relationship:
- `FamixTDereferencedInvocation` and `FamixTWithDereferencedInvocations`
  [![UML](https://img.shields.io/badge/external-UML-green)](Diagrams/derefInvok.png)
- `FamixTFileInclude` and `FamixTWithFileInclude`
  [![UML](https://img.shields.io/badge/external-UML-green)](Diagrams/fileInclude.png)


## Technical Traits

They do not model programming language entities but are used to implement Moose functionalities.

Currently, this includes several types of `FamixTSourceAnchors` that allow recovering the source code of the entities.
A typical `FamixTSourceAnchor` contains a filename, and start and end positions in this file.
[![UML](https://img.shields.io/badge/external-UML-green)](Diagrams/anchor.png)

*Technical traits* may also implement software engineering metric computation (`TLCOMMetrics`), or ways to model the programming language used (all `SourceLanguage`), or be  used to implement the generic [MooseQuery engine](https://moosequery.ferlicot.fr/).
[![UML](https://img.shields.io/badge/external-UML-green)](Diagrams/technic.png)

## Property Traits

They model composable properties that source code entities may possess.
Some examples are `FamixTNamedEntity` (entities that have a name), `FamixTTypedEntity` (entities that are statically typed), or a number of entities modeling ownership: `FamixTWithGlobalVariables` (entities that can own `FamixTGlobalVariables`), `FamixTWithFunctions` (entities that can own `FamixTFunctions`), ... 

There are 46 *property traits* currently in FamixNG including 38 traits modeling ownership of various possible kind of entities (`FamixTWith...`).

## Terminal Traits

 They model entities that can be found in the source code such as `Functions`, `Classes`, `Exceptions`, ...
These entities are often defined as a composition of some of the *property traits*.
For example, `FamixTClass` is composed of: `FamixTInvocationsReceiver` (class can be receiver of static messages), `FamixTPackageable`, `FamixTType` (classes can be used to type other entities), `FamixTWithAttributes`, `FamixTWithComments`, `FamixTWithInheritances`, `FamixTWithMethods`.

The name *terminal trait* refers to the fact that they can be used directly to create a programming language concept (a class, a package), whereas *property traits* are typically composed with other traits to make a meaningful programming language concept.

There are 38 such *terminal traits* currently in FamixNG.
