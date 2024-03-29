---
title: Angular filename convention
parent: Decisions
nav_order: 12
adr: "0012"
status: In progress
date: 2022-08-23
tags: [clients, angular]
---

# Angular Filename convention

## Context and Problem Statement

We currently use a mixed filename convention where some files follows the Angular styleguide, and
other files use camelCase. This causes some confusion as to which convention to follow, and we
should standardize on one convention to avoid confusion.

## Considered Options

- **Angular coding style guide** - The Angular coding style guide specifies "Separate file names
  with dots and dashes".
- **camelCase** - Our own convention has for a long time been to use camelCase.

### Decision Outcome

Use the [**Angular coding style guide**][naming]. More specifically [Style 02-02][style-02-02] and
[Style 02-03][style-02-03].

These two style rules focuses on using dashes to separate words in the descriptive name, and dots to
separate name from the type. Angular typically has the following types:

- `service` - Abstract Service
- `component` - Angular Components
- `pipe` - Angular Pipe
- `module` - Angular Module
- `directive` - Angular Directive

At Bitwarden we also use a couple of more types:

- `.api` - Api Model
- `.data` - Data Model (used for serializing domain model)
- `.view` - View Model (decrypted domain model)
- `.export` - Export Model
- `.request` - Api Request
- `.response` - Api Response
- `.type` - Enum
- `.service.implementation` - Implementation of an abstract service

The class names are expected to use the suffix as part of their class name as well. I.e. a service
implementation will be named `FolderServiceImplementation`, a request model will be named
`FolderRequest`.

Since abstracts are referenced far more frequently than implementations, they use the simplified
type while implementations must specify they are implementations e.g `.service` for the abstract vs
`.service.implementation` for the implantation.

### Positive Consequences <!-- optional -->

- Since most of our code is written in Angular, we should use the Angular coding style guide.
- Not using camelCase will avoid issues with case-sensitive file systems.

### Negative Consequences <!-- optional -->

- We need to update a lot of files to be consistent.

[naming]: https://angular.io/guide/styleguide#naming
[style-02-02]: https://angular.io/guide/styleguide#style-02-02
[style-02-03]: https://angular.io/guide/styleguide#style-02-03
