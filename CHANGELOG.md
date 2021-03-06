# Changelog JSON Schema Tools


2014-10

* handle circular dependencies in $ref arguments, so schema.to_h always resolves $refs properly 

2014-09

* add Schema class to represent a single schema, replaces simple Hash usage. Potentially breaks usage if you've gone crazy with Hash methods in your client.

2014-09

* refactor Reader to de-reference all pointers when initialized
* remove :exclude_root option for object to schema_hash BREAKING change if you want nesting define your schema accordingly
* add :links option for object to schema_hash, to include the links inline in an object
* support items definition for array type properties - BREAKING you must change old simple property definitions
* support oneOf definitions

2014-08

* add $ref resolver to merge property definitions from another file

2013-10
* allow all object properties in link href placeholders => contacts/{id}/{number}
* add base_url option to schema hash creation. Prepends an url to all links of a rendered object
* add to_schema_json for simpler model to json conversion
* add option to exclude_root in to_schema hash method

2013-06

* *has_schema_attr :schema=>{schema as ruby hash}* allow pass a schema as hash
* *SchemaTools::Reader.read('name', {Schema Hash}|'String Path'  )*  init a schema from hash additionally to passing a string as schema path
* rails 4 (ActiveModel) compatibility
* Testing with different ActiveModel Versions

2013-02

* add validations for classes generated by KlassFactory

2012-12

* initial version with reader, hasher, params cleaner, attributes module