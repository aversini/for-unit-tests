# DO NOT REMOVE


## TL;DR

This quasi-empty repo is used by other packages (fedtools-*, envtools) to run unit tests that require a valid git repository, with commits, logs and tags.

Usualy used by Travis since unit tests that require a valid repo can normaly use their own. The problem with Travis is that it's doing a shallow clone, bypassing all history and such.

To use this repo with Travis, you need to do 2 things:

  * Configure your Travis yml to install this repo before running the tests
  * In your tests, check if you are in Travis then use this repo, otherwise, use the local path.

## Travis yml example
```
language: node_js
node_js:
  - "4.2"
  - "0.12"
install:
  - cd ..
  - git clone https://github.com/aversini/for-unit-tests.git
  - cd fedtools-utilities
  - npm install
script: "npm run-script ci"
```

## Test example
```
  if (process.env.TRAVIS) {
    // Special case for Travis.
    // Some git commands fail because Travis does a shallow clone.
    // To bypass this limit, I've created an empty git repo (for-unit-tests)
    // that I clone before running the CI script (see the corresponding .travis.yml)
    cwd = path.join(__dirname, '..', '..', 'for-unit-tests');
  } else {
    cwd = __dirname;
  }
```
