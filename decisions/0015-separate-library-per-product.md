---
parent: Decisions
nav_order: 15
adr: "0015"
status: In progress
date: 2022-11-03
tags: [clients]
---

# Separate library per product

## Context and Problem Statement

Our `clients` repository currently stores much common logic inside of the `libs/common` package,
which is a library that is consumed by all of our clients. However not all parts of the package are
actually used by all of our clients which makes our bundles unecessarily large. Another issue with
this arrangement is the increased cognitive load for the different pods that have to filter out the
parts of the package that don't concern their work.

## Considered Options

- **Continue as is** - Place all services, models, views, etc in `libs/common`.
- **Put services that are only used in 1 client inside that client** - This has the benefit of not
  bloating the common package but will start "polluting" our frontends with request/response models
  that we're trying to hide behind services. Cognitive load is also not particularly improved.
- **Create new libs for each product** - This involves splitting our common code into `common`,
  `password-manager`, `admin-console` and `secrets-manager` library packages. It has all the benfits
  of the `common` package such as being able to keep internal request/response models while avoiding
  bloat and lowering cognitive load. It also continues to encorage separation of UI and domain
  knowledge.

## Decision Outcome

Chosen option: **The enumerated name of the option**

<!-- optional: brief reason for decision **or** the positive/negative consequences sections below -->

### Positive Consequences <!-- optional -->

### Negative Consequences <!-- optional -->
