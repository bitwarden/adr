# Use Angular Modules

## Context and Problem Statement

> NgModules are containers for a cohesive block of code dedicated to an application domain, a
> workflow, or a closely related set of capabilities. They can contain components, service
> providers, and other code files whose scope is defined by the containing NgModule. They can import
> functionality that is exported from other NgModules, and export selected functionality for use by
> other NgModules.
>
> -- <cite>https://angular.io/guide/architecture-modules</cite>

In ADR [0002 Define public module in NPM packages](./0002-public-module-npm-packages.md) we decided
to start using barrel files and restrict imports from other "modules". This solves some of the pain
points we have encountered, however all components are still exported in the barrel files since they
need to be defined in an Angular Module. This limits the usefulness of the barrel files.

Angular encourages creating many small NgModules, with people advocating for either one Module per
feature, or going so far as defining one module per component. In Angular v14 this was made easier
with the introduction of [standalone components](https://angular.io/guide/standalone-components).

## Considered Options

- **Do nothing** - Maintain the status quo and define most components in a single module. This would
  mean that each component is still essentially global.
- **[Angular Modules](https://angular.io/guide/architecture-modules)** - Add NgModules alongside our
  barrel files. This allows for proper encapsulation of internal components.
- **[Standalone Components](https://angular.io/guide/standalone-components)** - provides the
  benefits of NgModules without most of the additional boilerplate. Is still in preview and is
  therefore risky to rely on.

## Decision Outcome

Chosen option: **Angular Modules**

### Positive Consequences

- Internal components cannot be re-used outside of the feature.
- Modules are required for supporting lazy loading.
- Similar structure as _Standalone Components_ which will allow for easy migration in the future
  should this be deemed necessary.

### Negative Consequences

- Angular error handling for NgModules is awful and provides cryptic errors that are hard to debug.
- Additional boilerplate.
