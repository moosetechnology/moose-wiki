# File format

Moose is compatible with two file formats: JSON and MSE.
In the following, we describe how each file must be written.

## JSON

[JSON](https://json.org/) is a famous file with parsers and generators supported by many languages.
Here, we will not detail the JSON format but how it **must** be used to be loaded as a Moose model.

```json
[
    {
        "FM3": "EntityTypeName",
        "id": 1,
        "propertyOne": "string property value"
    },
    {
        "FM3": "AnotherAnotherEntityType",
        "id": 2,
        "entityProperty": { 
            "FM3": "EntityTypeName",
            "id": 3,
            "propertyInteger": 42
        }
    },
    {
        "FM3": "AnotherAnotherEntityType",
        "id": 4,
        "propertyThatReferesAnotherEntity": { "ref": 1 }
    },
    
]
```

## MSE

MSE is the default file format supported by Moose.
It is a generic file format and can describe any model.
It is similar to XML, the main difference being that instead of using verbose tags, it makes use of parentheses to denote the beginning and ending of an element.

The following snippet provides an example of a small model:

```mse
((FamixJava.Namespace (id: 1)
    (name 'aNamespace'))
  (FamixJava.Package (id: 201)
    (name 'aPackage'))
  (FamixJava.Package (id: 202)
    (name 'anotherPackage')
    (parentPackage (ref: 201)))
  (FamixJava.Class (id: 2)
    (name 'ClassA')
    (container (ref: 1))
    (parentPackage (ref: 201)))
  (FamixJava.Method
    (name 'methodA1')
    (signature 'methodA1()')
    (parentType (ref: 2))
    (LOC 2))
  (FamixJava.Attribute 
    (name 'attributeA1')
    (parentType (ref: 2)))
  (FamixJava.Class (id: 3)
    (name 'ClassB')
    (container (ref: 1))
    (parentPackage (ref: 202)))
  (FamixJava.Inheritance
    (subclass (ref: 3))
    (superclass (ref: 2))))
```

The file defines 8 entities: 1 Namespace, 2 Packages, 2 Classes, 1 Method, 1 Attribute and 1 Inheritance.
For each of these entities it provides a unique identifier (_e.g._, (id: 1)) and it defines properties.
In general, properties can be either primitive, like (name 'aNamespace'), or they can point to another entity, like in the case of (container (ref: 1)) which denotes that the container property of ClassA points to the instance of Namespace named aNamespace.

The overall object graph can be seen graphically below.

![MSE example](./mse-graph.png)
