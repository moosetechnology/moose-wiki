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

- Associations
  - Access
  - Include
  - Inheritance
  - Invocation
  - Reference
- Properties
  - Metrics
  - Named
  - Navigation
  - Stub
  - With class scope
  - With modifiers
  - With signature
- Relationships
  - Typed entities
  - Type alias
  - Trait usage
  - Exceptions
  - Templates
  - Source anchor
  - Scopes
  - Parameterizable types
  - Parameters
  - Packages
  - Namespace
  - Module
  - Method
  - Attribute
  - Annotations
  - Function
  - Enumerate
  - Compilation unit
  - Comment
  - Variables (global, local)
  - Header file
  - File system
- Common entities
  - Sourced entities
  - Named entities
  - Class
  - Method

## Predefined Associations

Associations should be thought of as n-m relationhips between entities.
For that reason, they are reified into their own traits and using an association involves:
- defining a class representing the association (for example Inheritance)
- defining two classes at each end of the association (for example subclass, superclass)

Associations should define two roles (`from` and `to`) to identify both ends of the association.

The predefined associations are:
  - Association
  - Access
  - Include
  - Inheritance
  - Invocation
  - Reference

## Predefined Properties

Properties are primitive types attributes in the entities (Number, String, Boolean):
- Metrics
- Named
  (Note: may be should be removed and linked to Namespace)
- Navigation
- Stub
- With class scope
- With modifiers
- With signature


## Predefined Relationships


## Predefined Entities

