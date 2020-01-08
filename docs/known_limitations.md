# Known Limitations

## Purpose

This set of documentation describes the data format GEMD.  This data concept is independent of any implementation of the format.  *However*, as the Citrine platform is assumed to be the primary point of interaction between this data model and most users, known limitations of this model are documented here, with appropriate links to this page provided elsewhere in this documentation.

## Limitations

* The Citrine Platform cannot check for cycles in the material history.  

    Material histories represent a chronology, so cycles in them represent time-travel (physically disallowed).  We do not have the ability to check that writing an object doesn't create a cycle in some material history.  

* The Citrine Platform does not yet support Molecular Values (InChI, SMILES) or Molecular Bounds.  

* We have no support for Composition Values (Nominal Composition, Empirical Formula), although Composition Bounds are supported.   

* The values of name and labels fields of Ingredient Runs should be inherited from the associated Ingredient Specs, and thus be identically equal. However, due to implementation details, care must be taken around creating these objects:
    *  If a user invokes the IngredientRun constructor in citrine-python with either the name or labels argument, a deprecation warning will be issued

    * If a user invokes the spec setter on an IngredientRun and the value is an IngredientSpec (as opposed to a LinkByUID), the `name` and `labels` fields will be set to `spec.name` and `spec.labels`, respectively

        * This is also true with the spec argument to the IngredientRun constructor - the spec setter is called by the constructor

    * If a user attempts to register an IngredientRun whose name or labels do not match those of its spec, a validation error (HTTP 400) will be returned

        * This includes if the values are None, so users should explicitly set their spec to be the appropriate IngredientSpec and not just a LinkByUID

    * Any attempt to update the name or labels of an IngredientSpec that is referenced by 1 or more IngredientRuns will return a validation error (HTTP 400)
