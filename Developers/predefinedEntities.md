# Predefined Entities / Traits in FamixNG <!-- omit in toc -->

To analyse a system in a given programming language, Moose must have a meta-model for that language.
For exemple for Java, the meta-model defines that Java programs have classes, containing methods, invoking other methods, etc.
The meta-model describes the entities that compose a program in the given language and how they are related.

In [another page](CreateNewMetamodel.md), we explain how to create new entities, define relationships, properties, etc.
In this page, we present the library of predefined entities that is part of FamixNG and helps declaring new entities by offering typical properties / relationships needed.

In FamixNG, recurrent properties are modelled into traits.
New entities are created as classes composed from these existing traits.
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
  - Function
  - Enumerate
  - Compilation unit
  - Comment
  - Variables (global, local)
  - Header file
  - File system
