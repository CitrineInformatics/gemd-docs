# Known Limitations

## Purpose

This set of documentation describes the next generation Citrine data format, GEMD, that is independent of any implementation of the format.  

*However*, as the Citrine Platform is assumed to be the primary point of interaction between this data model and most users, known limitations of this model are documented here, with appropriate links to this page provided elsewhere in this documentation.

## Limitations

* The Citrine Platform cannot check for cycles in the material history.  

    Material histories represent a chronology, so cycles in them represent time-travel (physically disallowed).  We do not have the ability to check that writing an object doesn't create a cycle in some material history.  

* The values of `name` and `labels` of Ingredient Runs are inherited from the associated Ingredient Specs, and thus are identically equal. However, this was not true in early designs.  This means some older implementations may still have those fields in Ingredient Runs and there are therefore order-of-operations concerns that might get in the way of object validation.  

    So long as a user uses the `spec` setter (or invokes the constructor with the `spec` argument) with an [Ingredient Spec](../specification/objects/#ingredient-spec) object (as opposed to a [LinkByUID](../specification/unique-identifiers/#linkbyuid) object), there should be no issue with validation on the Citrine Platform.
