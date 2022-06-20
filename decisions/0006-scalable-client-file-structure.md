# Adopt a Scalable File Structure For Angular Clients


## Issue
Currently code blocks in our Angular clients are defined using Angular Components (e.g ciphersComponent, vaultComponent). Many of these code blocks have outgrown this level of abstraction, and would benefit greatly from having access to their own child components, models, services, pipes, and other structures. Likewise, many of these components share functionality with other components in scattered areas across the file system, leading to long imports for inheritance at best and redundant code at worst (e.g the CiphersComponent and OrganizationCiphersComponentComponent).

Fixing this issue is a multi part effort. This ADR will cover one core piece of the solution: how the file structure of our Angular clients can be refactored to clearly isolate and scale code blocks.

## Considered Options
There are many patterns in use to get this job done, so I will just share what I would like for it to look like.
```
web/src/app
    modules
        feature (i.e vault-filters)
            shared (if servicing multiple modules)
                feature.module.ts
                feature.service.ts
            components (for child components. This pattern will be commonly used for large components i.e VaultFiltersModule.
            services
                feature.forms.service.ts
            models
            feature.module.ts (or sub-folders if servicing multiple features)
            feature.component.ts
    services
        services.module.ts
        {reactiveService}.service (reactive services that handle UI updates. i.e a collapsedFilterNodes.service that pushes updates to and from VaultFiltersComponent and StateService. This would establish a pattern like 0003, but for manipulating the UI).
        {rootLevelService}.service (the services that already exist here)
    pipes
    guards
    app.component.html
    app.component.ts
    app.module
    main.ts
    oss-routing.module.ts
    oss.module.ts
    polyfills.ts
    wildcard-routing.module.ts
```


### Decision Outcome
Implementing a file structure modeled after the above example.


### Pros
* This structure is very scalable, each module can grow and shrink at its own pace without disturbing others. There are patterns baked in to the file structure to aid many different growth related needs.
* There is a clear hierarchy of classes. 
* Modules can easily manage their own services, pipes, and children.
* Decouple parents from their children. No more ViewChilds, as we will be using reactive services to handle communication between components.

### Cons
* This structure can quickly get very deep, and may in fact look that way for us for a period of time. It's important to remember that the structual folders (`modules`, `components`, `services`) are guard rails for developers. They offer clear boxes for where to put classes based on what the class does, and can be infinetly nested without breaking the pattern. If we move components to this structure and they are deeply nested (many iterations of a `modules` folder) that is a code smell and an indicator that we should reduce the redundancy of those features, or split them out from each other completely.  It's also worth mentioning that our existing structure is very nested, as we build a complicated product. 
* This is a major structure change that will need to happen iteritvly 
