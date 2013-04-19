# Releaseable (BETA - work in progress)

Bash only (language agnostic) git release script for tagging and bagging a release candidate with changelog text generation

[![Build Status](https://travis-ci.org/tommeier/releaseable.png)](https://travis-ci.org/tommeier/releaseable)

This project performs two simple tasks:

  * Generate a changelog based on git commits and tag with a custom tag prefix
  * Generate a changelog based on git commits at deploy time and tag with a custom deploy tag prefix

Everything is configurable via command line parameters. Run with help command (`-h`) to list all possible configurations.

## Dependencies

  * Git
  * Bash (Mac OSX / *nix )
  * Grep

## Installation

This is the base language agnostic script. Git clone this repo somewhere on your system and ensure the `releaseable` and `releaseable-deployed` bin files are accessible in the PATH.

## Looking for?

Additional implementations using this base script to wrap functionality and share across languages:

  * Ruby (coming soon)
  * Python (coming soon)
  * Node NPM (coming soon)

## Example flow

The usage flow for this app is as follows:

  * Run `releaseable` at the end of a release cycle defining whether it is a major, minor or patch release. This generates a `CHANGELOG` in the root of the projet, and a `VERSION` file with content. If you've released before, this will find the last release and generate changelog and version number accordingly. A tag will be generated for this release.

  * During the deploy of this release tag, run `releaseable-deployed` on successful deployment. This will generate a new changelog, compared to the last deploy. The reason for this is simple. You may create many releases, but only some versions hit different environments. For example, you create release 0.0.1, deploy to staging, fix many issues, then deploy to production with all the additional fixes. The changelog for staging, for each release, is very different to that of production with all changes grouped together.

## Version file

This file simple contains the raw version number (e.g. `1.4.21`). Useful for parsing in a deployed app to display the current version number.

## Changelog file

Generation is based on all git commits between a start and end point ordered by recency. With nothing provided, by default, the scripts will work out the last commit for a tag prefix, or last deploy and generate up to HEAD.

Developers that follow feature branches can pass an optional parameter to generate the changelog only on the pull requests merged in (the 'epics') providing a much cleaner list of content.

Tagged content of the changelog is limited at the moment, I'm looking at ways to make this dynamic. But right now, any commit with the following prefixes (ignoring spaces and case) will group the commits in the `CHANGELOG` and present under ordered headings:

   * `[feature]`
   * `[bug]`
   * `[security]`

## Deploy

After a deploy running `releaseable-deployed` with the release tag passed in provides the ability to generate the changelog based only on the last deploy. With a custom deploy prefix, for example `deployed/staging` you can scope the changelog to a given environment.

## Full examples
<!-- options:
  required:
    $(arg_for $ARG_VERSION '<version>')  set the software versioning type (major or minor or patch)
  optional:
    [$(arg_for $ARG_RELEASE_PREFIX '<prefix>')] set the release prefix (default: 'release/v')
    [$(arg_for $ARG_FORCE)]            force push of new tags (default: commit changes but do not push)
  changelog:
    [$(arg_for $ARG_START '<start>')]  set the start point (default: the last tag name that matches the prefix)
    [$(arg_for $ARG_FINISH '<finish>')] set the end/finish point (default: HEAD)
    [$(arg_for $ARG_APPEND)]            append to changelog (default: overwrite)
    [$(arg_for $ARG_PULL_REQUESTS)]            set to only pull requests (default: all commits)
    [$(arg_for $ARG_CHANGELOG '<changelog_file>')] set the changelog filename (default: CHANGELOG)
    [$(arg_for $ARG_VERSION_FILE '<version_file>')] set the version file name (default: VERSION)
  general:
    $(arg_for $ARG_HELP_TEXT)  show this help text
 -->

### Releaseable
(TODO)

Release with defaults (first time):
```
$> releaseable -v 'minor'
```
Generates:
  * version file: `0.1.0`
  * tag         : `release/v0.1.0`
  * Changelog   : all commits text up until now

---

Release with defaults (second time):
```
$> releaseable -v 'major'
```
Generates:
  * version file: `1.1.0`
  * tag         : `release/v1.1.0`
  * Changelog   : all commits between last release and this one

---

Release with pull requests for changelog:
```
$> releaseable -v 'major' -P
```
Generates:
  * version file: `1.1.0`
  * tag         : `release/v1.1.0`
  * Changelog   : all merged pull requests and the body text of the merge commit




## Example CHANGELOG output


## Contributing

All fork + pull requests welcome!

To run the tests locally:

```Bash

$> test/bin/run_all

```

## TODO

 - [ ] Add full examples to Readme
 - [ ] Create remaining TODO items as issues in Github
 - [ ] Test mode should display processes it would run (--dry-run option)
 - [ ] Change output of script to hide most output (unless dry run activated)
 - [ ] Create Ruby gem branch and release as a gem. Use better OPTPARSER to use more human readable variables to pass to - der lying  script
 - [ ] Create Node NPM branch and release as an NPM.
 - [ ] Create Python PIP branch and release as a PIP.
 - [ ] Test on variety of systems and servers
 - [ ] [potentially] Fix issue in tests where the time can very rarely cross over a second boundary (make specs ignore seconds difference)
 - [ ] [potentially] Change releaseable to work out prefix if given a start or an end point and no prefix
 - [ ] [potentially] Make test helpers for generating the content (changing the style now will break a lot of tests)
 - [ ] [potentially] Make CHANGELOG tagging dynamic (search for initial square brackets), with features + bugs on top of listing
 - [ ] [potentially] Make CHANGELOG generation read in optional template, with wildcards to apply logic to view
 - [ ] [potentially] Work out how to test git push being fired, mocking a git command
 - [ ] [potentially] Use an left pad command to align help text correctly
 - [*] Optionally force push of tags, otherwise ask for confirmation to send
 - [*] Remove the 'skip execute' code
 - [*] Create language maps (bash first, to make ruby only etc) for argument naming. Use everywhere in specs
 - [*] Make test helpers for setting argument mapping (for easily changing in other wrappers)
 - [*] Review argument naming and choose better letters
 - [*] Remove unnessary $(output)= statements in tests
 - [*] Update releaseable to remove the division of release and version prefix, make just one prefix (simpler)
 - [*] Write success cases for releaseable-deployed script
 - [*] Split up functions and specs into more logical divisions (changelog, git) rather than one large support file
 - [*] Remove *.sh filenames and rely on shebangs on each executable file. Support files keep for editors to use.
 - [*] Split into seperate github repo with migrated history
 - [*] Load Travis-CI





