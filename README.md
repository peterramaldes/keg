# Specification for Federated Knowledge Bases

*Join us Fridays on <https://rwxrob.live> as we hash out this
specification, or just join us anytime in IRC at (#afkworks)*

## Reserved Environment Variables

### `KNPATH`

The optional `KNPATH` environment variable contains one or more
colon-delimited full paths to all knowledge bases for the current user
account. 

The first knowledge base in the `KNPATH` must be considered the default
knowledge base for the current user (entity).

## Knowledge Bases

A Knowledge Base *is* the root level knowledge node which optionally
contains one or more other nodes recursively (like a typical directory
tree structure).

### Files

A Knowledge Base contains the following files within its root node
directory:

Name|Summary|Required
-|-|-
`README.md`|root node and cover|required
`MANIFEST`|node refs with seconds by last changed|required
json - searchable summary in JSON
subs - subscription recommendations and warnings
words - every single word, count, and node containing (JSON)
<node>/ - node subdirectories or other subdirectories
dex/ - reserved (optional) base index node
dex/json - rendered base index data



## Knowledge Nodes

### Files

Name|Summary|Required
-|-|-
`README.md`|simplified Pandoc Markdown|required
`data.yml`|simplified YAML structured data|optional but conventional
`generate`|static content generator script|optional

Knowledge nodes can contain any files but consideration should be given
to the fact that others will be replicating them as a part of federated
knowledge base sharing. Therefore, large files should be avoided at all
costs and emphasis should be on textual content, knowledge source.

The file types will usually be:

* Simplified Pandoc Markdown
* Simplified YAML Structured Data
* Compressed Raster Image Formats
* Scalable Vector Images
* Generator Scripts 

#### `README.md`

The `README.md` file contains free-form [Simplified Pandoc Markdown].

#### YAML Files

A single knowledge node may contain one or more YAML files. Data from
all YAML files must be in [Simplified YAML Format][]. Use of YAML files
is designed to allow content maintainers the greatest flexibility
without losing specificity in order to organize files in the most
convenient manner to be maintained "by hand."

Files may and should contain extensive use of visual whitespace and
comments with the expectation and promise that these files will *only*
be changed through the use of text editors used by humans. (JSON is used
for everything else.)

##### Default Data Structure 

All YAML files that do *not* begin with three dashes are considered in
the same single data structure as if the files had been concatenated
together in no particular order before being parsed and loaded into
memory. The default data structure must be named `Data` and is reserved.

##### Separate Data Structures

All YAML files beginning with three dashes (`---`) must be considered
separate data structures with names that exactly match the portion of
the file name up to, but not including, the first period (`.`). For
example, a file named `2021-01-21_05:18:51.yml` containing a `Recorded`
field would be as if the following YAML file were parsed:

```yaml
---
2021-01-21_05-18-51: 
  Recorded: 2021-01-21T05:18:51-0500
```

Separate data structure files do not need to be in [Simplified YAML Format][]
and can contain whatever is allowed by YAML version 1.2. This allows
them to specify a schema to which they comply if desired.

##### Special `schema.yml` Root Node File

A special `schema.yml` file (a reserved name) may be placed in the root
node to contain one or more YAML model schema specifications if found.
This allows node creators to specify their own domain models in addition
to those already put forth by the standard. To use the schemas therein,
simply use the triple-dash form of YAML in each node and specify the
schema being used per the YAML standard.

The default 'Data' structure (no dashes) does not comply to any specific
schema other than simplified YAML itself.

##### `data.yml`

Conventional name for main data in [Simplified YAML Format].

#### `schema.yml`

Optional schema specification of the `data.yml` using full YAML schema
notation.

Many standard schemas are expected to be a standard part of
the initial specification including the following:

* Schedule

#### `generate`

By convention an optional `generate` script may be placed in each
knowledge node. It's purpose is to generate static content from dynamic
sources.

While the `generate` script must never be executed by anyone but the
node creator (being considered nothing more than a text file to anything
else) it does provide localized insight into the origin and rendering of
all data within the node. For this reason it should always be included
when possible.

There are several practical uses for the `generate` script:

* Generate portions `README.md` file from the contained YAML data
* Generate YAML data from external data sources
* Produce CSV and other views of data queries

## Simplified Pandoc Markdown

Simplified Pandoc Markdown is a subset of full Pandoc Markdown and a
superset of [GitHub Flavored
Markdown](https://duck.com/lite?kd=-1&kp=-1&q=GitHub+Flavored+Markdown).
Its primary purpose is to remove redundancies while providing full
support for mathematical notation. As a result, Simplified Pandoc
Markdown is the easiest to learn and only requires one pass to fully
parse and render. 

### Strongly Recommended But Not Required

Use of Simplified Pandoc Markdown is not a specification requirement,
but a strong suggestion. It is expected that most tools will only
support it, however.

## Simplified YAML Format

YAML is known to be insecure and complicated in its full form but is
commonly used only for simpler data representations. Simplified YAML
requires several specific constraints to address this:

* No `---` or `...`
* No linking

Keep in mind that the sometimes problematic `yes` and `no` Boolean
values are still a part of Simplified YAML.

## Reserved Command Line Utility Names

* `keg` - utility for sharing on KEG
* `kn` - utility for managing local knowledge base

## Formerly Known As

Here are some fun and official naming considerations over the years:

* Universal Knowledge Transfer Service (UKNTS)
* Knowledge Universal Transfer Service (KNUTS)
* Federated Universal Knowledge Network (FUKN)
* Universal Federated Knowledge Database (FUKD)
* The Knowledge Net
* README World Exchange
* The Essential Web
