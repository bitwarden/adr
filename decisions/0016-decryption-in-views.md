---
parent: Decisions
nav_order: 14
adr: "0016"
status: In progress
date: 2022-11-04
tags: [clients, typescript]
---

# Move Decryption and Encryption to Views

## Context and Problem Statement

Bitwarden has a couple of different models for representing data.

- `<Domain>` - The domain model which represents the encrypted data state.
- `<Domain>View` - The View Model which typically represents the decrypted state of domain models.

Since we have two different models for representing the encrypted and decrypted state of the same
_Domain_, this also means we need a way to convert between the two models i.e. encrypting and
decrypting the data.

### How it's currently being done

The way this is currently done is by having the `<Domain>Service` expose either an _Observable_
which contains the decrypted views, or by having a promise based method to decrypt it. The
`<Domain>Service` also typically expose a `encrypt` method which converts from the `View` and the
`Domain` model.

There is also typically a `decrypt` method on `Domain` models themselves which performs the actual
decrypting logic. It does so by calling `decrypt` on the `EncString` objects which in turn relies on
a global container service to retrieve the `CryptoService` and `EncryptService` for performing the
actual operations.

### The problems

There are a couple of problems with this approach:

- Encrypting and decryption is split into two different places. Decryption happens directly on the
  domain model, while encryption happens in the service.
- We rely on a global container service to retrieve the `CryptoService` and `EncryptService`. Which
  makes it harder to properly unit test.
- Our current models act essentially as a transformation pipeline.
  `Request -> Data -> Domain -> View`. And there is a slightly different transformation pipeline for
  exports where the end state is a `Export` model, not a `View` model.
- It would be nice to have a way to support multiple `View` models per domain in the future.

## Considered Options

- **Move logic to `<Domain>Service`** - We already have services for the different domains, so it
  would make sense to move the logic there.
- **Move logic to `<Domain>View`** - We could move the logic to the `View` models themselves,
  combined with a generic service to encrypt and decrypt views.

## Decision Outcome

Chosen option: **Move logic to `<Domain>View`**.

### Implementation

Example [PR for Folders](https://github.com/bitwarden/clients/pull/3732).

```ts
class FolderDomain implements DecryptableDomain {

  id: string;
  name: EncString;
  revisionDate: Date;

  keyIdentifier(): string | null {
    return null;
  }
}

class FolderView implements Encryptable<Folder> {
  id: string = null;
  name: string = null;
  revisionDate: Date = null;

  keyIdentifier(): string | null {
    return null;
  }

  async encrypt(cryptoService: CryptoService, key: SymmetricCryptoKey): Promise<Folder> {
    const folder = new Folder();
    folder.id = this.id;
    folder.name = await cryptoService.encrypt(this.name, key);
    folder.revisionDate = this.revisionDate;

    return folder;
  }

  static async decrypt(cryptoService: CryptoService, key: SymmetricCryptoKey, model: Folder) {
    const view = new FolderView();
    view.id = model.id;
    view.name = await model.name.decryptWithCryptoService(cryptoService, key);
    view.revisionDate = model.revisionDate;
    return view;
  }
}
```

Which would be used like this:

```ts
  const folder = new Folder();
  
  const folderView: FolderView = this.cryptoService.decrypt(FolderView, folder);

  const encryptedFolder: Folder = this.cryptoService.encrypt(folderView);
```
