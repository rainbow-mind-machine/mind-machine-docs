# Developer Notes

## Makefiles

The Makefile in each mind machine repository provides rules to help you
perform common tasks. There are three types of tasks:

1. Makefile rules run after the _first_ clone of a _brand-new_ mind machine repo
1. Makefile rules run after a fresh clone of an existing mind machine repo
1. Makefile rules for testing, creating, deploying


## The Prime Number Version System

Crash course in the prime number version system:

Branches:

* `master` - contains stable, tagged releases. Each release is a unique prime
  number, occurring in ascending order.

* `dev` - development branch. All work happens here.

* `feature` - feature branch. Feature branches are created from the dev branch,
  and commits from these feature branches are merged back into the dev branch
  before they are ultimately added to a mind machine release.

* `release/vP` - branch preparing a release of version P. Changes on release
  branches are almost always synchronized with the development branch.

Workflow:

* Start from a stable version on master
* Create branch dev from master (or pull from master into dev if it already exists)
* Create branch feature from dev (feature branch contains all work on a particular feature)
* When ready, merge changes from feature into dev
* Once dev branch is ready for a release, create a release candidate branch
  `release/vP`

## Testing

 * Using examples to test interactivity
 * Tests are mainly smoke tests
 * For complicated example, see bear/python-twitter on Github

## Release Process

See branching workflow above. Checklist:

* Test examples
* Test create Docker container
* Test documentation (but documentation can be/should be separate)
* Update submodules
* Mainly code

When ready:

* Pypi upload

How to set up:

* Dockerhub (update with new versions)

## Docker

 * Developer considerations for docker container


