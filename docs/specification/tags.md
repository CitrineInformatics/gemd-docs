# Tags

For any identifiers that are uniquely associated with an object, we have [Alternative ID](../unique-identifiers/#alternative-ids).
However, there are a number of cases where an identifier spans multiple objects.
A great example of this would be a Batch ID for multiple outputs of something that is conceptually the same process.
Perhaps a user decides that they'd like to bake both a chocolate cake and a vanilla cake in the same oven at the same time.
The specified temperatures and durations are similar enough that this should work well.
They'd create two Process Runs--one outputting a chocolate cake Material Run, and one outputting a vanilla cake Material Run.
But there is nothing recording the fact that both cakes were in the oven together.
They can define a tag that would allow them to record this information with the id of their oven, and the date, "Oven\_14::2019-04-15", and apply that tag to both of the Process Runs and/or Material Runs.

---
## Tags

Tags are a flexible way to store hierarchical information about your data, or to store identifiers that span many objects.
Each tag is string valued, but tags can use `::` as a delimiter to define a hierarchy from broad to narrow.
However, care should be taken to ensure that the first class in the hierarchy is sufficiently specific such that there is diversity in the first class (or _prefix_) of hierarchical tags.
This way, the prefix can be used as a partition key for distributing data in a database.

Field name | Value type | Description
-----------|------------|------------
`tags`     | Set[String]| A set of strings

##### Constraints

Field name  | Relationship | Field Name
------------|:------------:|------------
len(`tags`) | <=           | 100
len(a tag)  | <=           | 256, UTF-8 encoded

##### Example

```javascript
{
    "tags" : [
        "product_13::batch_45",
        "kiln_37::2019-04-12",
        "Al203::powder::batch_24",
        "fresh powdah"
    ]
}
```
