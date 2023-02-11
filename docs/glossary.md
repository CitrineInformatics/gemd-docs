# Glossary

### Attribute
A characteristic of a material, process, or measurement. Attributes can be a GEMD property, condition, or parameter.

### Attribute Template
Attribute templates define a domain concept (e.g. density, yield strength), and include bounds to define acceptable values for a given attribute.

### Bounds
Bounds are used in Object and Attribute templates. Object templates define the least constrained bounds, and attribute templates can constrain bounds to a tighter range or set.  Bounds may be real or categorical.

### Categorical Bounds
Bounds that are defined by a list of categories.

### Condition Attribute
Environmental variables (typically measured) that may affect a process or measurement, such as temperature or pressure.

### Conditions
Environmental aspects of a process or measurement.

### Data Object
GEMD Materials, Processes, Measurements, and Ingredients. Data objects along with attributes are linked to form a complete description of a material and how it was created.

### GEMD
Graphical Expression of Materials Data, an open source format for storing materials data

### Ingredient
A material that is annotated with information related to its usage in a process. This information is stored in an "Ingredient Object" in the GEMD format, which is a type of Data Object. The Ingredient Object contains information about the material's role in the process, such as its quantities, properties, and conditions. This information is used to describe the relationships between materials and processes in a comprehensive and interconnected manner.

### Ingredient Object
A  data object that annotates a material with information related to its usage in a process. Ingredient objects are used define how much of a material is used in a new process. Note that 'ingredient' and 'material' object definitions are not interchangeable.

### Material
A material in GEMD is described as a physical substance that is used in processes or measurements and is characterized by its properties. The information related to a material, including its properties and usage in a process, is stored in the material object, which is a type of data object in GEMD. The material object is interconnected with other data object in GEMD, including Processes, Measurements, and Ingredients, to provide a comprehensive representation of materials data.

### Material Object
GEMD data object that describes a material and its properties

### Material Spec
A specification of a material, such as a purchased ingredient or reference material, typically from a data sheet.

### Measurement Run
A recorded measurement of a property. 

### Measurement Object
A data object used to describe an operation that measures or characterizes properties of a material.

### Nominal Real Value
A value type in which the value is recorded as a specific number, with no information about the uncertainty of that number.

### Object Template
Object Templates define a domain concept. They contain collections of Attribute Templates that together constrain the values of an object's associated attributes to valid ranges.

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


