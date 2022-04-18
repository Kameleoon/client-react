# Kameleoon JavaScript SDK Packages

### Configure nexus user
```bash
npm config set @kameleoon:registry https://nexus.kameleoon.net/repository/npm-modules/
npm login --registry=https://nexus.kameleoon.net/repository/npm-modules/
```

### Release new version

1. `make start` - start docker container
2. `make install` -  install npm dependencies
2. make changes and create a commit (linting and commit message will be checked on commit according to [Conventional Commits](https://conventionalcommits.org))
3. `make release` - generate `CHANGELOG.md` and upgrade `package.json` version based on previous commits (`CHANGELOG_INTERNAL.md` will be created with repository
specific changes, make sure to later commit it to kameleoon github repository).
4. `make release-commit` - create a release commit with pre-defined commit message.
5. push changes to repository (git tags will be set automatically)

### Docker commands
* `make clean` - remove container
* `make start` - build and run container
* `make stop` - stop container
* `make install` - install node modules
* `make clean-node-modules` - remove node modules
* `make reinstall` - reinstall node modules
* `make build` - rebuild container
* `make release` - clean local tags, fetch remote tags, update `package.json` and generate `CHANGELOG.md`
* `make storybook` - build and run storybook
* `make test` - run tests

