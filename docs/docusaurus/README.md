---
title: Documentation Site
slug: /readme
---

This documentation site is built using [Docusaurus 2](https://v2.docusaurus.io/), a modern static website generator.

## System Requirements

https://docusaurus.io/docs/installation#requirements

## Installation

Follow the [CONTRIBUTING_CODE](https://github.com/great-expectations/great_expectations/blob/develop/CONTRIBUTING_CODE.md) guide in the `great_expectations` repository to install dev dependencies.

Then run the following command from the repository root to install the rest of the dependencies and build documentation locally (including prior versions) and start a development server:
```console
invoke docs
```

Once you've run `invoke docs` once, you can run `invoke docs --start` to start the development server without copying and building prior versions. Note that if prior versions change your build won't include those changes until you run `invoke docs --clean` and `invoke docs` again.

```console
invoke docs --start
```

To remove versioned code, docs and sidebars run:

```console
invoke docs --clean
```


## Linting

[standard.js](https://standardjs.com/) is used to lint the project. Please run the linter before committing changes.

```console
invoke docs --lint
```

## Build

To build a static version of the site, this command generates static content into the `build` directory. This can be served using any static hosting service.

```console
invoke docs --build
```

## Deployment

Deployment is handled via [Netlify](https://app.netlify.com/sites/niobium-lead-7998/overview).

## Other relevant files

The following are a few details about other files Docusaurus uses that you may wish to be familiar with.

- `sidebars.js`: JavaScript that specifies the sidebar/navigation used in docs pages
- `src`: non-docs pages live here
- `static`: static assets used in docs pages (such as CSS) live here
- `docusaurus.config.js`: the configuration file for Docusaurus
- `babel.config.js`: Babel config file used when building
- `package.json`: dependencies and scripts
- `yarn.lock`: dependency lock file that ensures reproducibility

sitemap.xml is not in the repo since it is built and uploaded by a netlify plugin during the documentation build process. 

## Documentation changes checklist

1. For any pages you have moved or removed, update _redirects to point from the old to the new content location


## Versioning

To add a new version, follow these steps:

(Note: yarn commands should be run from docs/docusaurus/)

1. It may help to start with a fresh virtualenv and clone of gx.
1. Check out the version from the tag, e.g. `git checkout 0.15.50`
1. Modify `docs/docusaurus/docusaurus.config.js` and `docs/docusaurus/docs/components/_data.jsx` to the new version (the patch version may be off by 1 e.g. 0.17.22 instead of 0.17.23 since we update this in the release PR which comes after the release tag).
1. Make sure dev dependencies are installed `pip install -c constraints-dev.txt -e ".[test]"` and `pip install pyspark`
1. Install API docs dependencies `pip install -r docs/sphinx_api_docs_source/requirements-dev-api-docs.txt`
1. Build API docs `invoke api-docs` from the repo root.
1. Run `yarn install` from `docs/docusaurus/`.
1. Temporarily change `onBrokenLinks: 'throw'` to `onBrokenLinks: 'warn'` in `docusaurus.config.js` to allow the build to complete even if there are broken links.
1. Run `yarn build` from `docs/docusaurus/`.
1. Create the version e.g. `yarn docusaurus docs:version 0.15.50` from `docs/docusaurus/`.
1. Pull down the version file (run `docs/docs_version_bucket_info.py` to generate the url).
1. Unzip and add your newly created versioned docs via the following:
1. Copy the version you built in step 4 from inside `versioned_docs` in your repo to the `versioned_docs` from the unzipped version file.
1. Copy the version you built in step 4 from inside `versioned_sidebars` in your repo to the `versioned_sidebars` from the unzipped version file.
1. Add your version number to `versions.json` in the unzipped version file (at the top if it is the most recent).
1. Run `zip oss_docs_versions.zip versioned_docs versioned_sidebars versions.json` to zip up `versioned_docs`, `versioned_sidebars` and `versions.json` as `oss_docs_versions.zip` and upload the zip file to the s3 bucket (see `docs/docs_version_bucket_info.py` for the bucket name). Make sure `versioned_docs`, `versioned_sidebars` and `versions.json` are at the top level of the zip file (not nested in a folder).
1. Once the docs are built again, this zip file will be used to build the earlier versions.
