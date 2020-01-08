# Value Types

Value is a generic term for the information contained in an [attribute](../attributes).
A Value may be one of the following Value Types:

| Value Type       | Variant                                        | Description                                                                                                                                                                                                |
|------------------|------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **RealValue**    | `NominalReal`<br>`NormalReal`<br>`UniformReal` | A double value and a unit (e.g., 32F, 45 meters, 14 kg).<br><br>Each variation provides a different mechanism for defining uncertainty.                                                                    |
| **IntegerValue** | `NominalInteger`<br>`UniformInteger`           | A whole integer value.                                                                                                                                                                                     |
| **Categorical**  | `NominalCategorical`<br>`DiscreteCategorical`  | Categorical values are distributions over the valid category names representing the probability of each category.  For DiscreteCategorical, the values must sum to 1.0.                                    |
| **Composition**  | `NominalComposition`<br>`EmpiricalFormula`     | A value containing the composition of the material as a map from the names of the components to their numerical quantities. The quantities are not required to be expressed on a unit or fractional basis. |
| **Molecular**    | `Smiles`<br>`Inchi`                            | Molecular structure using popular encoding schemes                                                                                                                                                         |

<!-- We might add these back later, but they are currently unused
**Boolean**	 |       |A true/false value (e.g., active, deleted)
**String**	 |       |Any string value (e.g., email, notes, zip code, first name, etc.)
-->

Many of these values represent probability distributions, e.g. normal and uniform distributions over
continuous bounds or discrete distributions over categorical ones.

Each of the Value Types is described below.
Every field is required.

---

## Real Values

Two common distributions are supported: normal (i.e. Gaussian) and uniform.
In cases when there is no distributional information, the "nominal" value type
can be used to specify the expected value.

### Normal Real Value

Normally distributed value parameterized by a real-valued mean and standard deviation.

