# Taurus Philosophy

## Scope
Taurus records physical and contextual information about materials and chemicals.
This includes:

 - Physical properties of materials and chemicals
 - Descriptions of processes and characterizations performed on the material, including who, when, and why
 - Provenance of the materials and chemicals
 - A controlled vocabulary for domain concepts like "vapor pressure" and "universal testing machine"

## Objective
Taurus enables information reuse and supports a material and chemicals system of record.

## Guiding principles

#### Taurus does not define a controlled vocabulary

It is not the place of the data format to say "Bandgap" instead of "band gap" or "Young's modulus" instead of "Young modulus".
[Objects](../specification/objects) and
[Attributes](../specification/attributes) have `name` fields that can take any value.
Even the notion of equivalence/comparability is abstracted away to templates (see below),
which can be used to define controlled vocabularies in data.

#### Validations define a controlled vocabulary

Taurus captures validation in the form of
[Attribute Templates](../specification/attribute-templates) and
[Object Templates](../specification/object-templates).
These same objects serve to define a controlled vocabulary of domain concepts.
The way that taurus indicates that a property is a "vapor pressure" vs an "ambient pressure" vs
a "pressure applied on the horizontal face" is by defining templates for each of those concepts and assigning
those templates to their corresponding properties.

This controlled vocabulary of templates supports robust comparisons.
If two attributes define the same attribute template, then they should be directly comparable.
On the other hand, if two attributes only define the same dimension (e.g. mass-length-time) then they may correspond
to distinct and incomparable physical concepts.

#### Everything is bounded

The bounds on real, integer, and categorical values do not support infinity or wildcard categories.
If an [Attribute](../specification/attributes) is assigned an [Attribute Template](../specification/attribute-templates), then
its values must be bounded.
The utility is that those bounds define either a finite set of options the attribute might take, for categorical and integer types, or
a finite range for real types.
This defines a scale for evaluating how similar two attributes are set to select values from when inputting or generating data.

How do we get away with this requirement?
The universe only has so much energy, so there is a practical upper limit to most physical quantities.
More practically, the physical systems present in materials and chemicals can range considerably in size, but not infinitely so.
When defining the bounds in templates, it is important to have a broad ranges of use cases in mind.
[Attribute Templates](../specification/attribute-templates) should contain especially broad ranges, while
[Object Templates](../specification/object-templates) can refine those ranges to a tighter scope of application.

#### Never turn away data

If the data is in scope, then the data model should have a place to write it.
Catch-all fields, e.g. `notes`, are useful, but common notes patterns should be elevated into the spec.
When linking data, the data author should be blocked by other data authors as infrequently as possible.

Another consequence of never wanting to turn data away is that curation via template assignment is optional.
It is better to have data with only unstructured context, e.g. notes, than to reject data because the user cannot or does not assign a template.
This is particularly true for implementations that control the generation of templates, limiting the ability of any user to define an appropriate template.

#### Process is a first-class citizen

The same stuff processed in different ways results in different materials, so the process by which a material was created belongs on equal footing with the material itself.
In taurus, they are in a 1:1 correspondence with each other: every material object has a process object that produced it.
It is required because a material cannot be described without describing its process.

#### Multiple authors can contribute to the history of a material

Over its lifetime, a material and its precursors frequently change hands.
Consequently, the people who know about each processing and measurement step can be different.
In extreme cases, a third party measurement might be deliberately blinded to the description of the sample being tested.

The data model supports this by encoding information about a material in a set of objects, each scoped to contain the information that an individual or team would have.
Each individual or team owns their steps in the process, but doesn't need to worry about the downstream steps.
The objects are linked in a chronological order from oldest to most recent.

#### We don't always get what we want

But if we try sometimes, we record what we'll need later.

Taurus distinguishes between the intent to do something (a Spec) and the reality of what happened (a Run).
Further, a single intent can be realized multiple times.
This lets taurus capture both systematic and random errors.
It also allows for the definition of intent to precede generation of physical artifacts, supporting information hand-offs to experimental groups.


#### Uncertainty quantification is opt-out rather than opt-in

No measurement can be absolutely certain, and no calculation derived from uncertain inputs can magically produce certain outputs.
Capturing that uncertainty is incredibly valuable as it informs a wide variety of statistical analyses.  
Uncertainty can take many forms, which are represented by a variety of distribution [Value Types](../specification/value-types),
which must be used when recording physical information into [Attributes](../specification/attributes).

However, uncertainty quantification can be challenging and is often impossible in hindsight.
Therefore, `nominal` values are permitted to express "It was nominally X, but we don't know how certain we are about it."


#### Material history is chronological

For any given physical object, one can trace back the series of processes that generated it and the ingredient materials that went into those processes. 
We can represent this chronological history as a directed acyclic graph where each material links to the process that created it and each process links to the materials that went into it.
Each of those input materials must exist *before* the process occurs.
As a consequence there can be no cycles in a well-formed material history, as that would imply a material being created and then traveling back in time to be used in one of the processes that were involved in creating it.
Starting from a root material and following references in the material->process->ingredient->material order, one can traverse the entire history and never see the same object twice.
