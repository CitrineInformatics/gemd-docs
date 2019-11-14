# FAQ

It's not you; it's us.
Taurus is not a simple representation, and
we frequently get questions about how to use it to represent certain kinds of data.
This page attempts to answer some of those questions.

## Where do I store statistics about my measurements

Let's say that you have data for the same kind of measurement being repeated multiple times.
For example, you take a sample from your material, subdivide it 8 times, and perform the
same measurement on each of those 8 sub-samples.
The measurement that you are performing can be represented by a single
[`MeasurementSpec`](../specification/objects/#measurement-spec)
and each of those 8 sub-samples as its own
[`MeasurementRun`](../specification/objects/#measurement-run)
associated with that spec.
Each
[`MeasurementRun`](../specification/objects/#measurement-run)
is associated with the
[`MatererialRun`](../specification/objects/#measurement-run)
representing the material that you sampled and sub-divided.

For each property in the
[`MeasurementRun`](../specification/objects/#measurement-run)
objects, you may want to compute some statistics of the 8-sample distribution.
For example: the mean and the standard deviation of each property and/or its minimum and maximum values.
In the future, these statistics will be automatically computed on the Citrine platform.
For now, you can compute them yourself and record them in another
[`MeasurementRun`](../specification/objects/#measurement-run)
object distinct from the 8 samples.
If there are multiple properties and/or multiple statistics, they should all go in the same `MeasurementRun`.
The statistics should be
[`Property`](../specification/attributes/#property)
attributes with their `origin` field set to `computed`.

## How do I represent repeated applications of the same process?

Consider a situation in which a process is repeatedly applied to a material, but does not substantially change the material.
For example, repeated application of heat-treatment to harden a ceramic or multiple coats of paint.
Because material histories are chronological, each application must be a new process and must produce a new output material.
The materials/processes may reference the same templates, however, if the same attributes are relevant for each iteration.

## How do I represent repeated uses of the same material?

It is perfectly fine for the same material to be used in multiple processes throughout a material history, but a new ingredient must be created for each use.
Ingredients annotate the use of a specific material in a specific process, recording how much was used and labeling the role of the material.
Process templates should be used to describe what types of ingredients are expected in a process.

The same material can even be used multiple times in one process.
Consider the following example: a thin film is created by thermally evaporating three materials in succession onto a substrate.
The thermal evaporation process template specifies the allowed names as "layer 1," "layer 2," and "layer 3."
We have several materials to choose from for the three layers, but in one instance we wish to evaporate a layer of material "A," then a layer of "B," then a final layer of "A."
We create a process spec linked to the thermal evaporation process template and create three ingredient specs, each of which point to the process spec as their `process`.
One ingredient points to "A" as its `material` and has `layer 1` as its name.
One points to "B" as its `material` and has `layer 2` as its name.
And the final ingredient spec _also_ points to "A" as its `material` but is differentiated because it has `layer 3` as its name.

## What is the difference between `description` and `notes`?

`description` is a field on both [Attribute Templates](../specification/attribute-templates) and [Object Templates](../specification/object-templates).
It is used to describe the type of data that a template is intended to constrain; documentation of intended use.
It is strongly encouraged to document all templates, given both how important they are in constraining data and communicating structure to analysis algorithms, and in how much reuse a template is likely to get by a range of users on a given  platform.

`notes` are associated with with [Attributes](../specification/attributes) and [Objects](../specification/objects).
These are particular pieces of information which may be important in understanding this particular piece of data but do not naturally fit in other fields of these objects.
This might include an annotation about something unusual about this particular sample.
These would normally be information that is useful for a human who might review this record but would not be useful in training a machine learning model.
