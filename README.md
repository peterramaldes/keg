# KEG, The Knowledge Exchange Grid (Graph)

*Join us Fridays on <https://rwxrob.live> as we hash out this
specification, or just join us anytime in IRC at (#afkworks)*

## Reserved Environment Variables

Knowledge workers can quickly change roles/contexts by changing a single
environment variable enabling them to work productively with different
local knowledge nodes throughout the day. This flexibility allows work
to be done from a single computer account as well as facilitates
searches and other work that might apply to all local nodes.

### `KN`

The `KN` environment variable contains the full path to the current
local root knowledge node. This allows context to be changed for the
entire session or just for a single command.

```bash
# start a new bash shell with given KEG context 
KN=$REPOS/github.com/rwxrob/rwxrob exec bash

# just run a log script for the given context
KEG=$REPOS/github.com/skilstak/keg log
```

Scripts and the `keg` and/or `kn` helper utilities should be aware of
the `KEG` (and optionally `KEGPATH`) variables. Anything else related to
those KEGs should be loaded from `keg.yml` within each.

This approach borrows heavily from the successful model used by `sudo`.

### `KEGPATH`

The optional `KEGPATH` environment variable contains one or more
colon-delimited full paths to all local knowledge nodes for the current
user account. 

## Knowledge Bases

A Knowledge Base *is* the root level knowledge node which optionally
contains one or more other nodes recursively (like a typical directory
tree structure).

### Files

A Knowledge Base contains the following files within its root node
directory:

Name|Summary|Required
-|-|-
`keg.yml`|master key YAML file|required
`README.md`|root node and cover|required
`MANIFEST`|node refs with seconds by last changed|required

<!-- TODO need to get better alternatives to these
json - searchable summary in JSON
subs - subscription recommendations and warnings
words - every single word, count, and node containing (JSON)
<node>/ - node subdirectories or other subdirectories
dex/ - reserved (optional) base index node
dex/json - rendered base index data
-->

## Knowledge Nodes

### Files

Name|Summary|Required
-|-|-
`README.md`|simplified Pandoc Markdown|required
`data.yml`|simplified YAML structured data|optional but conventional
`*.yml`|more YAML data|optional
`generate`|static content generator script|optional

Knowledge nodes can contain any files but consideration should be given
to the fact that others will be replicating them if shared. Therefore,
large files should be avoided at all costs and emphasis should be on
textual content, knowledge source.

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

Conventional name for main data in [Simplified YAML Format][].

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

## Universal Resource Identifiers

Since any network protocol --- or no protocol --- can be used to access
any KEG node the only unique identifier required is a domain name
(including subdomains) and the dotted Node identifier. It is expected
(but not required) that the KEG creator and/or maintainer will have
administrative access to the domain in order to add the required TXT
record. If not, the KEG node will remain *unvalidated* but will still be
usable.

If an optional `jq` query is supplied the resulting raw JSON will be
implied in the return. By convention all KEG Node data keys are always
initial capitalized. This allows them to be distinguished from the node
path itself, which always has initial lowercase.

```pegn
KEGURI <-- 'keg:' Node Query? ('@' Domain)?
Node   <-- (!'.' visible)+ ('.' (!'.' visible)+)*
Query     <-- # jq limited query
```

Examples:

```
keg:md.lang@rwx.gg
keg:md.lang.Summary@rwx.gg
```

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
* Add text assumed to be [Simplified Pandoc Markdown][] (therefore,
  the `|` operator is preferred for long text blocks over `>`).

Keep in mind that the sometimes problematic `yes` and `no` Boolean
values are still a part of Simplified YAML.

## Reserved Command Line Utility Names

Reserved Name|Usage
|:-:|-
`keg`|An knowledge package manager and sharing utility build and managed under the KEG initiative.
`kn`|A reserved name for a local knowledge management utility command or script to be created and used however the knowledge worker wants on their local system. It's expected that hundreds or millions of different `kn` tools will exist depending on the specific needs of the worker. A base Bash `kn` script will be managed under the KEG initiative, but it by no means required.

```
# query a node on the KEG
keg tap best.beers  
keg tap best.beers@rwx.gg
eval $(keg _bash)
eval $(kn _bash)
complete -C kn kn
complete -C keg keg
```

## Formerly Known As

Here are some fun and official naming considerations over the years:

* Universal Knowledge Transfer Service (UKNTS)
* Knowledge Universal Transfer Service (KNUTS)
* Federated Universal Knowledge Network (FUKN)
* Universal Federated Knowledge Database (FUKD)
* The Knowledge Net
* README World Exchange
* The Essential Web

## FAQ

### What is the difference between "keg" and "kn"?

The term "keg" always refers to the sharing network on which a "kn"
(knowledge node) is shared. 

### What is the difference between a "KEG node" and a "knowledge node"?

A KEG node is a knowledge node that is shared on the KEG. Knowledge
nodes can exist locally without ever being shared. A KEG node must be
shared to be called such.

### Why not "knowledge base"?

Because the term is inaccurate. The word "base" implied it can't be
further composed or otherwise combined. Therefore, the best and most
accurate term is "knowledge node" even if a single node is a root node
shared over KEG.
