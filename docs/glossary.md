# Glossary

### Attribute
A characteristic of a material, process, or measurement, which can be a property, condition, or parameter. (FIXME vs template?)

### Attribute Template
A domain concept that defines a canonical name and bounds, which describe the values that are acceptable for an attribute. (FIXME this is super circular)
Another attempt: 
Attribute Templates are used in Object Templates to define the ranges of validity for an object's associated attributes. The parameters, properties, and conditions fields in Object Templates are defined as sets of pairs, each containing an Attribute Template and a Bounds that further constrains the bounds in the Attribute template.

### Bounds
Bounds are used in Object Templates to further constrain the bounds in the Attribute templates. The Bounds must fall within the Attribute Template's Bounds, and should be set to those Bounds if no further constraint is desired.     May be real or categorical.

### Categorical Bounds
Bounds defined by a list of categories.

### Compositional Value
A value type that represents a composition of a material, such as the composition of a solution. (FIXME look this one up)

### Condition Attribute
Environmental variables (typically measured) that may affect a process or measurement, such as temperature or pressure. ( FIXME Is the attribute sense correct here?)

### Conditions
Environmental aspects of a process or measurement in GEMD.

### Data Object
GEMD Materials, Processes, Measurements, and Ingredients (Interconnected?)

### GEMD
Graphical Expression of Materials Data, an open source format for storing materials data

### Ingredient
In the context of GEMD (Graphical Expression of Materials Data), an ingredient is defined as a material that is annotated with information related to its usage in a process. This information is stored in an "Ingredient Object" in the GEMD format, which is a type of Data Object. The Ingredient Object contains information about the material's role in the process, such as its quantities, properties, and conditions. This information is used to describe the relationships between materials and processes in a comprehensive and interconnected manner.

### Ingredient Object
GEMD data object that annotates a material with information related to its usage in a process

### Material
A material in GEMD is described as a physical substance that is used in processes or measurements and is characterized by its properties. The information related to a material, including its properties and usage in a process, is stored in the material object, which is a type of data object in GEMD. The material object is interconnected with other data object in GEMD, including Processes, Measurements, and Ingredients, to provide a comprehensive representation of materials data.

### Material Object
GEMD data object that describes a material and its properties

### Material Spec
A specification of a material, such as a purchased ingredient or reference material, typically from a data sheet.

### Measurement Run
A recorded measurement of a property. 

### Measurement Object
GEMD data object that describes an operation to measure or characterize properties of a material

### Nominal Real Value
A value type in which the value is recorded as a specific number, with no information about the uncertainty of that number.

### Object Template
Object Templates define a domain concept, such as baking in a standard residential oven or a taste test. They contain collections of Attribute Templates that together constrain the values of an object's associated attributes to valid ranges. They also define a canonical name, a description, and can be tagged.

### Parameter Attribute
Describes the settings of a tool in a process or measurement object. Parameters are non-environmental variables (typically specified and controlled) that may affect a process or measurement, such as oven dial temperature position for a kiln firing, or magnification for a measurement taken with a SEM.

### PIF
Portable Information Format, a precursor to GEMD that is no longer in use

### Process Object
GEMD data object that describes an operation to generate an output material from inputs

### Process Run
A recorded run of a process, which may include parameters that are controlled and/or measured.

### Process Template
Process templates are used to define constraints for the names and labels of ingredients. They may link conditions and parameters. (FIXME) 

### Property Attribute
Describes a measured or calculated property of a material object. A property is a characteristic of a material that can be measured, such as chemical composition, density, or yield strength.

###  PropertyAndConditions
Known or unmeasured properties of a Material Spec, with specified conditions. (FIXME this def is not good, this is really squishy)

### Real Bounds
Bounds defined by a range of finite real values and a unit.

### State
Template, spec and run. Data objects are created in a state, representing generalization (template), intent (spec), and actual result (run)

### Tag
Tags are used to categorize Object Templates. (FIXME example)

### Uniform Real Value
A value type in which the value is recorded as a range, with a lower bound and an upper bound, to represent the uncertainty of the measurement.


## FIXME notes
- Should each data object/state pair have an entry? Probably?
- check coverage... (mabye just by types)
- examples should be inline or as blocks?
- -   Origin: The source of an attribute's data, which can be measured, predicted, calculated from a set of finer-level data, specified, derived from measured values, or unknown. check this