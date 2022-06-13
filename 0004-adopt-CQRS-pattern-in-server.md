# Adopt CQRS pattern in Server

## Context and Problem Statement

We use an entity pattern to represent data stored in our databases and bind these entity classes automatically using either Dapper or Entity Framework.

We use constructor-based Dependency Injection to deliver dependencies to objects.

The two above facts mean that our Entities cannot act without receiving all necessary state as method parameters, which goes against our typical DI pattern. 

As a current solution, we use an <<Entity>>Service pattern to act on our entities. These classes end up being dumping grounds for all actions involving the entity. Which results in many [bloaters|https://refactoring.guru/refactoring/smells/bloaters] and [couplers|https://refactoring.guru/refactoring/smells/couplers]

## Considered Options

* **<<Entity>>Services** - Discussed above
* **Commands** - Fundamentally our problem is that the <<Entity>>Service name encapsulates absolutely anything you can do with that entity and excludes any code reuse across different entities. The command pattern creates command classes based on the action being taken on the entity. This naturally limits the classes scope and allows for reuse should two entities need to implement the same command behavior. https://docs.microsoft.com/en-us/azure/architecture/patterns/cqrs

## Decision Outcome

Chosen option: **Commands** 

Commands seem all-around the better decision to the incumbent. We gain code reuse and limit class scope. In addition, we have an iterative path to a full-blown CQRS pipeline, with queued work.

### Positive Consequences <!-- optional -->


### Negative Consequences <!-- optional -->

