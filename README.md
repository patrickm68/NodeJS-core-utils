# Node.js Core Utilities
[![npm](https://img.shields.io/npm/v/node-core-utils.svg?style=flat-square)](https://npmjs.org/package/node-core-utils)
[![Build Status](https://img.shields.io/travis/nodejs/node-core-utils.svg?style=flat-square)](https://travis-ci.org/nodejs/node-core-utils)
[![AppVeyor Build Status](https://img.shields.io/appveyor/ci/joyeecheung/node-core-utils/master.svg?style=flat-square&logo=appveyor)](https://ci.appveyor.com/project/nodejs/node-core-utils/history)
[![codecov](https://img.shields.io/codecov/c/github/nodejs/node-core-utils.svg?style=flat-square)](https://codecov.io/gh/nodejs/node-core-utils)
[![Known Vulnerabilities](https://snyk.io/test/github/nodejs/node-core-utils/badge.svg?style=flat-square)](https://snyk.io/test/github/nodejs/node-core-utils)

CLI tools for Node.js Core collaborators.

<!-- TOC -->

- [Usage](#usage)
  - [Install](#install)
  - [Setting up credentials](#setting-up-credentials)
- [`ncu-config`](#ncu-config)
- [`get-metadata`](#get-metadata)
  - [Git bash for Windows](#git-bash-for-windows)
  - [Features](#features)
- [Contributing](#contributing)
- [License](#license)

<!-- /TOC -->

## Usage

### Install

```
npm install -g node-core-utils
```

If you would prefer to build from the source, install and link:

```
git clone git@github.com:nodejs/node-core-utils.git
cd node-core-utils
npm install
npm link
```

### Setting up credentials

Most of the tools need your GitHub credentials to work. You can either

1. Run any of the tools and you will be asked in a prompt to provide your
  username and password in order to create a personal access token.
2. Or, create a personal access token yourself on Github, then set them up
  using an editor.

If you prefer option 2, [follow these instructions](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)
to create the token.

Note: We need to read the email of the PR author in order to check if it matches
the email of the commit author. This requires checking the box `user:email` when
you create the personal access token (you can edit the permission later as well).

Then create an rc file (`~/.ncurc` or `$XDG_CONFIG_HOME/ncurc`):

```json
{
  "username": "your_github_username",
  "token": "token_that_you_created"
}
```

Note: you could use `ncu-config` to configure these variables, but it's not
recommended to leave your tokens in your command line history.

## `ncu-config`

Configure variables for node-core-utils to use. Global variables are stored
in `~/.ncurc` while local variabels are stored in `$PWD/.ncu/config`.

```
ncu-config <command>

Commands:
  ncu-config set <key> <value>  Set a config variable
  ncu-config get <key>          Get a config variable
  ncu-config list               List the configurations

Options:
  --version  Show version number                                       [boolean]
  --help     Show help                                                 [boolean]
  --global                                            [boolean] [default: false]
```

## `get-metadata`

This tool is inspired by Evan Lucas's [node-review](https://github.com/evanlucas/node-review),
although it is a CLI implemented with the Github GraphQL API.

```
get-metadata <identifier>

Retrieves metadata for a PR and validates them against nodejs/node PR rules

Options:
  --version         Show version number                                [boolean]
  --owner, -o       GitHub owner of the PR repository                   [string]
  --repo, -r        GitHub repository of the PR                         [string]
  --file, -f        File to write the metadata in                       [string]
  --check-comments  Check for 'LGTM' in comments                       [boolean]
  --max-commits     Number of commits to warn              [number] [default: 3]
  --help, -h        Show help                                          [boolean]
```

Examples:

```bash
PRID=12345

# fetch metadata and run checks on nodejs/node/pull/$PRID
$ get-metadata $PRID
# is equivalent to
$ get-metadata https://github.com/nodejs/node/pull/$PRID
# is equivalent to
$ get-metadata $PRID -o nodejs -r node

# Or, redirect the metadata to a file while see the checks in stderr
$ get-metadata $PRID > msg.txt

# Using it to amend commit messages:
$ get-metadata $PRID -f msg.txt
$ echo -e "$(git show -s --format=%B)\n\n$(cat msg.txt)" > msg.txt
$ git commit --amend -F msg.txt
```

### Git bash for Windows
If you are using `git bash` and having trouble with output use `winpty get-metadata.cmd $PRID`.

current known issues with git bash:
- git bash Lacks colors.
- git bash output duplicates metadata.

### Features

- [x] Generate `PR-URL`
- [x] Generate `Reviewed-By`
- [x] Generate `Fixes`
- [x] Generate `Refs`
- [x] Check for CI runs
- [x] Check if committers match authors
- [x] Check 48-hour wait
- [x] Check two TSC approval for semver-major
- [x] Warn new commits after reviews
- [ ] Check number of files changed (request pre-backport)

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md).

## License

MIT. See [LICENSE](./LICENSE).
