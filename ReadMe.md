SchemaType
==========

A Schema and Type data definition language for YAML and JSON

## Synopsis

```yaml
-name: example8-1
-desc: Schema for https://www.ietf.org/rfc/rfc4627.txt (#8.1)
-spec: schematype.org/v0.0.1
-import:
  ++: github:schematype/schematype/core/ @v0.0.1
  +net: github:schematype/schematype/net/ @v0.0.1

Image:
  Title: ++phrase 1..100
  Height: ++int-pos
  Width: ++int-pos
  Thumbnail?:
    Url: +net/url
    Height: ++int-pos
    Width: ++int-pos
  IDs?: ++int-pos[*]
```

## Description

SchemaType is a data definition language for data languages that have (or can
be easily viewed in) the YAML/JSON data model. That is, a directed acyclic
graph, or more commonly: hashes, arrays and scalars. This includes:

* YAML
* JSON
* BSON
* CSV and TSV
* Protobuf (maybe)
* RDBMS (maybe)

The primary goals of SchemaType are validation and programatic generation of
structured data, but other use cases may be discovered.

SchemaType aims to be concise and easy to read/write/maintain.  SchemaType
documents are written in YAML with some DSL formatting. Some of the key points
are:

* All schemas (types) defined and imported expand to URLs
  * URLs are encouraged to be under source control
  * Type imports can be pinned to specific commits
  * Anyone can author types
  * Anyone can "fork" or extend existing types
* All references to types start with '+'
* Keys starting with '-' are part of the SchemaType language
* SchemaType is strict by default
  * Anything not defined is invalid
  * Keys ending with '?' are optional (default is required)

## Language Design Goals

...

## Examples

Here are some examples of what SchemaType docs might look like, with JSON
Schema (or plain text) equivalents:

* Simple Geo Coordinate object:
  * [SchemaType](geo-coordinate.schema)
  * [JSON-Schema](geo-coordinate.json-schema)
* CloudFoundry `manifest.yml` files:
  * [SchemaType](manifest.schema)
  * No JSON-Schema. See [documentation](http://docs.pivotal.io/pivotalcf/1-7/devguide/deploy-apps/manifest.html)
  * [Sample mainfest.yml file](manifest.yml)
* NPM `package.json` files:
  * [SchemaType](package-json.schema)
  * [JSON-Schema](package-json.json-schema.yaml) in YAML. (JSON is waaay to verbose)
  * [JSON-Schema](package-json.json-schema) in JSON :-(
