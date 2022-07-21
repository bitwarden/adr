# Bitwarden Architectural Decision Records

An [Architectural Decision (AD)][ad] is a software design choice that addresses a functional or
non-functional requirement that is architecturally significant. This might, for instance, be a
technology choice (e.g., Java vs. JavaScript), a choice of the IDE (e.g., IntelliJ vs. Eclipse IDE),
a choice between a library (e.g., [SLF4J][slf4j] vs [java.util.logging][java.util.logging]), or a
decision on features (e.g., infinite undo vs. limited undo).

## @Bitwarden

The goal of introducing Architectural Decisions (AD) in the engineering team at Bitwarden is to
steer development towards a maintainable and expandable code base. While at the same time striving
for uniformity across all pods.

The AD will also serve as a base for proposing and planning technical debt.

## ADRs

The table contains all current ADRs, and includes tags to simplify locating topics. Please ensure
each ADR contains at least which projects they apply to (_clients_, _mobile_ and/or _server_).

| ID  | Title                                                                                        | Status      | Tags    |
| --- | -------------------------------------------------------------------------------------------- | ----------- | ------- |
| 1   | [Use Angular Reactive Forms](./decisions/0001-reactive-forms.md)                             | In progress | clients |
| 2   | [Define public module in NPM packages](./decisions/0002-public-module-npm-packages.md)       | In progress | clients |
| 3   | [Adopt Observable Data Services for Angular](./decisions/0003-observable-data-services.md)   | In progress | clients |
| 4   | [Refactor State Service](./decisions/0004-refactor-state-service.md)                         | In progress | clients |
| 5   | [Refactor Api Service](./decisions/0005-refactor-api-service.md)                             | In progress | clients |
| 6   | [Use Jest Mocks](./decisions/0006-clients-use-jest-mocks.md)                                 | In progress | clients |
| 7   | [Manifest v3 Browser memory caching](./decisions/0007-manifest-v3-browser-memory-caching.md) | In progress | clients |
| 8   | [Adopt CQRS pattern in server](./decisions/0008-adopt-CQRS-pattern-in-server.md)             | In progress | server  |

## Process

While the process was originally created primarily for leads to discuss architectural decisions,
it's important to us to keep the process open to suggestions from anyone. To that end, anyone is
free to open a PR _suggesting_ an AD. The suggestions will then be discussed to reach a general
consensus between the leads before being adopted.

## Pre-commit

This repository uses pre-commit to enforce style guidelines. Please set up pre-commit prior to
contributing.

- [Install](https://pre-commit.com/#install) pre-commit using your favorite package manager
  - Don't forget to run `pre-commit install` after installing pre-commit to set up the git hooks

[ad]: https://en.wikipedia.org/wiki/Architectural_decision
[slf4j]: https://www.slf4j.org/
[java.util.logging]:
  https://docs.oracle.com/javase/8/docs/api/java/util/logging/package-summary.html
