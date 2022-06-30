# Incorrect github package resolution to npm registry

## Issue description

This repo contains the following `package.json` and can be installed with the `npm install github:<username>/<repository>` command

```json
{
  "name": "debug",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/PierreJeanjacquot/my-custom-debug.git"
  },
  "author": "",
  "license": "ISC",
  "bugs": {
    "url": "https://github.com/PierreJeanjacquot/my-custom-debug/issues"
  },
  "homepage": "https://github.com/PierreJeanjacquot/my-custom-debug#readme"
}
```

Note that the `name` used in the `package.json` is the same than the existing npm package [debug](https://www.npmjs.com/package/debug).

npm audit features incorrectly resolve this package as the [debug](https://www.npmjs.com/package/debug) package hosted on npmjs.com

## Issue reproduction

install this package from the github repository

```sh
npm i github:PierreJeanjacquot/my-custom-debug#main
```

npm install reports 1 low severity vulnerability

the custom `debug` package is correctly installed from github

```sh
npm ls debug
```

```text
install-test@ /home/pierre/install-test
└── debug@1.0.0 (git+ssh://git@github.com/PierreJeanjacquot/my-custom-debug.git#abfeeee014c6cfc6a3a11beec5595a18c31f6704)
```

however audit resolves the custom package as [debug](https://www.npmjs.com/package/debug).

```sh
npm audit
```

```text
# npm audit report

debug  <2.6.9
Regular Expression Denial of Service in debug - https://github.com/advisories/GHSA-gxpj-cx7g-858c
No fix available
node_modules/debug

1 low severity vulnerability

Some issues need review, and may require choosing
a different dependency.
```
