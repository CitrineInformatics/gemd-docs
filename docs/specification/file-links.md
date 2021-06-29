# File Links

Data objects are commonly derived from or related to files, such as
the excel sheet from which a property was ingested,
or an SEM image associated with a measurement,
or a PDF describing detailed information for how to execute a process.

Objects can reference those files using `FileLink`s, 
which simply specify a filename and URL.

Field name | Value type | Description
-----------|------------|------------
`filename` | String     | The name of the file
`url`      | String     | The URL at which the file (and associated metadata) can be accessed

##### Constraints

None

##### Example

```javascript
{
    "file_links" : [
        {
            "filename" : "How-to-make-lucky-charms.pdf",
            "url" : "https://example.com/files/file/d8f12919-b201-4186-be95-10525eb4256a/version/2"
        }
    ]
}
```

