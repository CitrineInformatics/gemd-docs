# Attributes

**Properties** are characteristics of a material that could be measured, e.g. chemical composition, density, or yield strength.

> I recorded a measurement run of the density of my cookie.
I measured the property to be a [Nominal Real Value](../value-types/#nominal-real-value) of 604 kg/m³.

**Conditions** are the environmental variables (typically measured) that may affect a process or measurement: e.g. Temperature, Pressure.
>  "The reading on the thermometer inside my oven as I bake cookies was 355 degrees, and I know that my thermometer is only accurate to +- 5 degrees, so I'll make that a [Uniform Real Value](../value-types/#uniform-real-value) with a `lower_bound` of 350 and an `upper_bound` of 360.

**Parameters** are the non-environmental variables (typically specified and controlled) that may affect a process or measurement: e.g. Oven Dial Temperature Position for a kiln firing, or Magnification for a measurement taken with a SEM.
>  The "Bake Cookies" Process Spec has two parameters: a [Nominal Real Value](../value-types/#nominal-real-value) of 30 minutes for bake duration, and a [Nominal Real Value](../value-types/#nominal-real-value) of 350 degrees for oven temperature setting

> I know my oven tends to run cold, so as I was baking I set my temperature setting to 360 degrees.
I recorded this in the process run as a parameter with a [Nominal Real Value](../value-types/#nominal-real-value) of 360 Degrees.

Typically, conditions are going to apply to _measured_ environmental variables in process runs and measurement runs.
It may be appropriate to specify a Parameter attribute on a Spec, and describe that attribute as a Condition on Runs of that Spec if the value is being measured as opposed to controlled during the Run.
It may also be appropriate to include _both_ a Parameter and a Condition on the Run if the value is both controlled and measured.
The use of Conditions in Specs should be limited in favor of parameters.

Attributes are annotated with the `origin` of the data.  This field can have the following values:

- `measured`: The Value of this Attribute was directly measured.
- `predicted`: The Value of this Attribute came from a model, such as a complex simulation, a machine learning-derived computation or rule-of-thumb estimation
- `specified`: The Value of this Attribute was dictated, such as the oven temperature in a [Process Spec](../objects#process-spec).  This value should only appear in Specs.
- `computed`: The Value of this Attribute was derived directly from measured values, such as computing the yield stress from a stress-strain curve or computing the density from known mass and volume measurements.
- `unknown`: The origin of this Value is unknown.  This is the default value.

Attributes may be annotated with an [Attribute Template](../attribute-templates), which defines a canonical name and bounds on the attribute.

---

## Property, Condition, Parameter

All three types of attributes have the same fields.

Field name   | Value type | Default | Description
-------------|------------|---------|-------------
`type`       | String     | Req.    | One of: `property`, `condition`, `parameter`
`value`      | [Value](../value-types) | Req. | Any `Value` type
`name`       | String    | Req. | The name of the attribute, which is used to identify it within a Data Object
`notes`      | String     | None | Some free-form notes about the attribute.
`origin`     | `measured`, `predicted`, `specified`, `computed`, `unknown` | `unknown` | The origin of the attribute
`template`   | [Attribute Template](../attribute-templates) | None | Attribute Template which defines bounds
`file_links` | Set[\[File Links](../file-links)] | Empty set | Links to associated files, with resource paths into the files API

##### Constraints

Field name | Relationship | Field Name
-----------|:------------:|------------
len(`name`) | <=    | 128, UTF-8 Encoded
len(`notes`)  | <=    | 32,768 (32KB), UTF-8 Encoded
`template` | is a | template type that matches the attribute type, e.g. `PropertyTemplate` for a `Property`
`value`| is a | `template`'s Value Type
`value`| conforms to | `template`.`bounds`

##### Examples

```json
{
    "type" : "property",
    "name" : "A Real Valued Property",
    "value" : {
        "type" : "uniform_real",
        "lower_bound" : 1.995,
        "upper_bound" : 2.005,
        "units": "inch"
    },
    "origin" : "measured",
    "template" : {
        "name" : "A Property Template",
        "type" : "property_template",
        "bounds" : {
            "type" : "real_bounds",
            "lower_bound" : 0.0,
            "upper_bound" : 10.0,
            "default_units": "meters"
        }
    },
    "file_links" : [
        {
            "filename" : "How-to-make-lucky-charms.pdf",
            "link" : "files/file/d8f12919-b201-4186-be95-10525eb4256a/version/2"
        }
    ]
}
```

```json
{
    "type" : "condition",
    "value" : {
        "type" : "uniform_real",
        "lower_bound" : 1.0,
        "upper_bound" : 2.0,
        "units": "degC"
    },
    "origin" : "measured",
    "template" : {
        "type" : "condition_template",
        "name" : "temperature",
        "bounds" : {
            "type" : "real_bounds",
            "lower_bound" : 0.0,
            "upper_bound" : 1.0e6,
            "default_units": "Kelvin"
        }
    }
}
```

```json
{
    "type" : "parameter",
    "value" : {
        "type" : "nominal_real",
        "nominal" : 1.0,
        "units": "degC"
    },
    "origin" : "specified",
}
```

## Properties and Conditions

In the [Material Spec](../objects#material-spec) object,
one may need to specify a property along with the conditions under which the property occurs.
For example:

 - Vapor pressure of 5.6 kPa _at_ a temperature of 35 deg C
 - Gas phase _at_ a pressure of 1 kPa and a temperature of 300 K

This is supported by the `PropertyAndConditions` compound attribute.

Field name   | Value type | Default | Description
-------------|------------|---------|-------------
`type`       | String     | Req.    | `property_and_conditions`
`property`   | Property   | Req.    | Any `Property` attribute
`conditions` | Set[Conditions] | Empty set | Any conditions associated with the property
