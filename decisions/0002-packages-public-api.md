# Define public APIs in NPM packages

## Context and Problem Statement

We currently use direct reference of files across different packages. This
essentially means that each file is part of the public API which makes it
difficult to define internal APIs. Which leads to everything being part
of the public API, which in turn leads to a high amount of friction for each
change.

## Considered Options

* **Direct references** - We can decide to continue as is. Without a public
API.
* **Define a public API using index.ts** - We add `index.ts` to each folder that defines the public API. The other packages then imports the root index file and are forbidden from direct references to internal files.

## Decision Outcome

Chosen option: **Define a public API using index.ts**.

### Positive Consequences <!-- optional -->

* The public API is defined.
* Imports can be keept cleaner since not every file needs to be manually imported.

### Negative Consequences <!-- optional -->

* We will have to update `index.ts` files whenever we add a new public api.
