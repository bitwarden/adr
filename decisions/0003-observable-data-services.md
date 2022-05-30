# Adopt Observable Data Services for Angular

## Context and Problem Statement

The Bitwarden clients currently have a quite complex state architecture, where
all the state is handled by a single service. This has resulted in everything
being tightly coupled to the `StateService` essentially making it a God object.
We need to decouple the services from the state.

At the same time we need to migrate our components

## Considered Options

* [Observable/Reactive Data Services][observable]
* [NGRX](https://ngrx.io/) - Reactive State for Angular (Redux implementation)

## Decision Outcome

Chosen option: **Observable data services**, because

* Allows us to quickly interate towards a more reactive data model.
* Does not require a significant upfront investment.
* The work towards a reactive data model will allow us to adopt patterns like
NGRX in the future should it be needed.

### Example

#### Organizations

The `OrganizationService` should take ownership of all Organization related
data.

```ts
// state.service.ts

// StateService should have generic data access methods.
//  We most likely do not need to store things in memory for `StateService`
//  since the memory portion should be handled by the individual services.
interface StateService {
  getAccountData<T>: (account: string, key: string, options?: StorageOptions) => Promise<T>;
  saveAccountData<T>: (account: string, key: string, options?: StorageOptions) => Promise<T>;
}

// organization.service.ts

// StorageKey is an internal constant, and should be prefixed with the domain.
const StorageKey = "organizations";

// OrganizationService keeps track of the active account and replaces it's data if needed.
class OrganizationService {
  private _activeAccount: Account;
  private _organizations: new BehaviorSubject<Organization[]>([]);
  organizations$: Observable<Organization[]> = this._organizations$.asObservable();

  constructor() {
    stateService.account$.subscribe((account) => {
      this._activeAccount = account;
      this.getOrgsForAccount(account).then(this._organizations$.next);
    }
  }

  async save(organizations: { [id: string]: OrganizationData }) {
    return await this.stateService.saveAccountData(this._activeAccount, StorageKey, organizations);
    await this.stateService.save(organizations);
    await this._organizations$.next(await this.decryptOrgs(this._activeAccount, organizations));
  }
}
```

## Pros and Cons of the Options

### Observable Data Services

* Good: Lightweight
* Good: Can be refactored incrementally
* Good: Reactive, will notify on changes.
* Bad: No strict standard, more a set of guidelines.
* Bad: State is split into multiple tiny "stores"

### NGRX

NGRX is the most popular Redux implementation for Angular. For more details,
read about the [motivation behind redux][redux-motivation] and view this
[diagram of NGRX architecture](https://ngrx.io/guide/store).

* Good: Most popular redux library for Angular
* Good: Decouples components
* Good: Single state which simplifies operations
* Bad: Requires a significant rewrite of the whole state layer
* Bad: Adds complexity and can make it difficult to understand the data flow.

[observable]: https://blog.angular-university.io/how-to-build-angular2-apps-using-rxjs-observable-data-services-pitfalls-to-avoid/
[redux-motivation]: https://redux.js.org/understanding/thinking-in-redux/motivation