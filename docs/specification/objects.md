# Objects

There are two kinds of objects: Specs and Runs.
Specs represent the intent and expectation of the material, process, ingredient, or measurement,
while Runs capture what actually happened.
This captures natural variations and forms an association between samples and design as multiple Runs of the same Spec.

Specs are specific.
In `MaterialSpec`, `ProcessSpec`, `IngredientSpec`, and `MeasurementSpec` objects, value should be given nominal values, e.g.:
real-valued attributes on Specs should have [Nominal Values](../value-types/#nominal-real-values).
This is in contrast to another common usage of the term "Specification" (or tolerance) as a range of accepted values, e.g. "The material is in spec if the nitrogen impurity concentration is below 0.1%."
In this data model, that notion of a "spec" that an object can "be in" is an [Object Template](../object-templates).

Specs can have an [Object Template](../object-templates/) associated, which bounds the valid units and values of the [Attributes](../attributes) on the Spec.
Object Runs associated with an object Spec inherit the [Object Template](../object-templates/) associated with the Spec, and their [Attributes](../attributes) are thus also constrained by the [Object Template](../object-templates/).


* Many Specs can reference the same Object Template.
* Each Spec can be associated with at most one Object Template.
* Many Runs can reference the same Spec.
* Each Run must be associated with exactly one Spec.

---
## Process Spec

An expectation of a process.
Processes transform zero or more input materials into exactly one output material.


Field name | Value type | Default | Description
-----------|------------|---------|-------------
`uids`        | Map[String, String] | Empty | A collection of [Unique Identifiers](../unique-identifiers)
`type`        | String     | Req. | "process_spec"
`name`| String     | Req. | The name of the spec
`notes`       | String     | None | Some free-form notes about the spec.
`tags`        | Set[String]| Empty | [Tags](../tags)
`file_links`  | Set\[[File Links](../file-links)] | Empty | Links to associated files, with resource paths into the files API
`template`    | [Process Template](../object-templates/#process-template) | None | A template bounding the valid values for parameters and conditions on this process.
`parameters`  | Set\[[Parameters](../attributes/#parameters)] | Empty | Specified parameters for the process spec
`conditions`  | Set\[[Conditions](../attributes/#conditions)] | Empty | Specified conditions for the process spec
`ingredients` | Set\[[Ingredient Spec](./#ingredient-spec)] | Empty | Ingredient Specs
`output_material` | [Material Spec](./#material-spec) | Req. | Output Material Spec


##### Constraints

All [Attributes](../attributes) sharing an [Attribute Template](../attribute-templates) with an Attribute on the associated [Object Template](../object-templates/) will be constrained by the (potentially tighter) bounds set in the `template` Process Template.

Field name | Relationship | Field Name
-----------|:------------:|------------
len(`name`) | <=    | 128, UTF-8 Encoded
len(`description`)  | <=    | 32,768 (32KB), UTF-8 Encoded
parameter names | must be unique | among parameter names
condition names | must be unique | among condition names

##### Example

```json
{
    "type" : "process_spec",
    "uids" : {
        "id" : "064148e6-1cce-4d89-bfde-7ecd0aa4632b"
    },
    "tags" : [
        "baking::cookies"
    ],
    "name" : "Bake Cookies",
    "notes" : "Process Spec for baking cookies in an oven",
    "template" : {
        "type" : "link_by_uid",
        "scope" : "cookie_templates",
        "id" : "baking_process_01"
    },
    "file_links" : [
        {
            "filename" : "nestle-tollhouse-recipe.pdf",
            "url" : "https://example.com/file/d8f12919-b201-4186-be95-10525eb4256a/version/2"
        }
    ],
    "parameters" : [
        {
            "type" : "parameter",
            "name" : "Oven Temperature",
            "origin" : "specified",
            "template" : {
                "type" : "link_by_uid",
                "scope" : "cookie_templates",
                "id" : "oven_temp"
            },
           "tags" : ["oven_settings::duration"],
            "value" : {
                "type" : "nominal_real",
                "nominal" : 450,
                "units" : "kelvin"
            }
        },
        {
            "type" : "parameter",
            "name" : "Baking Time",
            "origin" : "specified",
            "template" : {
                "type" : "link_by_uid",
                "scope" : "cookie_templates",
                "id" : "oven_time"
            },
           "tags" : ["oven_settings::duration"],
           "value" : {
                "type" : "nominal_real",
                "units" : "seconds",
                "nominal" : 600
            }
        }
    ],
    "ingredients" : [
        {
            "type" : "link_by_uid",
            "scope" : "cookie ingredients",
            "id" : "chocolate chip cookie batter"
        },
        {
            "type" : "link_by_uid",
            "scope" : "secret ingredients",
            "id" : "love"
        }
    ],
    "output_material" : {
        "type" : "link_by_uid",
        "scope" : "id",
        "id" : "18d95397-4887-48f0-bdda-94a9a4c5ef45"
    }
}
```

---
## Process Run

A particular instance of a process.

Field name | Value type | Default | Description
-----------|------------|---------|------------
`uids`        | Map[String, String] | Empty | A collection of [Unique Identifiers](../unique-identifiers)
`type`        | String     | Req. | "process_run"
`name`| String     | Req. | The name of the process run
`notes`       | String     | None | Some free-form notes about the process run
`tags`        | Set[String]| Empty | [Tags](../tags)
`file_links`  | Set\[[File Links](../file-links)] | Empty | Links to associated files, with resource paths into the files API
`source`      | [Source](./#source) | None | provenance information for the process
`spec`| [Process Spec](./#process-spec) | Req. | Spec for this process
`parameters`  | Set\[[Parameters](../attributes/#parameters)] | Empty | Measured parameters for the process run
`conditions`  | Set\[[Conditions](../attributes/#conditions)] | Empty | Measured conditions for the process run
`ingredients` | Set\[[Ingredient Run](./#ingredient-run)] | Empty | Ingredient Runs
`output_material` | [Material Run](./#material-run) | Req. | Output Material Run

##### Constraints

Same as `ProcessSpec`, but with the `template` inherited from the `spec`, i.e. `spec.template`.

##### Example

```json
{
    "type" : "process_run",
    "uids" : {
        "cookie_ids" : "choc_chip_proc_001_run_006",
        "id" : "ea7af3f4-8dbf-41ba-8084-c6f2e31907a5"
    },
    "tags" : [
        "baking::cookies"
    ],
    "name" : "Bake Cookies Fo' Real",
    "notes" : "Process run baking some chocolate chip cookies in an oven",
    "process_spec" : {
        "type" : "link_by_uid",
        "scope" : "id",
        "id" : "064148e6-1cce-4d89-bfde-7ecd0aa4632b"
    },
    "parameters" : [
        {
            "type" : "parameter",
            "name" : "Oven Temperature",
            "origin" : "measured",
            "template" : {
                "type" : "link_by_uid",
                "scope" : "cookie_templates",
                "id" : "oven_temp"
            },
           "tags" : ["oven_settings::temperature"],
            "value" : {
                "type" : "uniform_real",
                "lower_bound" : 447.5,
                "upper_bound" : 452.5,
                "units" : "kelvin"
            }
        },
        {
            "type" : "parameter",
            "name" : "Baking Time",
            "origin" : "measured",
            "template" : {
                "type" : "link_by_uid",
                "scope" : "cookie_templates",
                "id" : "oven_time"
            },
           "tags" : ["oven_settings::duration"],
           "value" : {
                "type" : "nominal_real",
                "units" : "seconds",
                "nominal" : 614
            }
        }
    ],
    "ingredients" : [
        {
            "type" : "link_by_uid",
            "scope" : "cookie ingredients",
            "id" : "chocolate chip cookie batter #45 in cookie 6"
        },
        {
            "type" : "link_by_uid",
            "scope" : "secret ingredients",
            "id" : "love #724 in cookie 6"
        }
    ],
    "output_material" : {
        "type" : "link_by_uid",
        "scope" : "cookie ids",
        "id" : "chocolate_chip_00038"
    }
}
```

---
## Ingredient Spec

The intent for an ingredient, which annotates a material with information related to its usage in an individual process.

Field name | Value type | Default | Description
-----------|------------|---------|------------
`uids`         | Map[String, String] | Empty | A collection of [Unique Identifiers](../unique-identifiers)
`type`         | String     | Req. | "ingredient_spec"
`name`| String     | Req. | The name of the ingredient, unique within the process that contains it
`labels`       | Set[String] | Empty | Additional labels on the ingredient for describing the type or role of the ingredient
`material`     | [Material Spec](./#material-spec) | Req. | Material that is this ingredient
`process`      | [Process Spec](./#process-spec) | Req. | Process that the ingredient is used in
`notes`       | String     | Empty | Some free-form notes about the spec.
`tags`        | Set[String]| Empty | [Tags](../tags)
`file_links`  | Set\[[File Links](../file-links)] | Empty | Links to associated files, with resource paths into the files API
`mass_fraction` | [Real Value](../value-types#real-values) | None | The mass fraction of the ingredient in the process
`volume_fraction` | [Real Value](../value-types#real-values) | None | The volume fraction of the ingredient in the process
`number_fraction` | [Real Value](../value-types#real-values) | None | The number fraction of the ingredient in the process
`absolute_quantity` | [Real Value](../value-types#real-values) | None | The absolute quantity of the ingredient in the process


##### Constraints

Field name | Relationship | Field Name
-----------|:------------:|------------
len(`name`) | <=    | 128, UTF-8 Encoded
`mass_fraction` | <= | 1
`volume_fraction` | <= | 1
`number_fraction` | <= | 1

##### Example

```json
{
    "type" : "ingredient_spec",
    "material" :{
        "type" : "link_by_uid",
        "scope" : "id",
        "id" : "18d95397-4887-48f0-bdda-94a9a4c5ef45"
    },
    "process" :{
        "type" : "link_by_uid",
        "scope" : "my_scope",
        "id" : "a-cool-process"
    },
    "uids" : {
        "cookie_ids" : "choc_chip_spec_001_in_sandwich"
    },
    "notes" : "Chocolate chip cookies used in making an ice cream sandwich",
    "absolute_quantity" : {
        "type" : "nominal_integer",
        "nominal" : 2
    },
    "mass_fraction" : {
        "type" : "nominal_real",
        "nominal" : 0.35
    },
    "name" : "cookie",
    "labels" : ["faces"]
}
```

---
## Ingredient Run

A particular instance of an ingredient spec.

Field name | Value type | Default | Description
-----------|------------|---------|------------
`uids`         | Map[String, String] | Empty | A collection of [Unique Identifiers](../unique-identifiers)
`type`         | String     | Req. | "ingredient_run"
`name`| String     | Req. | The name of the ingredient, unique within the process that contains it
`labels`       | Set[String] | Empty | Additional labels on the ingredient for describing the type or role of the ingredient
`material`     | [Material Run](./#material-run) | Req. | Material that is this ingredient
`process`      | [Process Run](./#process-run) | Req. | Process that the ingredient is used in
`notes`       | String     | None | Some free-form notes about the run.
`tags`        | Set[String]| Empty | [Tags](../tags)
`file_links`  | Set\[[File Links](../file-links)] | Empty | Links to associated files, with resource paths into the files API
`mass_fraction` | [Real Value](../value-types#real-values) | None | The mass fraction of the ingredient in the process
`volume_fraction` | [Real Value](../value-types#real-values) | None | The volume fraction of the ingredient in the process
`number_fraction` | [Real Value](../value-types#real-values) | None | The number fraction of the ingredient in the process
`absolute_quantity` | [Real Value](../value-types#real-values) | None | The absolute quantity of the ingredient in the process
`spec`| [Ingredient Spec](./#ingredient-spec) | Req. | The spec of which this is a run

##### Constraints

Field name | Relationship | Field Name
-----------|:------------:|------------
`mass_fraction` | <= | 1
`volume_fraction` | <= | 1
`number_fraction` | <= | 1

##### Example

```json
{
    "type" : "ingredient_run",
    "material" :{
        "type" : "link_by_uid",
        "scope" : "id",
        "id" : "18d95397-4887-48f0-bdda-94a9a4c5ef45"
    },
    "process" :{
        "type" : "link_by_uid",
        "scope" : "my_scope",
        "id" : "a-cool-process"
    },
    "uids" : {
        "cookie_ids" : "choc_chip_run_4_in_sandwich_7"
    },
    "notes" : "Chocolate chip cookie batch 4 used in making an ice cream sandwich batch 7",
    "absolute_quantity" : {
        "type" : "uniform_integer",
        "lower_bound" : 2,
        "upper_bound" : 2
    },
    "mass_fraction" : {
        "type" : "normal_real",
        "mean" : 0.347,
        "std" : 0.002
    },
    "name" : "cookie",
    "labels" : ["faces"]
}
```


---
## Material Spec

The expectation for a material.
Materials have exactly one producing process.
Material specs may include expected properties,
but do so via the [PropertiesAndConditions](../attributes#properties-and-conditions) compound attribute.
In this way, material specs can associate an expected property value with the conditions under which it is expected.
For example, if a material is purchased and its Safety Data Sheet quotes a normal boiling point of 54 C,
a property is known even though there is never an explicit measurement of that property by a person in the lab.  It could
therefore be annotated with a Boiling Temperature of 54 C (property) at 1 atm (condition).

Field name | Value type | Default | Description
-----------|------------|---------|------------
`uids`        | Map[String, String] | Empty | A collection of [Unique Identifiers](../unique-identifiers)
`type`        | String     | Req. | "material_spec"
`name`| String     | Req. | The name of the spec
`notes`       | String     | None | Some free-form notes about the spec.
`tags`        | Set[String]| Empty | [Tags](../tags)
`file_links`  | Set\[[File Links](../file-links)] | Empty | Links to associated files, with resource paths into the files API
`template`    | [Material Template](../object-templates/#material-template) | None | A template bounding the valid values for properties of this material.
`properties`  | Set\[[PropertiesAndConditions](../attributes/#properties-and-conditions)] | Empty | Expected properties for the material spec
`process`     | [Process Spec](./#process-spec) | Req. | The Process Spec that produces this material

##### Constraints

All Attributes sharing an [Attribute Template](../attribute-templates) with an Attribute on the associated Object Template will be constrained by the (potentially tighter) bounds set in the `template` Material Template.

Field name | Relationship | Field Name
-----------|:------------:|------------
property names | must be unique | among property names

##### Example

```json
{
    "type" : "material_spec",
    "uids" : {
        "cookie_ids" : "choc_chip_spec_001",
        "id" : "18d95397-4887-48f0-bdda-94a9a4c5ef45"
    },
    "name" : "Chocolate Chip Cookie Spec",
    "notes" : "Material Spec for chocolate chip cookies",
    "template" : {
        "type" : "link_by_uid",
        "scope" : "cookie_templates",
        "id" : "choc_chip_001"
    },
    "properties" : [
        {
            "type" : "property_and_conditions",
            "property": {
                "type" : "property",
                "name" : "Cookie Composition",
                "origin" : "specified",
                "template" : {
                    "type" : "link_by_uid",
                    "scope" : "cookie_templates",
                    "id" : "choc_chip_comp_01"
                },
                "value" : {
                    "type" : "nominal_composition",
                    "quantities" : {
                        "flour" : 355,
                        "baking soda" : 6,
                        "baking powder" : 9,
                        "salt" : 8,
                        "butter": 225,
                        "granulated sugar" : 205,
                        "brown sugar" : 225,
                        "vanilla extract" : 15,
                        "eggs" : 50,
                        "chocolate chips" : 395,
                        "chopped nuts" : 225
                    }
                }
            },
            "conditions": []
        }
    ],
    "process_spec" : {
        "type" : "link_by_uid",
        "scope" : "id",
        "id" : "064148e6-1cce-4d89-bfde-7ecd0aa4632b"
    }
}
```

---
## Material Run

A particular instance of a material, e.g. a sample, ingot, or wafer.

Field name | Value type | Default | Description
-----------|------------|---------|------------
`uids`        | Map[String, String] | Empty | A collection of [Unique Identifiers](../unique-identifiers)
`type`        | String     | Req. | "material_run"
`name`| String     | Req. | The name of the material run
`notes`       | String     | None | Some free-form notes about the material run
`tags`        | Set[String]| Empty | [Tags](../tags)
`file_links`  | Set\[[File Links](../file-links)] | Empty | Links to associated files, with resource paths into the files API
`spec`        | [Material Spec](./#material-spec) | Req. | The material spec of which this is a run
`process`     | [Process Run](./#process-run) | Req. | The Process Run that produced this material
`measurements`  | Set\[[Measurement Run](./#measurement-run)] | Empty | characterizations of this Material Run
`sample_type`   | `experimental`, `production`, or `virtual`, `unknown` | `unknown` | Context of how this material was made to be


##### Constraints

Same as Material Spec, but with the `template` inherited from the `spec`, i.e. `spec.template`.

##### Example

```json
{
    "type" : "material_run",
    "uids" : {
        "cookie_ids" : "choc_chip_001_run_006"
    },
    "name" : "Chocolate Chip Cookie Run 006",
    "notes" : "Material Run for chocolate chip cookies",
    "sample_type" : "production",
    "spec" : {
        "type" : "link_by_uid",
        "scope" : "cookie_ids",
        "id" : "choc_chip_001"
    },
    "process" : {
        "type" : "link_by_uid",
        "scope" : "cookie_ids",
        "id": "choc_chip_proc_001_run_006"
    },
    "measurements" : [
        {
            "type" : "link_by_uid",
            "scope" : "cookie_ids",
            "id"    : "choc_chip_001_hedonic_006"
        }
    ]
}
```


---
## Measurement Spec

An expectation for a measurement, indicating the parameters of and conditions under which to perform the measurement.

Field name | Value type | Default | Description
-----------|------------|---------|------------
`uids`        | Map[String, String] | Empty | A collection of [Unique Identifiers](../unique-identifiers)
`type`        | String     | Req. | "measurement_spec"
`name`| String     | Req. | The name of the spec
`notes`       | String     | None | Some free-form notes about the spec.
`tags`        | Set[String]| Empty | [Tags](../tags)
`file_links`  | Set\[[File Links](../file-links)] | Empty | Links to associated files, with resource paths into the files API
`template`    | [Measurement Template](../object-templates/#measurement-template) | None | A template bounding the valid values for parameter and conditions of the measurement.
`parameters`  | Set\[[Parameters](../attributes/#parameters)] | Empty | Specified parameters for the measurement
`conditions`  | Set\[[Conditions](../attributes/#conditions)] | Empty | Specified conditions for the measurement

##### Constraints

All attributes sharing an [Attribute Template](../attribute-templates) with an attribute on the associated Object Template will be constrained by the (potentially tighter) bounds set in the `template`.

Field name      | Relationship   | Field Name
----------------|:--------------:|------------
property names  | must be unique | among property names
condition names | must be unique | among condition names
parameter names | must be unique | among parameter names

##### Example

```json
{
    "type" : "measurement_spec",
    "uids" : {
        "cookie_ids" : "choc_chip_hedonic_spec"
    },
    "name" : "Chocolate Chip Cookie Hedonic Index Measurement Spec",
    "notes" : "Measurement Specs for measuring the hedonic index of chocolate chip cookies",
    "template" : {
        "type" : "link_by_uid",
        "scope" : "cookie_templates",
        "id" : "choc_chip_hedonic"
    },
    "parameters" : [
        {
            "type" : "parameter",
            "name" : "Cookie Quantity",
            "notes" : "You'll want to eat at least this many cookies for the hedonic index test",
            "origin" : "specified",
            "template" : {
                "type" : "link_by_uid",
                "scope" : "cookie_templates",
                "id" : "cookie_count"
            },
            "value" : {
                "type" : "nominal_integer",
                "nominal" : 7
            }
        }
    ],
    "conditions" : [
        {
            "type" : "condition",
            "name" : "Cookie Temperature",
            "notes" : "Let them cool, or you'll burn your mouth and ruin the test.",
            "origin" : "specified",
            "template" : {
                "type" : "link_by_uid",
                "scope" : "cookie_templates",
                "id" : "cookie_eating_temp"
            },
            "value" : {
                "type" : "nominal_real",
                "nominal" : 320,
                "units" : "kelvin"
            }
        }
    ]
}
```

---
## Measurement Run

A particular instance of a measurement.

Field name | Value type | Default | Description
-----------|------------|---------|------------
`uids`        | Map[String, String] | Empty | A collection of [Unique Identifiers](../unique-identifiers)
`type`        | String     | Req. | "measurement\_run"
`name`| String     | Req. | The name of the measurement run
`notes`       | String     | None | Some free-form notes about the measurement run
`tags`        | Set[String]| Empty | [Tags](../tags)
`file_links`  | Set\[[File Links](../file-links)] | Empty | Links to associated files, with resource paths into the files API
`source`      | [Source](./#source) | None | provenance information for the measurement
`spec`        | [Measurement Spec](./#measurement-spec) | Req. | The measurement spec of which this is a run
`material`    | [Material Run](./#material-run) | Req. | The material run being measured
`parameters`  | Set\[[Parameters](../attributes/#parameters)] | Empty | Measured parameters for the measurement
`conditions`  | Set\[[Conditions](../attributes/#conditions)] | Empty | Measured conditions for the measurement
`properties`  | Set\[[Properties](../attributes/#properties)] | Empty | Measured properties for the measurement


##### Constraints

Field name      | Relationship   | Field Name
----------------|:--------------:|------------
property names  | must be unique | among property names
condition names | must be unique | among condition names
parameter names | must be unique | among parameter names

##### Example

```json
{
    "type" : "measurement_run",
    "uids" : {
        "cookie_ids" : "choc_chip_hedonic_run_006"
    },
    "name" : "Chocolate Chip Hedonic Measurement",
    "notes" : "Rate the cookies on a scale from 9.9-10",
    "measurement_spec" : {
        "type" : "link_by_uid",
        "scope" : "cookie_ids",
        "id" : "choc_chip_hedonic_spec"
    },
    "material_run" : {
        "type" : "link_by_uid",
        "scope" : "cookie_ids",
        "id": "choc_chip_001_run_006"
    },
    "properties" : [
        {
            "type" : "property",
            "name" : "Chocolate Chip Cookie Hedonic Index",
            "notes" : "delish",
            "origin" : "measured",
            "template" : {
                "type" : "link_by_uid",
                "scope" : "cookie_templates",
                "id" : "hedonic_index_prop"
            },
            "value" : {
                "type" : "nominal_real",
                "nominal" : 9.997,
                "units" : ""
            }
        }
    ],
    "conditions" : [
        {
            "type" : "condition",
            "name" : "Measured Cookie Temperature",
            "notes" : "used a thermopen",
            "origin" : "measured",
            "template" : {
                "type" : "link_by_uid",
                "scope" : "cookie_templates",
                "id" : "cookie_eating_temp"
            },
            "value" : {
                "type" : "uniform_real",
                "lower_bound" : "318.15",
                "upper_bound" : "318.25",
                "units" : "kelvin"
            }
        }
    ],
    "parameters" : [
        {
            "type" : "parameter",
            "name" : "Cookie Quantity",
            "notes" : "I kept going and lost count.",
            "origin" : "measured",
            "template" : {
                "type" : "link_by_uid",
                "scope" : "cookie_templates",
                "id" : "cookie_count"
            },
            "value" : {
                "type" : "uniform_integer",
                "lower_bound" : 7,
                "upper_bound" : 12
            }
        }
    ]
}
```
---
## Source

Provenance is the documented history of an Object.
This includes information such as who performed a measurement, the literature source describing the design of a process, or the purveyor and catalog number for a purchased material.
This type of information tends to have limited value for modeling and other analysis but is essential for verification and auditing.

At present the only type of source supported is who performed a task and when they did so.

Field name    | Value type | Default | Description
--------------|------------|---------|-------------
`type`        | String     | Req. | "performed_source"
`performed_by`| String | None | The person who performed the measurement
`performed_date`| String | None | The date the measurement was performed; ISO-8601 date-formatted string (YYYY-MM-DD or YYYY-MM-DDTHH:mm:SS)


##### Example

```json
{
    "type": "performed_source",
    "performed_by": "joe@abc.com",
    "performed_date": "2015-03-14T15:09:27"
}
```
