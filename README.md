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

