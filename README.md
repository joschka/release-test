# release-npm-package

Minimal starting point for creating a npm package which can be released with one manual click or (if applicable) triggered automatically by an event. Requires conventional commits!

## Enabler: conventional commits

> The Conventional Commits specification is a lightweight convention on top of commit messages. It provides an easy set of rules for creating an explicit commit history; which makes it easier to write automated tools on top of. This convention dovetails with SemVer, by describing the features, fixes, and breaking changes made in commit messages.

– [conventionalcommits.org](https://www.conventionalcommits.org/)

Putting in the effort to write machine readable commits at commit time frees us from creating a changelog and figuring out the next version number at release time. This makes releasing super simple and ensures that it happens often.

## Assumption: `dist` folder is released

Assumption: you have some kind of build step and you want to release only your build artifact (`dist` folder). Make sure to have a `package.json` in the `dist` folder before releasing.

## Good to know: package.json version

The `"version"` field in the `package.json` *in the repository* is never updated. It's always `"0.0.0-development"`. The *published* `package.json` though contains the version.

## Good to have: npm package provenance

It allows linking packages to their source and build and increases trust in the supply chain. This is only possible when the `npm release` runs on GitHub Actions, not when you release on your local machine.

Make sure to have the following config on your `package.json` to enable npm package provenance.

```json
"publishConfig": {
    "provenance": true
},
```

## Checklist

In case you just want to cherry pick for your exisiting package.

* `package.json`: set `"version"` to `"0.0.0-development"` (optional, but recommended)
* `package.json`: add `"publishConfig": { "provenance": true }` (optional, but recommended)
* repository settings: add a `NPM_TOKEN` repository secret (repo -> settings -> secrets and variables -> actions -> new repository secret). Find the value in 1Password ("NPM"), look for "Publish Token", its value starts with "npm_"
* copy `.github/workflows/release.yml` and adapt to your needs. Make sure to not remove permissions and too keep the configuration of the `setup-node` action.
* `npm install --save-dev semantic-release`

