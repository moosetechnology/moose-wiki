# Predefined Entities / Traits in FamixNG <!-- omit in toc -->

To analyse a system in a given programming language, Moose must have a meta-model for that language.
For exemple for Java, the meta-model defines that Java programs have classes, containing methods, invoking other methods, etc.
The meta-model describes the entities that compose a program in the given language and how they are related.

In [another page](CreateNewMetamodel.md), we explain how to create new entities, define relationships, properties, etc.
In this page, we present the library of predefined entities that is part of FamixNG and helps declaring new entities by offering typical properties / relationships needed.

In FamixNG, recurrent properties are modelled into traits.
New entities are created as classes composed from these existing traits.
Some common entities (like classes) pre-composed with common traits are also defined.
We list here all traits available.

FamixNG is still under developement and the library of available traits is not completly stabilised.
The following should nevertheless help user to make sense of the more than one hundred traits available.

## Categories of traits

First, one can divide the set of traits in four categories:
- Associations
- Technical
- Property
- Terminal

They are described in the following

## Association Traits

They model the fact that an entity is used (referred to) in the source code.
Such reference creates an association between the entity that uses (refers) and the one that is used (referred to).
This includes the four associations of the old Famix: Inheritance, Invocation (of a function or a method), Access (to a variable), and Reference (to a type).

Associations should be thought of as n-m relationhips between entities.
For that reason, they are reified into their own traits and using an association involves:
- defining a class representing the association (for example Inheritance)
- defining two classes at each end of the association (for example subclass, superclass)
- they define two roles (`from` and `to`) to identify both ends of the association:

There are five full fledged associations in FamixNG:
- `FAMIXTAccess`, from: `FAMIXTWithAccess`, to: `FAMIXTAccessible`
- `FAMIXTInheritance`, from: `FAMIXTWithInheritance`, to: `FAMIXTWithInheritance`
- `FAMIXTInvocation`, from: `FAMIXTWithInvocation`, to: `FAMIXTInvocable`, for OO programs, there is an extra receiver: `FAMIXTInvocationReceiver`
- `FAMIXTReference`, from: `FAMIXTWithReferences`, to: `FAMIXTReferenceable`
- `FAMIXTTraitUsage`, from: `FAMIXTWithTrait`, to: `FAMIXTTrait`


To this we may add two more specialized "associations": `DereferencedInvocation` (call of a pointer to a function in C) and `FileInclude` (also in C).
These do not reify the association as a separate entity, but they might do in the future.
For now there are only two traits to put at each end of the relationship:
- `FAMIXTDereferencedInvocation` and `FAMIXTWithDereferencedInvocations`
- `FAMIXTFileInclude` and `FAMIXTWithFileInclude`


## Technical Traits

They do not model programming language entities but are used to implement Moose functionalities.
Currently, this includes several types of `TSourceAnchors`, associated to `TSourcedEntity` to allow recovering their source code (a typical `TSourceAnchor` contains a filename, and start and end positions in this file).
Other \emph{Technical traits} implement software engineering metric computation, or are used to implement the generic [MooseQuery engine](https://moosequery.ferlicot.fr/).

## Property Traits

They model composable properties that source code entities may possess.
This includes `TNamedEntity` (entities that have a name) and `TTypedEntity` (entities that are statically typed), or a number of entities modeling ownership: `TWithGlobalVariables` (entities that can own `TGlobalVariables`), `TWithFunctions` (entities that can own `TFunctions`),... 

There are 46 property traits currently in FamixNG including 38 traits modeling ownership of various possible kind of entities.

## Terminal Traits

 They model entities that can be found in the source code such as `Functions`, `Classes`, `Exceptions`, \ldots
These entities are often defined as a composition of some of the \emph{Core traits}.
For example, `TClass` is composed of: `TInvocationsReceiver` (class can be receiver of static messages), `TPackageable`, `TType` (classes can be used to type other entities), `TWithAttributes`, `TWithComments`, `TWithInheritances`, `TWithMethods`.
The name terminal trait refers to the fact that they can be used directly to create a programming language concept (a class, a package), whereas property traits are typically composed with other traits to make a meaningful programming language concept.
There are 38 such terminal traits currently in FamixNG.
