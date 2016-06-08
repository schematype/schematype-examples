SchemaType
==========

A Schema and Type data definition language for YAML and JSON

## Synopsis

```yaml
-name: example8-1
-desc: Schema for https://www.ietf.org/rfc/rfc4627.txt (#8.1)
-spec: schematype.org/v0.0.1
-import:
  +%: github:schematype/schematype/%/#v0.0.1

Image:
  Title: +str/phrase 1..100
  Height: +num/int/pos
  Width: +num/int/pos
  Thumbnail?:
    Url: +net/url
    Height: +num/int/pos
    Width: +num/int/pos
  IDs?: +num/int/pos[*]
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

## Syntax Overview

SchemaType aims to be concise and easy to read/write/maintain. SchemaType
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

## Compact Notation

SchemaType DSL has 2 styles. Here are snippets of both:

**Compact:**

```
website: +net/url /example.com/ --The website URL
sanity: +num/int -10..10 --The sanity level
```

**Verbose**:

```
website:
  -type: +net/url
  -match: /example.com/
  -desc: The website URL
sanity:
  -type: +num/int
  -range: [-10, 10]
  -desc: The sanity level
```

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

## SchemaType vs JSON-Schema

* The schematype definitions are *exact* as they import exact versioned used types
* Things are defined at the same level as the instances
with JSON schema the nesting to things of interest makes it unweildy
* Constraints of types are always defined in proximity to the defintions

  for example: https://github.com/schematype/schematype-examples/blob/master/package-json.json-schema.yaml#L351 defines what keys are required thats line 351, and they keys are defined at line 68

* people using json-schema will define, say, a url field to be type 'string'

  which is ludicrous
  that makes the full text of War and Peace, a valid URL

* JSON-Schema defines maybe a dozen string types, defines them in the spec

  schematype defines only the most core types: any, str, num, float, int, bool
  but they are never used in user schemas
  they are the bases of things like url
  defining your own types is strongly encouraged
  schematype will define a std lib of types
  but they are not special. not specced
  and you can fork any of them
  if you find a core type bug, you fork it, use it, maybe PR it
  and then switch back if it goes upstream

* JSON-Schema is not-strict by default. SchemaType must account for everything.
