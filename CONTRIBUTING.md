# Contributing Guide

## For Contributors

Contributions are always welcome!
If you have discovered a bug or have a feature request, [please create an issue on GitHub](https://github.com/kintone/js-sdk/issues/new/choose).
Pull requests are also welcome when you find trivial bugs or typos.

We use English for all commit messages, code comments, issues, pull requests.
For Japanese speakers, we have another repository: https://github.com/kintone/js-sdk-ja.
Please file an issue there.

(Translated in Japanese)
本リポジトリではコミットメッセージやコードコメント、issues、pull requests において英語を使用しています。
日本語話者向けに専用のリポジトリも用意しています: https://github.com/kintone/js-sdk-ja
こちらに issue をご登録ください。

### Setup

This repository is a monorepo using pnpm.

```sh
% cd js-sdk
% pnpm install
```

### Develop

```sh
% cd js-sdk
% pnpm start
```

### Test

Before run `lint` and `test` scripts, you have to run `build`.

```sh
% cd js-sdk
% pnpm build
% pnpm test
% pnpm lint
```

## For Maintainers

### Merge

After you have approved a PR, please merge the PR using **Squash and merge** with [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) format. The commit message should include the target package name as the scope.

```sh
# rest-api-client is the scope of this commit
feat(rest-api-client): add record.getRecord()
```

### Release

1. Verify the change log of the release PR `chore: release main`
2. Merge the PR using **Squash and merge**

## Create a new package

When you create a new package, you must define the following npm-scripts, otherwise CI would be failed.

### `build`

You have to define `build` script to build source files.
In most cases, the script would be `tsc --build --force`.

Actually, the script isn't run when releasing new versions of packages, so it's not necessary.
But it might be helpful when you want to build only specific packages rather than all pacakges, I'm not sure there is any case it is necessary though.

But `prebuild` and `postbuild` are important.

`prebuild` is run before `build`, so it's useful to clean build assets before running a new build.
In most cases, the script would be `pnpm clean`, which runs `rimraf` for build assets directories.

`postbuild` is run after `build`, so it's useful to build other assets like ESM and UMD builds.

### `lint`

You have to define `lint` script to lint source files.
In most cases, the script would be the `eslint` command.

You don't have to `tsc --noEmit` as the `lint` script because `tsc` is run as the `build` script that is run before the `lint` script.

### `test`

You have to define `test` script to test source files.
In most cases, the script would be the `jest` command.

### `test:ci`

You have to define `test` script to test source files on CI environments.
In most cases, the script would be the `jest --runInBand` command.

**GitHub Actions might not need the `--runInBand` to run Jest successfully, so we might remove the script in the future.**

### Configure TypeScript Project References

You have to add the new package into the `references` field in `packages/tsconfig.json`, and its dependencies in `kintone/js-sdk` have to be added into the `references` field in the package's `tsconfig.json`.
The `tsconfig.json` has to extend `packages/tsconfig-base.json`.

## Import a package into `kintone/js-sdk`

You can import an exsiting package with `lerna import path/to/package`.

After the PR has been merged, you have to create a tag at the last commit where the package had been released.
The tag format is `package-name@version`
