# Adopt Angular Modules For Code Blocking Angular Components

## Issue

Currently code blocks in our Angular clients are defined using Angular Components (e.g
ciphersComponent, vaultComponent). Many of these code blocks have outgrown this level of
abstraction, and would benefit greatly from having access to their own child components, models,
services, pipes, and other structures. Likewise, many of these components share functionality with
other components in scattered areas across the file system, leading to long imports for inheritance
at best and redundant code at worst (e.g the CiphersComponent and OrganizationCiphersComponent).

Fixing this issue is a multi part effort. This ADR will cover one core piece of the solution: how we
should bundle a set of related components and their local dependencies.

## Considered Options

- [Angular Modules](https://angular.io/guide/architecture-modules)
- [Barrel Files](https://dev.to/luispa/how-to-create-barrels-in-typescript-or-javascript-59ma)

## Decision Outcome

Choosen option: Angular Modules

## Pros and Cons

### Ng Modules

#### Pros

- Modules provide all the same benefits as barrel files: clean imports and exports from folder
  structures. I would say Modules even do this better, because an import of one Angular Module can
  be equivelent to an import of n different files exported from a barrel file, but that is an
  opinion.
- Modules are the Angular standard. Modules are heavily documented directly by Angular and is the
  code blocking tool used by Angular for their own code and for all of their examples in their
  documentation. This provides a safe guard rail for our growing team, as standards will be easily
  identified or learned just with the Angular docs.
- Modules are great for scoping code. We can provide module scoped child components, services,
  pipes, etc. that are isolated to a specfic code block to its module and guard off that code from
  other blocks. This is incredibly self documenting. Barrel files do not support this in any way,
  meaning all components and their dependencies will need to be declared somewhere else, likely in a
  much larger module that doesn't provide context to relationships between code blocks. They are not
  particularly self documenting other than saying "this stuff is in this folder". Being able to
  scope services to modules specifically will be very helpful as we try to better isolate
  functionality within a feature. One example of this is our transition to reactive forms. With
  NgModules a form can become a form service that seperates input validation, submit and error
  handling, etc. from rendering of a template.
- Modules enable us to use lazy loading for routes
- Using modules provide a natural and scalable file system template, and the angular cli can be used
  to generate every piece of it if desired. (see 006)
- We already use modules in several ways, most noteably for our global services. Extending this
  pattern to components and their scoped dependencies is a natural progression of this.
- Eventually, Angular will not be requiring an NgModule for providing component dependencies and we
  will be able to easily migrate single component blocks to this style if desired.

#### Cons

- You can get some not great errors when mishandling Angular Modules
- A lot of the time, especially with simple modules, this may seem like extra boilerplate. I would
  argue this is just setting up to scale.

### Barrel Files

#### Pros

- Less boilerplate
- Not having to deal with Angular Module related error messages during development.

#### Cons

- Less explicit about the relationship siblings have with each other in a code block
- No way to enforce scoping of services or components to just their code block.
- No lazy loading, or we implement multiple patterns and use modules just when routing is involved,
  but this can be confusing.

### Example

We are already doing this a bit (predating ADRs) with the
[VaultFiltersModule](https://github.com/bitwarden/clients/blob/master/apps/web/src/app/modules/vault-filter/vault-filter.module.ts).
It demonstrates a couple of useful features of NgModules.
