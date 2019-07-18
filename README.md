# Patch JSON Document Format Guide

This is a guide documenting the structure of the patch reporting JSON format for consumption by FoxGuard Solutions.  It details the types and potential values of all the properties, objects, and arrays that could be in the JSON document.  We recommend reviewing this document alongside a copy of the example `patches.json` document to better understand the document structure.

## Metadata

The following elements are metadata in the patch reporting JSON.  They help to identify the document for validation.

### `$id`

| Data Type | string |
| --- | --- |
| Minimum Occurrences | 1 (required) |
| Maximum Occurrences | 1 |
| Parent | Root |

FoxGuard Solutions provides this value.  From the [JSON Schema Draft 7 documentation section 8.2](https://json-schema.org/latest/json-schema-core.html#id-keyword):

The `$id` keyword defines a URI for the schema, and the base URI that other URI references within the schema are resolved against. A subschema&#39;s &quot;$id&quot; is resolved against the base URI of its parent schema. If no parent sets an explicit base with &quot;$id&quot;, the base URI is that of the entire document, as determined per RFC 3986 section 5 [RFC3986].

If present, the value for this keyword MUST be a string, and MUST represent a valid URI-reference [RFC3986]. This value SHOULD be normalized, and SHOULD NOT be an empty fragment \&lt;#\&gt; or an empty string \&lt;\&gt;.

### `$schema`

| Data Type | string |
| --- | --- |
| Minimum Occurrences | 1 (required) |
| Maximum Occurrences | 1 |
| Parent | Root |

FoxGuard Solutions provides this value.  From the [JSON Schema Draft 7 documentation section 7](https://json-schema.org/latest/json-schema-core.html#rfc.section.7):

The `$schema` keyword is both used as a JSON Schema version identifier and the location of a resource which is itself a JSON Schema, which describes any schema written for this particular version.

The value of this keyword MUST be a URI [RFC3986] (containing a scheme) and this URI MUST be normalized. The current schema MUST be valid against the meta-schema identified by this URI.

## Core Elements

### `documentDetails`

| Data Type | objects |
| --- | --- |
| Minimum Occurrences | 1 (required) |
| Maximum Occurrences | 1 |
| Parent | Root |
| Children | `publisher`, `generated` |

The `documentDetails` objects contains attributes specific for details about the origination of the JSON document.

_Tip: An &quot;object&quot; is a collection of keys and values._

### `publisher`

| Data Type | objects |
| --- | --- |
| Minimum Occurrences | 1 (required) |
| Maximum Occurrences | 1 |
| Parent | `documentDetails` |
| Children | `publisherUrl` |

The `publisher` objects contains attributes specific for details about who published the document.

### `publisherUrl`

| Data Type | string |
| --- | --- |
| Minimum Occurrences | 1 (required) |
| Maximum Occurrences | 1 |
| Parent | `publisher` |

The `publisherUrl` is a string value that points to the URL of the publisher of the document.

_Tip: In most cases that publisher is the vendor whose patches the document describes._

### `generated`

| Data Type | string |
| --- | --- |
| Format | YYYY-MM-DD |
| Minimum Occurrences | 1 (required) |
| Maximum Occurrences | 1 |
| Parent | `documentDetails` |

The date in YYYY-MM-DD format on which the JSON document was created.

### `vendor`

| Data Type | objects |
| --- | --- |
| Minimum Occurrences | 1 (required) |
| Maximum Occurrences | 1 |
| Parent | Root |
| Children | `name`, `note`, `products` |

The `vendor` objects describes the vendor that created the patches reported in the document.

### `name`

| Data Type | string |
| --- | --- |
| Minimum Occurrences | 1 (required) |
| Maximum Occurrences | 1 |
| Parent | `vendor` |

The name of the vendor publishing the patches that are listed in the JSON document.

### `note`

| Data Type | string |
| --- | --- |
| Minimum Occurrences | 0 (optional) |
| Maximum Occurrences | 1 |
| Parent | `vendor` |

The `note` property is used to provide any necessary additional information about the vendor that could be useful to consumers of the document.

_Tip: This field is optional. You can leave it blank or leave it out entirely._

### `products`

| Data Type | array (of objects) |
| --- | --- |
| Minimum Occurrences | 1 (required) |
| Maximum Occurrences | 1 |
| Maximum Elements | Unlimited |
| Parent | Root |
| Children | Objects containing: `name`, `operatingSystem`, `patchAvailability`, `versions` |

The `products` array contains a collection of objects that list information about individual products upon which the vendor is reporting patch data.

_Tip: The array must exist, but it can contain an unlimited number of objects (each of which represent a product), or it may be empty. These objects lack keys to identify them, so this documentation will refer to each of them as &quot;An element in: products&quot;_

### `name`

| Data Type | string |
| --- | --- |
| Minimum Occurrences | 1 (required) |
| Maximum Occurrences | 1 |
| Parent | An element in: `products` |

The name of the product being described.

### `operatingSystem`

| Data Type | string |
| --- | --- |
| Minimum Occurrences | 0 (optional) |
| Maximum Occurrences | 1 |
| Parent | An element in: `products` |

The `operatingSystem` property specifies the operating system that runs the product to which patches are applied.

### `patchAvailability`

| Data Type | string |
| --- | --- |
| Range | Public, Private |
| Minimum Occurrences | 1 (required) |
| Maximum Occurrences | 1 |
| Parent | An element in: `products` |

The `patchAvailability` property describes the availability of the patches applicable to the parent product.  There are only two supported options.  Patches for the product can be either publicly available, or privately available.

### `versions`

| Data Type | array (of objects) |
| --- | --- |
| Minimum Occurrences | 1 (required) |
| Maximum Occurrences | 1 |
| Maximum Elements | Unlimited |
| Parent | An element in: `products` |
| Children | Objects containing: `name`, `released`, `cpe23`, `patches` |

The `versions` array contains a collection of objects that list information about versions belonging to the parent product.

_Tip: The array must exist, but it can contain an unlimited number of objects (each of which represent a version), or it may be empty. These objects lack keys to identify them, so this documentation will refer to each of them as &quot;An element in: versions&quot;_

### `name`

| Data Type | string |
| --- | --- |
| Minimum Occurrences | 1 (required) |
| Maximum Occurrences | 1 |
| Parent | An element in: `versions` |

The name of the version being described.

### `released`

| Data Type | string |
| --- | --- |
| Format | YYYY-MM-DD |
| Minimum Occurrences | 1 (required) |
| Maximum Occurrences | 1 |
| Parent | An element in: `versions` |

The date in YYYY-MM-DD format that the version was released.

### `cpe23`

| Data Type | array (of strings) |
| --- | --- |
| Format | [CPE 2.3](https://cpe.mitre.org/specification/) |
| Minimum Occurrences | 0 (optional) |
| Maximum Occurrences | 1 |
| Maximum Elements | Unlimited |
| Parent | An element in: `versions` |

The `cpe23` array is used to list the applicable version 2.3 CPEs (Common Platform Enumeration) provided by NVD for the patch.

### `patches`

| Data Type | array (of objects) |
| --- | --- |
| Minimum Occurrences | 1 (required) |
| Maximum Occurrences | 1 |
| Maximum Elements | Unlimited |
| Parent | An element in: `versions` |
| Children | Objects containing: `name`, `severity`, `updateType`, `notes`, `patchVersion`, `released`, `filename`, `checksums`, `cves`, `links` |

The `patches` array contains a collection of objects that list information about patches belonging to the parent version.

_Tip: The array must exist, but it can contain an unlimited number of objects (each of which represent a patch), or it may be empty. These objects lack keys to identify them, so this documentation will refer to each of them as &quot;An element in: patches&quot;_

### `name`

| Data Type | string |
| --- | --- |
| Minimum Occurrences | 1 (required) |
| Maximum Occurrences | 1 |
| Parent | An element in: `patches` |

The name of the patch being described.

### `severity`

| Data Type | string |
| --- | --- |
| Range | `Unknown`, `Critical`, `Important`, `Optional` |
| Minimum Occurrences | 0 (optional) |
| Maximum Occurrences | 1 |
| Parent | An element in: `patches` |

The `severity` property indicates the criticality level of the patch.  It can be omitted if the information is not known.  The value `Optional` should not be used to indicate that the value is unknown, but rather that the application of the patch is optional for patching a product.

### `updateType`

| Data Type | string |
| --- | --- |
| Range | `Security`, `Non-Security`, `Potentially Security-Related` |
| Minimum Occurrences | 0 (optional) |
| Maximum Occurrences | 1 |
| Parent | An element in: `patches` |

The `updateType` property describes the patches security implications.

- `Security` means that applying the patch will fix a security vulnerability.
- `Non-Security` means that applying the patch will not fix a security vulnerability but is another type of update like a bug-fix.
- `Potentially Security-Related` is used to indicate that the security-applicability of a patch is not known or fully understood.

### `notes`

| Data Type | array (of objects) |
| --- | --- |
| Minimum Occurrences | 0 (optional) |
| Maximum Occurrences | 1 |
| Maximum Elements | Unlimited |
| Parent | An element in: `patches` |
| Children | Objects containing: `type` and `content` |

The `notes` array contains a collection of objects that list textual information pertinent to the patch.

_Tip: If the array exists it can contain an unlimited number of objects (each of which represent a note), or it may be empty. These objects lack keys to identify them, so this documentation will refer to each of them as &quot;An element in: notes&quot;_

### `type`

| Data Type | string |
| --- | --- |
| Range | `Comment`, `Description`, `Security Summary` |
| Minimum Occurrences | 1 (required) |
| Maximum Occurrences | 1 |
| Parent | An element in: `notes` |

The `type` property describes the kind of note recorded in the object.

### `content`

| Data Type | string |
| --- | --- |
| Minimum Occurrences | 1 (required) |
| Maximum Occurrences | 1 |
| Parent | An element in: `notes` |

The `content` property is freeform text that are the contents of the note.

### `patchVersion`

| Data Type | string |
| --- | --- |
| Minimum Occurrences | 1 (required) |
| Maximum Occurrences | 1 |
| Parent | An element in: `patches` |

The `patchVersion` property indicates the version of the patch that is applicable to the parent version.

### `released`

| Data Type | string |
| --- | --- |
| Format | YYYY-MM-DD |
| Minimum Occurrences | 1 (required) |
| Maximum Occurrences | 1 |
| Parent | An element in: `patches` |

The date in YYYY-MM-DD format upon which the patch was released.

### `fileName`

| Data Type | string |
| --- | --- |
| Minimum Occurrences | 0 (optional) |
| Maximum Occurrences | 1 |
| Parent | An element in: `patches` |

The `fileName` property is the name of the patch file to be installed.

### `checksums`

| Data Type | object |
| --- | --- |
| Minimum Occurrences | 0 (optional) |
| Maximum Occurrences | 1 |
| Parent | An element in: `patches` |
| Children | `md5`, `sha1`, `sha256`, `sha384`, `sha512`, `ripemd160` |

The `checksums` object contains a collection of various types of checksums and their corresponding values.

### `md5`

| Data Type | string |
| --- | --- |
| Minimum Occurrences | 0 (optional) |
| Maximum Occurrences | 1 |
| Parent | `checksums` |

The `md5` property represents the MD5 checksum of the patch file.

### `sha1`

| Data Type | string |
| --- | --- |
| Minimum Occurrences | 0 (optional) |
| Maximum Occurrences | 1 |
| Parent | `checksums` |

The `sha1` property represents the SHA-1 checksum of the patch file.

### `sha256`

| Data Type | string |
| --- | --- |
| Minimum Occurrences | 0 (optional) |
| Maximum Occurrences | 1 |
| Parent | `checksums` |

The `sha256` property represents the SHA-256 checksum of the patch file.

### `sha384`

| Data Type | string |
| --- | --- |
| Minimum Occurrences | 0 (optional) |
| Maximum Occurrences | 1 |
| Parent | `checksums` |

The `sha384` property represents the SHA-384 checksum of the patch file.

### `sha512`

| Data Type | string |
| --- | --- |
| Minimum Occurrences | 0 (optional) |
| Maximum Occurrences | 1 |
| Parent | `checksums` |

The `sha512` property represents the SHA-512 checksum of the patch file.

### `ripemd160`

| Data Type | string |
| --- | --- |
| Minimum Occurrences | 0 (optional) |
| Maximum Occurrences | 1 |
| Parent | `checksums` |

The `ripemd160` property represents the RIPEMD-160 checksum of the patch file.

### `cves`

| Data Type | array (of string) |
| --- | --- |
| Minimum Occurrences | 0 (optional) |
| Maximum Occurrences | Unlimited |
| Parent | An element in: `patches` |

The `cves` array is used to list the applicable CVEs (Common Vulnerabilities and Exposures) provided by NVD for the patch.

### `links`

| Data Type | object |
| --- | --- |
| Minimum Occurrences | 0 (optional) |
| Maximum Occurrences | 1 |
| Parent | An element in: `patches` |
| Children | `securityBulletins`, `releaseNotes`, `related`, `download` |

The `links` object contains a collection of various types of URLs of other pertinent information about a patch such as security bulletins, release notes, or a URL to download a patch.

### `securityBulletins`

| Data Type | array (of string) |
| --- | --- |
| Minimum Occurrences | 0 (optional) |
| Maximum Occurrences | 1 |
| Maximum Elements | Unlimited |
| Parent | `links` |

The `securityBulletins` array lists one or more relevant URLs pointing to security bulletins for a patch.

### `releaseNotes`

| Data Type | array (of string) |
| --- | --- |
| Minimum Occurrences | 0 (optional) |
| Maximum Occurrences | 1 |
| Maximum Elements | Unlimited |
| Parent | `links` |

The `releaseNotes` array lists one or more relevant URLs pointing to release notes for a patch.

### `related`

| Data Type | array (of string) |
| --- | --- |
| Minimum Occurrences | 0 (optional) |
| Maximum Occurrences | 1 |
| Maximum Elements | Unlimited |
| Parent | `links` |

The `related` array lists one or more relevant URLs pointing to any additional information found on the web for a patch.

### `download`

| Data Type | string |
| --- | --- |
| Minimum Occurrences | 0 (optional) |
| Maximum Occurrences | 1 |
| Parent | `links` |

The `download` property is a URL that points to a location on the web where a patch can be downloaded.
