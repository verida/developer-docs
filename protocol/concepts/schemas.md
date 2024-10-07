# Schemas

### Data Schemas

Whenever data records are saved in Verida databases and shared across applications, it is essential to ensure that the data adheres to a specific format and structure. This is usually achieved through schemas. A schema describes the structure of a piece of data and can be used to validate data exchanged between different parties. Some commonly used schemas include JSON (JSON Schema), XML (XSD — XML Schema Definition), GraphQL (GraphQL Schema), DDL (SQL Databases), embedded protocols buffers (Protobuf), and more.

This enables:

* Data of a particular type created in one application can be used in another application (ie: A user’s Contact list can be shared across multiple communication apps)
* Data can be validated before storing in the user’s database
* Data can be queried in a consistent way
* Data can be grouped together by type, enabling permissions to be shared between apps

The Verida [Client SDK](../client-sdk/) uses valid [JSON Schema](https://json-schema.org/) files that specify the structure and validation constraints of your data.

At the very least, each schema defines the following elements:

* Schema name
* List of valid fields
* List of required fields
* Default database name
* Default database indexes

{% hint style="success" %}
When you enter data in a Verida database, it is validated against the defined schema before every save.
{% endhint %}

Well-defined schemas are a key enabler of interoperability when building decentralized apps since they enable seamless data sharing between individual instances of your application, microservices, or even external applications.

The Verida Client SDK provides a set of base / shared schemas for interoperability between applications.

Application developers can submit push requests to contribute to the base schemas or develop their own schemas for their business or consortium of organizations.

## Types of schemas <a href="#types-of-schemas" id="types-of-schemas"></a>

The following are the main types of schemas used by applications building on the Verida network.

### Core schemas <a href="#core-schemas" id="core-schemas"></a>

Verida defines core schemas that are used in the underlying Verida Protocol. This includes some base schemas that all other schemas should include, schemas used for inbox messaging and metadata about databases for each Verida account.

You can find these schemas in the [Verida Core Schemas repo](https://github.com/verida/schemas-core).

### Common schemas <a href="#common-schemas" id="common-schemas"></a>

Verida defines some common schemas that can be used by applications on the Verida network for storing personal data. As a developer building on Verida, you can leverage the core schemas for a wide range of scenarios including credential management, shopping, health, employment data, social contacts, location tracking, and more.

You can find these schemas in the [Verida Common Schemas repo](https://github.com/verida/schemas-common).

{% hint style="info" %}
We actively encourage developers to [create an issue](https://github.com/verida/schemas-common/issues/new) if you would like to propose additional common schemas that can be used by many applications.
{% endhint %}

Verida is responsible for maintaining the `core` and `common schemas`, merging appropriate PR's from third parties and handling any data migration between schema versions with users running the Verida Wallet.

### Application schemas <a href="#application-schemas" id="application-schemas"></a>

Applications are free to create their own schemas for use within their application. These schemas should be valid JSON schema files that are hosted by your application.

Application developers will be responsible for any data migration required caused by upgrades from one schema version to another schema version. In the future, tools may be developed by Verida or the community to help simplify or standardize this data migration process.

### Industry schemas <a href="#industry-schemas" id="industry-schemas"></a>

Anyone can create a schema and use it, including industry bodies. In the future, it’s anticipated that pro-active industry associations will help shape data standards for use across many industries. This will help individuals retain data portability.

## The Verida Base Schema <a href="#the-verida-base-schema" id="the-verida-base-schema"></a>

All schemas inherit from a [base schema](https://core.schemas.verida.io/base/latest/schema.json) which provides a consistent set of common fields across all databases in the Verida network.

Here is the structure of the base Verida Schema and all properties included.

```json
{
    "$schema": "https://json-schema.org/draft/2020-12/schema",
    "$id": "https://core.schemas.verida.io/base/v0.1.0/schema.json",
    "title": "Base schema",
    "description": "A base schema",
    "type": "object",
    "database": {
        "indexes": {
            "schema": ["schema"]
        }
    },
    "appearance": {
        "style": {
            "color": "#f2f2f2",
            "icon": "./icon.svg"
        },
        "nameField": "name",
        "summaryField": "summary"
    },
    "properties": {
        "name": {
            "title": "Name",
            "description": "Name of the item within card list and details",
            "type": "string"
        },
        "summary": {
            "title": "Summary",
            "description": "Brief summary of item",
            "type": "string",
            "maxLength": 100
        },
        "archived": {
            "title": "Archived",
            "description": "Is this record archived?",
            "type": "boolean"
        },
        "schema": {
            "title": "Schema",
            "description": "URI of the schema for this record",
            "type": "string"
        },
        "signatures": {
            "title": "Signatures",
            "description": "Signatures verifying this data has been signed",
            "type": "object"
        },
        "sourceApplication": {
            "title": "Source application",
            "description": "Name of the application this data was sourced from",
            "type": "string"
        },
        "icon": {
            "title": "Icon",
            "description": "URL of an icon for this record. Must be square, prefer SVG, otherwise minimum 50p x 50p",
            "type": "string"
        },
        "insertedAt": {
            "title": "Inserted",
            "description": "Date/time this record was inserted",
            "type": "string",
            "format": "date-time"
        },
        "modifiedAt": {
            "title": "Modified",
            "description": "Date/time this record was modified",
            "type": "string",
            "format": "date-time"
        }
    },
    "required": [
        "schema"
    ]
}
```

### JSON Schema Inheritance <a href="#json-schema-inheritance" id="json-schema-inheritance"></a>

In your development scenarios, you may have some schemas that share common properties. Instead of describing properties for each of these schemas repeatedly, you can import external schema definitions.

In JSON, we use the `allOf` to combine schema operations, where schema B includes all the attributes of schema A, then defines additional properties specific to itself. Notice the use of `allOf` in the example `social/contact` schema below:

```json
{
    "$schema": "https://json-schema.org/draft/2020-12/schema",
    "$id": "https://common.schemas.verida.io/social/contact/v0.1.0/schema.json",
    "title": "Contact",
    "titlePlural": "Contacts",
    "description": "A record of a contact",
    "type": "object",
    "appearance": {
      "style": {
        "color": "#FD4F64",
        "icon": "./icon.svg"
      }
    },
    "database": {
      "name": "social_contact",
      "indexes": {
        "email": ["email"],
        "did": ["did"],
        "name": ["lastName", "firstName"]
      }
    },
    "layouts": {
      "create": [
        "firstName",
        "lastName",
        "email",
        "mobile",
        "did"
      ],
      "view": [
        "firstName",
        "lastName",
        "email",
        "mobile",
        "did"
      ]
    },
    "allOf": [
            {"$ref": "https://core.schemas.verida.io/base/v0.1.0/schema.json"}
    ],
    "properties": {
            "firstName": {
                "title": "First Name",
                "type": "string"
            },
            "lastName": {
                "title": "Last Name",
                "type": "string"
            },
            "email": {
                "title": "Email",
                "type": "string",
                "format": "email"
            },
                "mobile": {
                "title": "Mobile",
                "type": "string"
            },
            "did": {
            "title": "DID",
            "type": "string"
            }
        },
        "required": ["firstName", "lastName"]
  }
```

All custom schemas in Verida must include the base schema consistency across all schemas on the network.

## Creating Schemas <a href="#creating-schemas" id="creating-schemas"></a>

You can create your own schemas in a `schema.json` file and publish it to your website.

Here’s an example custom schema for a note:

```json
{
    "$schema": "https://json-schema.org/draft/2020-12/schema",
    "$id": "https://<your host>/general/note/<version>schema.json",
    "title": "Note",
    "description": "A simple text note",
    "type": "object",
    "database": {
        "name": "general_note",
        "indexes": {
            "insertedAt": ["insertedAt"]
        }
    },
    "allOf": [
        {"$ref": "https://<your host>/core/base/schema.json"},
        {
            "properties": {
                "title": {
                    "title": "Title",
                    "description": "Title for the note",
                    "type": "string"
                },
                "body": {
                    "title": "Body",
                    "description": "Main body content for the note",
                    "type": "string"
                },
                "tags": {
                    "title": "Tags",
                    "type": "array",
                    "items": {
                        "type": "string"
                    }
                }
            },
            "required": ["title", "body"]
        }
    ]
  }
```

You can then host your schema at `https://<your host>/general/note/<version>/schema.json` and open a datastore using your schema with:

```typescript
const datastore = app.openDatastore('https://<your host>/general/note/<version>schema.json')
```

## Retrieving Schema Objects <a href="#retrieving-schema-objects" id="retrieving-schema-objects"></a>

**Get Schema JSON :** You can retrieve your hosted schema that you have used to open a datastore in json format as shown in the example code below:

```typescript
const schemas = await app.getClient().getSchema('https://<your host>/general/note/schema.json');
const json = await schemas.getSchemaJson()
```

**Get Schema Path:** This returns the schema url.

```typescript
const schemaUrl = await schemas.getPath()
```

**Get Schema Appearance :** This returns the styles Object that contains schema custom color and icon.

```typescript
const styles = await schemas.getAppearance()
```

## Schema Validation <a href="#schema-validation" id="schema-validation"></a>

Data is validated against the schema before every save. you can also call the validate function to validate the schema data and also log validation errors. Below is an example code sample for validating a schema.

```typescript
const schemas = await app.getClient().getSchema('https://<your host>/general/note/schema.json')
const isValid = await schemas.validate(data) // returns a boolean
const validationErrors = schemas.errors
```

You can access `schema.errors` in the above example which is an array that contains the list of validation errors.

## Schema Versioning <a href="#schema-versioning" id="schema-versioning"></a>

Schema versioning in a decentralized environment is a challenging problem. We have chosen to provide a convention for scheming which may be formalized in the future. There is some prior art of versioning software and schemas that is useful. There is some [discussion here](https://github.com/verida/verida-js/issues/18) for those interested in diving deeper.

Schemas are versioned with a logical structure in the following format:

* `https://example.xyz/<schema name>/latest/schema.json` -- The latest, most up-to-date version of a schema
* `https://example.xyz/<schema name>/v1.0.1/schema.json` -- The latest, most up-to-date version of a schema is also stored as a versioned file
* `https://example.xyz/<schema name>/v1.0.0/schema.json` -- An archive of a previous schema version
* Schemas have an `$id` property that represents the full versioned URI of the schema (ie: `https://example.xyz/<schema name>/v1.0.1/schema.json`)

**Minor version upgrades**

Our key priority is to ensure that **minor schema upgrades are backwards compatible**. The protocol needs to ensure this is possible without needing to update the version of all schemas across the network or within a particular application context.

For example; It should be possible to add a new field to `base` schema that doesn't break previous `base` schemas and new applications can start using that field without defining any new schemas of their own.

This will be possible with a future upgrade of the Client SDK.

## Migration <a href="#migration" id="migration"></a>

Initially, it will be up to each application to detect data that is an old version and manually run code to update it to the latest version.

In the future, Verida aims to provide tools in the Client SDK to simplify this upgrade process, enforce data upgrades and potentially leverage the Trust Framework to distribute trusted data migration logic.