A normal distribution is considered valid with a set of [Real Bounds](../attribute-templates#real-bounds)
if the mean lies between the upper and lower bound; the width of the distribution
is not considered.
From a computational perspective, this means Normal Real Values are actually
truncated Gaussians, though this is only significant for a distribution
near the boundary.

| Field name | Value type | Description                                   |
|------------|------------|-----------------------------------------------|
| `type`     | String     | "normal\_real"                                |
| `mean`     | Number     | Mean of the distribution                      |
| `std`      | Number     | Standard deviation of the normal distribution |
| `units`    | String     | A String describing the units                 |

##### Constraints

| Field name | Relationship | Field Name |
|------------|:------------:|------------|
| `std`      |      >=      | 0          |

##### Example

```json
{
    "type" : "normal_real",
    "units" : "kelvin",
    "mean": 350,
    "std": 1.03
}
```


### Uniform Real Value

A Uniform continuous distribution value, with inclusive lower and upper bounds.
These are especially useful for expressing uncertainty when the number of digits is truncated.
In order to be valid, the entirety of the distribution must fall within the bounds.

| Field name    | Value type | Description                     |
|---------------|------------|---------------------------------|
| `type`        | String     | "uniform\_real"                 |
| `lower_bound` | Number     | Lower bound of the distribution |
| `upper_bound` | Number     | Upper bound of the distribution |
| `units`       | String     | A String describing the units   |
##### Constraints

| Field name    | Relationship | Field Name    |
|---------------|:------------:|---------------|
| `lower_bound` |      <=      | `upper_bound` |

##### Example

A value field read off a digital display with 3 decimal points.

```json
{
    "type" : "uniform_real",
    "units" : "meter",
    "lower_bound": 0.9995,
    "upper_bound": 1.0005
}
```

### Nominal Real Value

Nominal value, which does not specify an uncertainty but is not to be assumed to be exact.

| Field name | Value type | Description                               |
|------------|------------|-------------------------------------------|
| `type`     | String     | "nominal\_real"                           |
| `nominal`  | Number     | A nominal value - not assumed to be exact |
| `units`    | String     | A String describing the units             |

##### Constraints

None

##### Example

```json
{
    "type" : "nominal_real",
    "units" : "meter",
    "nominal": 1.0
}
```

---

## Integer Values

Currently, there are two integer types: a uniform (i.e. range) distribution and a
nominal (i.e. specified) value.

### Uniform Integer Value

A uniform integer distribution value, with inclusive lower and upper bounds.
These are especially useful for expressing uncertainty when the number of digits is truncated.
In order to be valid, the entirety of the distribution must fall within the bounds.

| Field name    | Value type | Description                     |
|---------------|------------|---------------------------------|
| `type`        | String     | "uniform\_integer"              |
| `lower_bound` | Integer    | Lower bound of the distribution |
| `upper_bound` | Integer    | Upper bound of the distribution |

##### Constraints

| Field name    | Relationship | Field Name    |
|---------------|:------------:|---------------|
| `lower_bound` |      <=      | `upper_bound` |

##### Example

A value field read off a digital display with 3 decimal points.
```json
{
    "type" : "uniform_integer",
    "lower_bound": 246,
    "upper_bound": 255
}
```

### Nominal Integer Value

Nominal value, which does not specify an uncertainty but is not to be assumed to be exact.

| Field name | Value type | Description                               |
|------------|------------|-------------------------------------------|
| `type`     | String     | "nominal\_integer"                        |
| `nominal`  | Integer    | A nominal value - not assumed to be exact |

##### Constraints

None

##### Example

```json
{
    "type" : "nominal_integer",
    "nominal": 42
}
```

---

## Categorical

Categorical values are distributions over the valid category names representing the probability
of each category.
These are different than mixtures; 60% water and 40% ethanol is not a categorical distribution.
For those, see `Composition`.

Currently, there are two categorical types:
a discrete (i.e. enumerated) distribution and a nominal (i.e. specified) value.

### Discrete Categorical Value

Distribution of categories stored as a map from the string label to the probability

| Field name      | Value type          | Description                                            |
|-----------------|---------------------|--------------------------------------------------------|
| `type`          | String              | "discrete\_categorical"                                |
| `probabilities` | Map[String, Number] | A map from string category names to their probability. |

##### Constraints

| Field name                             | Relationship | Field Name |
|----------------------------------------|:------------:|------------|
| abs(sum(probabilities.values()) - 1.0) |      <       | 1.0e-9     |
| each probability value                 |      >=      | 0          |

##### Example

```json
{
    "type":"discrete_categorical",
    "probabilities" : {
        "red" : 0.54,
        "blue" : 0.46
    }
}
```

### Nominal Categorical Value

Nominal value, which does not specify an uncertainty but is not to be assumed to be exact.

| Field name | Value type | Description               |
|------------|------------|---------------------------|
| `type`     | String     | "nominal\_categorical"    |
| `category` | String     | The category of the value |

##### Constraints

None

##### Example

```json
{
    "type" : "nominal_categorical",
    "category" : "red"
}
```

---

## Composition
A value representing the composition of the material as a set of component names and their respective quantities.
The quantities are not required to be expressed on a unit or fractional basis.
For example "one part flour two parts sugar" is acceptable.

See [Known Limitations](../../known_limitations).  **Support for these Value Types is still pending.**

### Nominal Composition

A composition represented as a map from the component name to the quantity.
The quantities do not express an uncertainty, but also do not imply that there is absolute certainty in their values.

| Field name   | Value type          | Description            |
|--------------|---------------------|------------------------|
| `type`       | String              | "nominal\_composition" |
| `quantities` | Map[String, Number] | Map[String, Number]    |

##### Constraints

| Field name          | Relationship | Field Name |
|---------------------|:------------:|------------|
| each quantity value |      >=      | 0          |

##### Example

```json
{
    "type" : "nominal_composition",
    "quantities" : {
        "water" : 120,
        "ethanol" : 80
    }
}
```

### Empirical Formula

A composition represented as a chemical formula string.
The order and grouping of the elements is ignored.

| Field name | Value type | Description                                           |
|------------|------------|-------------------------------------------------------|
| `type`     | String     | "empirical\_formula"                                  |
| `formula`  | String     | Chemical formula to be parsed as an empirical formula |


##### Example

```json
{
    "type" : "empirical_formula",
    "formula" : "SiO2"
}
```

---

### Molecular Structure

Molecular structure types are used to define attributes that contain information about
the structure and composition of a molecule.
Given a molecular structure, one can infer the chemical formula, existence of functional groups,
and local chemical environment of each atom in the molecule.
Most commonly a molecular structure will refer to the entire material,
but multiple molecular structures can be used to define fragments of a material, e.g. monomers of a polymer.

See [Known Limitations](../../known_limitations).  We plan to (**but do not yet**) support two ways of representing a molecular structure:

* SMILES string
* InChI string

See [Known Limitations](../../known_limitations).  **Support for these Value Types is still pending.**

#### SMILES Value

A value containing a [SMILES string](https://en.wikipedia.org/wiki/Simplified_molecular-input_line-entry_system).

| Field name | Value type | Description     |
|------------|------------|-----------------|
| `type`     | String     | "smiles"        |
| `smiles`   | String     | A SMILES string |


##### Example

```json
{
    "type" : "smiles",
    "smiles" : "c1(C=O)cc(OC)c(O)cc1"
}
```

#### InChI Value

A value containing an [InChI string](https://en.wikipedia.org/wiki/International_Chemical_Identifier).
Note: this is not the same as the InChI key.

| Field name | Value type | Description     |
|------------|------------|-----------------|
| `type`     | String     | "inchi"         |
| `inchi`    | String     | An InChI string |


##### Example

```json
{
    "type" : "inchi",
    "inchi" : "InChI=1/C8H8O3/c1-11-8-4-6(5-9)2-3-7(8)10/h2-5,10H,1H3"
}
```
