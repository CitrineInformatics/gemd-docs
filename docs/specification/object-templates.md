# Object Templates

Object Templates, like Attribute Templates, define a domain concept, e.g. convection baking in a standard residential oven, a cheesecake, or a taste test.
Unlike Specs, Object Templates define ranges validity.

Object Templates contain collections of
[Attribute Templates](../attribute-templates/)
that together constrain the values of an object's associated attributes to valid ranges.
They also define a canonical name, a description, and can be tagged.

The `parameters`, `properties`, and `conditions` fields in Object Templates are defined as sets of pair.
Each pair contains at
[Attribute Template](../attribute-templates/)
and a Bounds that further constraints the bounds in the Attribute template.
For example, the baking temperature might generally be defined to be between 100 degF and 1500 degF,
but a Process Template describing a specific oven model may constrain the baking temperature to be between 150 degF and 550 degF.
The Bounds must fall within the Attribute Template's Bounds, and should be set to those Bounds if no further constraint is desired.
Each Attribute Template can only be included in each Object Template once.

---
## Process Template

Process templates are further able to constrain the names and labels on their ingredients.
If those sets are empty, then no constraint is implied.

Field name    | Value type | Default | Description
--------------|------------|---------|------
`uids`        | Map[String, String] | Empty | A collection of [Unique Identifiers](../unique-identifiers)
`type`        | String     | Req. | "process\_template"
`tags`        | Set[String]| Empty set | [Tags](../tags)
`name`        | String | Req. | The name of the template
`allowed_names` | Set[String] | Empty set | The set of names that ingredients are allowed to use in their `name` field
`allowed_labels` | Set[String] | Empty set | The set of labels that ingredients are allowed to use in their `labels` field
`description` | String | None | Some text describing what this template is for.
`conditions`  | Set\[Pair([Condition Templates](../attribute-templates), [Bounds](../attribute-templates))] | Empty set | Templates for associated conditions and bounds narrowing the allowable range
`parameters`  | Set\[Pair([Parameter Templates](../attribute-templates), [Bounds](../attribute-templates))] | Empty set | Templates for associated parameters and bounds narrowing the allowable range

##### Constraints

Field name | Relationship | Field Name
-----------|:------------:|------------
len(`name`) | <=    | 128, UTF-8 Encoded
len(`description`)  | <=    | 32,768 (32KB), UTF-8 Encoded
Bounds | contained in | Attribute template bounds
parameters templates | are unique within | parameters
condition templates | are unique within | conditions

##### Example

This is an example of a process template that has fully represented attribute templates.

```json
{
    "type" : "process_template",
    "uids" : {
        "cookie_templates" : "baking_process_01",
        "id" : "064148e6-1cce-4d89-bfde-7ecd0aa4632b"
    },
    "tags" : [
        "baking::cookies"
    ],
    "name" : "Bake Cookies",
    "description" : "Template for baking cookies in an oven",
    "parameters" : [
        [
            {
                "name" : "Oven Temperature",
                "uids" : {"cookie_templates": "oven_temp"},
                "tags" : ["oven_settings::temperature"],
                "description" : "A template for valid temperature ranges for baking cookies. Below 328K you're not even pasteurizing the dough.",
                "bounds" : {
                    "default_units" : "kelvin",
                    "type" : "real_bounds",
                    "lower_bound" : 328,
                    "upper_bound" : 750
                }
            },
            {
                "type" : "real_bounds",
                "default_units" : "kelvin",
                "lower_bound" : 400,
                "upper_bound" : 500
            }
        ],
        [
            {
                "name" : "Baking Time",
                "uids" : {"cookie_templates": "oven_time"},
                "tags" : ["oven_settings::duration"],

                "description" : "A template for valid duration ranges for baking cookies.",
                "bounds" : {
                    "default_units" : "seconds",
                    "type" : "real_bounds",
                    "lower_bound" : 0,
                    "upper_bound" : 86400
                }
            },
            {
                "type" : "real_bounds",
                "default_units" : "seconds",
                "lower_bound" : 300,
                "upper_bound" : 7200
            }
        ]
    ]
}
```


---
## Material Template

