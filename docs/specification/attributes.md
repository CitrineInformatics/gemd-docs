# Attributes

**Properties** are characteristics of a material that could be measured (e.g., chemical composition, density, or yield strength).

> I recorded a Measurement Run of the density of my cookie.
I measured the property to be a [Nominal Real Value](../value-types/#nominal-real-value) of 604 kg/m³.

**Conditions** are the environmental variables (typically measured) that may affect a process or measurement: e.g. Temperature, Pressure.
>  The reading on the thermometer inside my oven as I bake cookies was 355 degrees, and I know that my thermometer is only accurate to +- 5 degrees, so I'll make that a [Uniform Real Value](../value-types/#uniform-real-value) with a `lower_bound` of 350 and an `upper_bound` of 360.

**Parameters** are the non-environmental variables (typically specified and controlled) that may affect a process or measurement: e.g. Oven Dial Temperature Position for a kiln firing, or Magnification for a measurement taken with a SEM.
>  The "Bake Cookies" Process Spec has two parameters: a [Nominal Real Value](../value-types/#nominal-real-value) of 30 minutes for bake duration, and a [Nominal Real Value](../value-types/#nominal-real-value) of 350 degrees for oven temperature setting

> I know my oven tends to run cold, so as I was baking I set my temperature setting to 360 degrees.
I recorded this in the Process Run as a parameter with a [Nominal Real Value](../value-types/#nominal-real-value) of 360 Degrees.

Typically, conditions are going to apply to _measured_ environmental variables in Process Runs and Measurement Runs.
It may be appropriate to specify a Parameter attribute on a Spec, and describe that attribute as a Condition on Runs of that Spec if the value is being measured as opposed to controlled during the Run.
It may also be appropriate to include _both_ a Parameter and a Condition on the Run if the value is both controlled and measured.
The use of Conditions in Specs should be limited in favor of parameters.

Attributes may be annotated with an [Attribute Template](../attribute-templates), which defines a canonical name and bounds on the attribute.

#### PropertyAndConditions

**PropertyAndConditions** are known or unmeasured Properties (at specified Conditions) of a [Material Spec](../objects/#material-spec). Typically, these will come from technical specification sheets of purchased ingredients or reference materials such as safety data sheets (SDS).
> I purchased 100% Ethanol. According to the SDS, pure ethanol has a Density (Property) of 0.789 g/cc at 20 degC (Condition 1) and 1 atm (Condition 2). I will add this as a PropertyAndConditions to the ethanol [Material Spec](../objects/#material-spec). This PropertyAndConditions will have the density Property in the property field and a List containing both Conditions in the conditions field.

> I purchased 80% Ethanol. According to the Technical Specifications, the solution is 80% ethanol and 20% water (Property). I will add this [Compositional Value](../value-types/#composition) as a PropertyAndConditions to the [Material Spec](../objects/#material-spec). However, this PropertyAndConditions will only have information in the property field, as no associated conditions are needed.

#### Origin

Attributes are annotated with the `origin` of the data.  This field can have the following values:

- `measured`: The Value of this Attribute was directly measured.
- `predicted`: The Value of this Attribute came from a model, such as a complex simulation, a machine learning--derived computation or rule-of-thumb estimation
- `summary`: The Value of this Attribute is calculated based on a set of the same property at a finer level of granularity (aggregation of data).
- `specified`: The Value of this Attribute was dictated, such as the oven temperature in a [Process Spec](../objects#process-spec).  This value should only appear in Specs.
- `computed`: The Value of this Attribute was derived directly from measured values, such as computing the yield stress from a stress-strain curve or computing the density from known mass and volume measurements.
- `unknown`: The origin of this Value is unknown.  This is the default value.

---

## Property, Condition, Parameter

All three types of attributes have the same fields.

Field name   | Value type | Default | Description
-------------|------------|---------|-------------
`type`       | String     | Req.    | One of: `property`, `condition`, `parameter`
`value`      | [Value](../value-types) | Req. | Any `Value` type
`name`       | String     | Req. | The name of the attribute, which is used to identify it within a Data Object
`notes`      | String     | None | Some free-form notes about the attribute.
`origin`     | `measured`, `predicted`, `summary`, `specified`, `computed`, `unknown` | `unknown` | The origin of the attribute
`template`   | [Attribute Template](../attribute-templates) | None | Attribute Template which defines bounds
`file_links` | Set[\[File Links](../file-links)] | Empty set | Links to associated files, with resource paths into the files API

##### Constraints

Field name    | Relationship | Field Name
--------------|:------------:|------------
len(`name`)   | <=           | 128, UTF-8 Encoded
len(`notes`)  | <=           | 32,768 (32KB), UTF-8 Encoded
`template`    | is a         | template type that matches the attribute type, e.g. `PropertyTemplate` for a `Property`
`value`       | is a         | `template`'s Value Type
`value`       | conforms to  | `template`.`bounds`

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
    "name" : "A Real Valued Condition",
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
    "name" : "A Real Valued Parameter",
    "value" : {
        "type" : "nominal_real",
        "nominal" : 1.0,
        "units": "degC"
    },
    "origin" : "specified",
}
```

## PropertyAndConditions


Field name   | Value type | Default | Description
-------------|------------|---------|-------------
`type`       | String     | Req.    | `property_and_conditions`
`property`   | Property | Req. | Property of the [Material Spec](../objects/#material-spec)
`conditions` | Set[Condition]     | None | List of conditions the Property is valid at


##### Examples

```json
{
    "type": "property_and_conditions",
    "conditions": [{
        "type": "condition",
        "name": "ambient temperature",
        "origin": "unknown",
        "value": {
            "nominal": 20.0,
            "type": "nominal_real",
            "units": "degree_Celsius"
        },

    },
    {
        "type": "condition",
        "name": "atmospheric pressure",
        "origin": "unknown",
        "value": {
            "nominal": 1.0,
            "type": "nominal_real",
            "units": "atm"
        }
    }],
    "property": {
        "type": "property",
        "name": "density",
        "origin": "unknown",
        "value": {
            "nominal": 0.7893,
            "type": "nominal_real",
            "units": "gram / cubic_centimeter"
        }
    }
 }

```
