# CircleCI Documentation

[![CircleCI](https://dl.circleci.com/insights-snapshot/gh/circleci/circleci-docs/master/build-deploy/badge.svg?window=30d)](https://app.circleci.com/insights/github/circleci/circleci-docs?branches=master&workflows=build-deploy&reporting-window=last-30-days&insights-snapshot=true)

[![CircleCI Build Status](https://circleci.com/gh/circleci/circleci-docs.svg?style=shield)](https://circleci.com/gh/circleci/circleci-docs)
[![GitHub license](https://img.shields.io/badge/license-MIT-blue.svg)](https://raw.githubusercontent.com/circleci/circleci-docs/master/LICENSE)
[![CircleCi Community](https://img.shields.io/badge/community-CircleCI%20Discuss-343434.svg)](https://discuss.circleci.com)
[![ja translation](https://img.shields.io/badge/dynamic/json?color=blue&label=ja&style=flat&query=%24.progress.0.data.translationProgress&url=https%3A%2F%2Fbadges.awesome-crowdin.com%2Fstats-13528254-306408.json)](https://crowdin.com)

This is the public repository for [CircleCI Docs](https://circleci.com/docs/), a
static website generated by [Jekyll](https://jekyllrb.com/). If you find any
errors in our docs or have suggestions, please follow our [Contributing
Guide](docs/CONTRIBUTING.md) to submit an issue or pull request.

For Server 3.x documentation development, see the [Server 3.x documentation](#server-3.x-documentation) section.

For minor changes like typos, you can click **Suggest an edit to this page**,
located at the bottom of each article. This will take you to the source file on
GitHub, where you can submit a pull request for your change through the UI.

For larger edits or new articles, you'll want to set up a local environment for
editing. Please see our [Local Development
document](./docs/local-development.md) to set that up.

If you have a question or need help debugging, please head to [CircleCI
Discuss](https://discuss.circleci.com/) where our support team will help you
out.

## Preview

It is possible to build and serve the Jekyll docs locally (without ruby or jekyll installed) using Docker and docker compose.
If you already configured Jekyll on your local machine, you need to delete `vendor` directory, before running the command.

```
$ docker-compose up
```

## Documentation Components

This repository houses and manages several arms of documentation for CircleCI.
This section will provide a brief overview of each "component" and how to get
started with making changes.

### `/Jekyll` - Main Site

This is the main [CircleCI documentation site](https://circleci.com/docs/2.0/).
This is built with Jekyll and houses the majority of our documentation. Other
branches of documentation (`src-api`, `src-crg`, etc) eventually get moved into
this folder (in our build process) and integrated into the Jekyll Site. Follow
the [local development guide](./docs/local-development.md) to get started with
building the Jekyll site.

We also have an automated code review tool setup, so it will run [markdownlint
](https://github.com/DavidAnson/markdownlint) on your PR and review for any
markdown style violations. The rules are located at [.markdownlint.jsonc
](https://github.com/DavidAnson/markdownlint/blob/main/.markdownlint.jsonc).
You can also fix most of the violations automatically. You can read more 
[here](https://github.com/circleci/circleci-docs/blob/master/docs/local-development.md#markdownlinter).

### `/src-api` - API v1.1 and v2 Build Tooling

Our API documentation source can be found in this folder.

**API v1** is written by hand, and compiled to work with
[Slate](https://github.com/slatedocs/slate). The compilation and deployment of
`v1` is handled by our `.circleci/config.yml`, which calls our `build_api_docs`
script. If you need to make changes to our V1 documentation, go to
`src-api/source/includes` and make changes as needed in the markdown files.

API v2 is compiled from an [OpenAPI
spec](https://github.com/OAI/OpenAPI-Specification). We use
[Redoc](https://github.com/Redocly/redoc) to compile our spec into a webpage. To
see the compilation process, refer to `build_api_docs.sh` and our
`.circleci/config.yml`. If you need to make changes to the output site, you will
likely need to make source code changes to the API, where the docs are generated
from.

This is the build folder we use for automatically generating documentation for
the CircleCI API v2. This uses [Slate](https://github.com/slatedocs/slate) and
[Widdershins](https://github.com/Mermade/widdershins) to create documentation
with a spec (that follows the Open API Spec) generated from the CircleCI code
base.

### `/src-js` - Javascript Files

Some JavaScript files that make use of Webpack  and bundles a Javascript
file that eventually gets loaded into Jekyll. Our JavaScript files are
splintered between Jekyll's assets folder and here. We should be navigating
toward one solution that will eventually aggregate all JS source code in one place.

### `/src-shared` - Shared Assets with circleci.com

This is a *git sub-module* for shared content with the main [CircleCI
website](https://circleci.com/docs/). The `js` folder within is symlinked into
Jekyll's assets folder for JavaScript. Eventually, the JS here could/should be
integrated with a better JS bundle solution per above. Please see the [local
development](./docs/local-development.md) guide for more information about
pulling in the updates for the sub-module.

## Server 3.x documentation

Server documentation is written in ASCIIDOC and built for the web and PDF using 
[jekyll-asciidoc](https://github.com/asciidoctor/jekyll-asciidoc) and [asciidoctor-pdf](https://github.com/asciidoctor/asciidoctor-pdf) plugins.

### Server capitalization

Server should not be capitalized, if it can be avoided.

### Branch nomenclature for subsequent server releases

**Note:** If you are creating server documentation for subsequent releases (i.e., 3.0 to 3.1), please 
create a PR against the `server/3.<NEW_VERSION_NUMBER>-preview` branch (please create a branch with
this nomenclature, if you don't see one).

- Prefacing a branch with `server/` will generate PDFs built of the files described in the following section. The PDFs appear as build artifacts 
and can be accessed from the CircleCI App.
- Appending `-preview` to a branch will deploy the preview site of the documentation updates.

### Important files

Below are important files to note when updating server documentation:

- `_server-3-overview.adoc`: This file "stitches" any Overview-related documentation together, such as overall architecture 
  and components, the server changelog, and frequently asked questions. This file is not expected to be updated often.
- `_server-3-install-guide.adoc`: This file "stitches" any Installation-related documentation together. **Any brand-new sections must be added here.**
- `_server-3-ops-guide.adoc`: This file "stitches" any Operator-related documentation together. **Any brand-new sections must be added here.**

- `build_pdfs_asciidoc.sh`: This is the shell script that is executed as a part of the pipeline when server documentation 
  is built. It uses `asciidoctor` to build the three PDFs above. You should not have to update this file.
  
- `sidenav.yml`: Defines links in the sidebar. **Any brand-new sections must be added here. Links to the PDFs must also be
  updated here with every new release. See the section named [`Server v3.x PDFs`](https://github.com/circleci/circleci-docs/blob/21815f9ef8ff7213eff54f920a452032b06cccb8/jekyll/_data/sidenav.yml#L355).**

- `_config.yml`: Update this file when you need to bump version numbers of `serverversion` (every new release),
  `terraformversion`, `kubectlversion`, `helmversion`, or `kostversion`. Consider these your global variables to use in any
  server documentation, (use the variable name in between two brackets, i.e. `{serverversion}`).
  
### Server changelogs

To update the server changelog, you will need to update both the docs site and outer. JIRA tickets (and their associated
pull requests, details, etc.) are linked in the Server Release e-mail sent out monthly by Sara Meisburger, in the 
Server 3.x Release Plan document.

- **`server-3-whats-new.adoc`**: Make sure the **Known Issues** section is up to date, and add a new section for the new 
  release.

- **Outer changelog**: the outer changelog is located in the private repository for circleci.com. Create a file containing
  the contents of the previous changelog (located in `changelog_ccie/`). You will need to update the `timestamp`
  and `version` values (and the Release header). Make sure the **Known Issues** section is up to date.

**Note:** For every new changelog, be sure to update the `serverversion` number in `_config.yml` and add the newly 
generated PDFs to your pull request. You will also need to change the links in `sidenav.yml` as described in [Important files](#important-files).

### More server documentation specifics

For more information on server documentation development (this includes overall syntax suggestions, local development, 
and 2.x-specific information) check out the previous instructions [here](https://github.com/circleci/circleci-docs/blob/master/docs/server-docs.adoc).

## Testing Checklist 
To allow for more flexibility, please find the checklist inside our confluence [space](https://circleci.atlassian.net/l/c/Tm5oiCFF).

## License Information
Documentation (guides, references, and associated images) is licensed as
Creative Commons Attribution-NonCommercial-ShareAlike CC BY-NC-SA. The full
license can be found
[here](http://creativecommons.org/licenses/by-nc-sa/4.0/legalcode), and the
human-readable summary
[here](http://creativecommons.org/licenses/by-nc-sa/4.0/).

Everything in this repository not covered above is licensed under the [included
MIT license](./docs/licence.md).
