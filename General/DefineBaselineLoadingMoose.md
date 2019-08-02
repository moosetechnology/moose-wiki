# Define a baseline loading Moose only if not present

Moose is a big project and loading it all the time is long. 

Multiple peoples came to me asking if there was a way in the baseline of a project to say "Load Moose only if needed".

Since it is a common question here is a guide on how to do that!

One way to do it is to use custom project attributes from Metacello. The principle is simple, depending on the state of your image, you can define project attributes and use them in your baseline.

To define the attributes you need to implement a method #customProjectAttributes that will return an array with the custom attributes. 

For example, if I just want to ensure any version of Famix is present I can implement it like this:

```Smalltalk
customProjectAttributes
	self class environment at: #MooseEntity ifAbsent: [ ^ #(#WithoutFamix) ].
	
	^ #()
```

If I want to ensure Moose is in the image I can define it like this:

```Smalltalk
customProjectAttributes
	RPackageOrganizer default packages detect: [ :package | package name = 'BaselineOfMoose' ] ifNone: [ ^#(#WithoutMoose) ].
	
	^ #()
```

Or I can also define both:

```Smalltalk
customProjectAttributes
	| attributes |
	attributes := OrderedCollection new.
	self class environment at: #MooseEntity ifAbsent: [ attributes add: #WithoutFamix ].
	
	RPackageOrganizer default packages detect: [ :package | package name = 'BaselineOfMoose' ] ifNone: [ attributes add: #WithoutMoose ].
	
	^ attributes asArray
```

And now I can use those attributes in a baseline via the #for:do: message.

## Full example

```Smalltalk
customProjectAttributes
	self class environment at: #MooseEntity ifAbsent: [ ^ #(#WithoutFamix) ].
	
	^ #()
```

```Smalltalk
baseline: spec
	<baseline>
	spec
		for: #common
		do: [
			spec package: 'MyProject'.

			spec
				for: #(#'WithoutFamix') do: [
					spec baseline: 'Moose' with: [ spec repository: 'github://moosetechnology/Moose:development/src' ]
					spec package: 'MyProject' with: [ spec requires: #('Moose') ] ]
```
