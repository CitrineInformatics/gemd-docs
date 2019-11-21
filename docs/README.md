# Taurus Data Model Documentation

This is the next major data model developed by Citrine, codename `taurus`.

The model links together materials, the processes that produced them,
and the measurements that characterize them. This facilitates the backwards traversal from a measurement to the material on which it was performed to the process by which it was produced to the materials which were used in that process. This generalizes and matures `preparation` and `subSystems` objects within the Physical Information File (PIF).

Additionally, the model makes a first-class distinction between *intent* and *realization*, captured by `Spec` and `Run` objects, respectively. A single intent `Spec` can be realized into multiple `Run` objects. This generalizes and matures the `ideal` concept from the PIF's `Composition` and `Quantity` objects.

The model contains a new type of object: the [`Measurement Run`](./specification/objects/#measurement-run). Measurements capture discrete measurement activity, including the parameters and conditions associated with a set of measured properties. This many-to-many relationship between properties, conditions, and parameters resolves a fundamental ambiguity present in the PIF; which properties were measured at the same time under the same conditions?

## Specification of the NextGen Data format 

The format is described in the following subsections:

* [Value Types](./specification/value-types)
* [Attributes](./specification/attributes)
* [Attribute Templates](./specification/attribute-templates)
* [Object Templates](./specification/object-templates)
* [Objects](./specification/objects)
* [Unique Identifiers](./specification/unique-identifiers)
* [Tags](./specification/tags)
* [File Links](./specification/file-links)

## Getting help

Check out our [FAQ](./faq.md)!

