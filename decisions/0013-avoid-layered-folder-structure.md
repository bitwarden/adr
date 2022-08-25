---
title: Avoid layered folder structure
parent: Decisions
nav_order: 13
adr: "0013"
status: In progress
date: 2022-08-23
tags: [clients, angular]
---

# Avoid layered folder structure

## Context and Problem Statement

Our Angular application currently partially uses a layered folder structure. This results in a
domain context being split between multiple folders several levels deep. This causes friction since
modifying services often requires modifying the models belonging to that service.

## Considered Options

- **Continue as is** - We can continue as we are, and continue to put models in
  `libs/common/models`.
- **Place models next to their owner** - In most cases a model is owned by a service. For requests
  and responses this is typically that domains api service. And the models are rarely if ever used
  outside that context. It's therefore logical to also put the models in the service folder. A rule
  of thumb is to put any model that is defined in the abstraction in the abstraction directory. And
  any model defined in the service in the service directory. Abstractions are part of the service
  public interface, while services are part of the internal interface.

### Decision Outcome

Use the **Place models next to their owner**.

### Example

```text
libs/common/
  abstractions/folder/
    folder.service.abstraction.ts
    folder-api.service.abstraction.ts
    responses/
      folder.response.ts  (Exposed as public API)
  services/folder/
    folder.service.ts
    folder-api.service.ts
    requests/
      folder.request.ts  (Internal, only used within the implementation)
```
