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

Use the **Angular coding style guide**.

### Positive Consequences <!-- optional -->

- Since most of our code is written in Angular, we should use the Angular coding style guide.
- Not using camelCase will avoid issues with case-sensitive file systems.

### Negative Consequences <!-- optional -->

- We need to update a lot of files to be consistent.