The Property Templates and Bounds contained in a material template are used to validate the properties in any
[`PropertyAndCondition`](../attributes/#properties-and-conditions)s in the
[Material Spec](../objects/#material-spec).
They are not, however, used to validate the properties in any
[Measurement Run](../objects/#measurement-run) objects attached to the
[Material Run](../objects/#material-run).
Rather, those properties are validated by the
[Measurement Template](../object-templates/#measurement-template).

Field name    | Value type | Default | Description
--------------|------------|---------|--------------
`uids`        | Map[String, String] | Empty | A collection of [Unique Identifiers](../unique-identifiers)
`type`        | String     | Req. | "material\_template"
`tags`        | Set[String]| Empty set | [Tags](../tags)
`name`| String | Req. | The name of the template
`description` | String | None | Some text describing what this template is for.
`properties`  | Set\[Pair([Property Templates](../attribute-templates), [Bounds](../attribute-templates))] | Empty set | Templates for associated properties and bounds narrowing the allowable range


##### Constraints

Field name | Relationship | Field Name
-----------|:------------:|------------
len(`name`) | <=    | 128, UTF-8 Encoded
len(`description`)  | <=    | 32,768 (32KB), UTF-8 Encoded
property templates | are unique within | properties

##### Examples

```json
{
    "type" : "material_template",
    "uids" : {
        "cookie_templates" : "choc_chip",
        "id" : "2e1bec7e-bda4-441d-bebb-1215bfa6ee0f"
    },  
    "tags" : [],
    "name" : "Chocolate Chip Cookie Template",
    "description" : "Template for chocolate chip cookie materials",
    "properties" : [
        [
            {
                "uids" : {"cookie_templates" : "choc_chip_comp_01"},
                "tags" : ["ingredients::cookies::nutsallowed"],
                "name" : "Chocolate Chip Cookie Composition",
                "description" : "Specifying the composition of the linked cookie",
                "bounds" : {
                    "type" : "categorical_bounds",
                    "categories" : [
                        "flour",
                        "baking soda",
                        "baking powder",
                        "salt",
                        "butter",
                        "granulated sugar",
                        "brown sugar",
                        "vanilla extract",
                        "eggs",
                        "chocolate chips",
                        "chopped nuts"
                    ]
                }
            },
            {
                "type" : "categorical_bounds",
                "categories" : [
                    "flour",
                    "baking soda",
                    "baking powder",
                    "salt",
                    "butter",
                    "granulated sugar",
                    "brown sugar",
                    "vanilla extract",
                    "eggs",
                    "chocolate chips",
                    "chopped nuts"
                ]
            }
        ]
    ]
}
```

---
## Measurement Template

Field name    | Value type | Default | Description
--------------|------------|---------|------------
`uids`        | Map[String, String] | Empty |  A collection of [Unique Identifiers](../unique-identifiers)
`type`        | String     | Req. | "measurement_template"
`tags`        | Set[String]| Empty set | [Tags](../tags)
`name`| String | Req. | The name of the template
`description` | String | None | Some text describing what this template is for.
`conditions`  | Set\[Pair([Condition Templates](../attribute-templates), [Bounds](../attribute-templates))] | Empty set | Templates for associated conditions and bounds narrowing the allowable range
`parameters`  | Set\[Pair([Parameter Templates](../attribute-templates), [Bounds](../attribute-templates))] | Empty set | Templates for associated parameters and bounds narrowing the allowable range
`properties`  | Set\[Pair([Property Templates](../attribute-templates), [Bounds](../attribute-templates))] | Empty set | Templates for associated properties and bounds narrowing the allowable range


##### Constraints

Field name | Relationship | Field Name
-----------|:------------:|------------
len(`name`) | <=    | 128, UTF-8 Encoded
len(`description`)  | <=    | 32,768 (32KB), UTF-8 Encoded
property templates | are unique within | properties
parameters templates | are unique within | parameters
condition templates | are unique within | conditions

##### Examples

```json
{
    "type" : "measurement_template",
    "uids" : {
        "cookie_templates" : "choc_chip_hedonic",
        "id" : "664e5b79-4c16-46db-aa9e-ff70d79d2f79"
    },
    "tags" : [],
    "name" : "Chocolate Chip Cookie Hedonic Index Measurement Template",
    "description" : "Template for chocolate chip cookie materials",
    "properties" : [
        [
            {
                "uids" : {"cookie_templates": "hedonic_index_prop"},
                "tags" : [],
                "name" : "Chocolate Chip Cookie Hedonic Index",
                "description" : "The allowable range for the hedonic index of chocolate chip cookies.",
                "bounds" : {
                    "type" : "real_bounds",
                    "lower_bound" : 9.99,
                    "upper_bound" : 10,
                    "default_units" : ""
                }
            },
            {
                "type" : "real_bounds",
                "lower_bound" : 9.99,
                "upper_bound" : 10,
                "default_units" : ""
            }
        ]
    ],
    "conditions" : [
        [
            {
                "name" : "Cookie Temperature",
                "uids" : {"cookie_templates": "cookie_eating_temp"},
                "tags" : [],
                "description" : "A template for valid temperature ranges for eating cookies.",
                "bounds" : {
                    "default_units" : "kelvin",
                    "type" : "real_bounds",
                    "lower_bound" : 250,
                    "upper_bound" : 380
                }
            },
            {
                "default_units" : "kelvin",
                "type" : "real_bounds",
                "lower_bound" : 250,
                "upper_bound" : 380
            }
        ]
    ],
    "parameters": [
        [
            {
                "name" : "Number of Cookies",
                "uids" : {"cookie_templates": "cookie_count"},
                "tags" : ["chocula"],
                "description" : "A template for the number of cookies to eat for a hedonic index test.",
                "bounds" : {
                    "type" : "integer_bounds",
                    "lower_bound" : 1,
                    "upper_bound" : 1000
                }
            },
            {
                "type" : "integer_bounds",
                "lower_bound" : 1,
                "upper_bound" : 1000
            }
        ]
    ]
}
```
