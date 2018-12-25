# jwt-manager

Server-side manager for active JSON Web Tokens (JWTs)

## Install

### Install from GitHub:

#### Spesific release:

**Note:** Replace `$VERSION` with the version number.

```sh
$ npm install --save https://github.com/revam/node-jwt-manager/releases/download/v$VERSION/package.tgz
```

### Install from git.lan:

Internet people can ignore this section.

#### Latest release:

```sh
$ npm install --save https://git.lan/mist@node/jwt-manager@latest/npm-pack.tgz
```

#### Spesific release:

**Note:** Replace `$VERSION` with the version number.

```sh
$ npm install --save https://git.lan/mist@node/jwt-manager@v$VERSION/npm-pack.tgz
```

## Usage

**Note:** `await` is not actually available in the global context, but let's
assume it is for this example.

```js
import JWTManager from "jwt-manager";

// Example user
const user = {
  id: "00000000-0000-0000-0000-000000000000",
  name: "John Smith",
  username: "josm",
};

const jm = new JWTManager({
  findSubject(test, test2, ...testRest) {
    console.log("%s %s %s", test, test2, testRest.join(" "));
    if (test === "this" && test2 === "is" && testRest[0] === "SPARTA") {
      return [user.id, {name: user.name}]
    }
  }
});

let token;

// We know if we provid the three arguments "this", "is", and "SPARTA" we get
// a signed token for our user data.
token = await jm.add({ args: ["this", "is", "SPARTA"]}); // token is "<header>.<payload>.<signature>"

// If we cannot find a subject with given arguments, then no token will be returned.
token = await jm.add({ args: ["this", "is", "GREEK"]}); // token is undefined

// Verifies an existing __signed__ token, and returns its decoded value if signature matches.
let obj = await jm.verify(token);

// Decodes token without verifying signature or content.
let obj2 = jm.decode(token);

// invalidates token or obj.
await jm.invalidate(token || obj); // true if token or obj is now invalid.
```

## Documentation

Documentation is available online at
[GitHub Pages](https://revam.github.io/node-jwt-manager/), or locally at
[http://localhost:8080/](http://localhost:8080/) with the following command:

```sh
$ npm run-script docs
```

## Typescript

This module includes a [TypeScript](https://www.typescriptlang.org/)
declaration file to enable auto complete in compatible editors and type
information for TypeScript projects. This module depends on the Node.js
types, so install `@types/node`:

```sh
npm install --save-dev @types/node
```

## Changelog and versioning

All notable changes to this project will be documented in [changelog.md](./changelog.md).

The format is based on [Keep a Changelog](http://keepachangelog.com/en/1.0.0/)
and this project adheres to [Semantic Versioning](http://semver.org/spec/v2.0.0.html).

## License

This project is licensed under the MIT license. See [license](./license) for the
full terms.
