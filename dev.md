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

## Release Process

The following checklists help in preparing for releases
on PyPI or Dockerhub. Releases happen in a two-step process.

### Pre-Release Checklist

A new release happens when the code is stable. The first step
in the release process is to create a pre-release branch, where
the code base can be modified but the modifications will
generally be specific to the version being released. (For example,
updating the version number in `setup.py`).

Pre-release checklist:

* `setup.py` tasks:
    * Does `setup.py build` work without errors?
    * Does `setup.py install` work without errors?
    * Has the description in `setup.py` been defined and updated?
    * Have the requirements in `setup.py` been changed, and do they 
      match the requirements in `requirements.txt`?
    * Does this mind machine properly depend on the correct, new 
      version of boring mind machine?
    * Has the version number in `setup.py` been bumped?
* Documentation tasks:
    * Has the version number in the documentation (shield on 
      `docs/index.md` page) been updated?
    * Has Readme ben updated/quickstart instructions tested?
* Git tasks:
    * Is the submodule using HTTPS? (should not use SSH)
    * Have submodules been updated? 
    * `git submodule foreach git checkout master; git submodule foreach git pull origin master`
    * Does this release include any large files? Are they necessary?
      Can they be slimmed?
* Docker tasks:
    * Is the Dockerfile cloning the repo from the correct URL?
    * Can a simple (non-mind-machine) python-alpine Dockerfile
      install the requirements in `requirements.txt`?

### Tests Checklist

The second step is to go through the build, run, and test process.

Test checklist:

* Nose tests
    * Does `python setup.py test` pass?
* Docker
    * Can the Docker container be created using the shell script?
    * Does `python setup.py test` pass inside the container?
* Documentation
    * Does `mkdocs build` work?
    * Is updated documentation ready to deploy?

### Create Release Branch

Once the release checklist has been completed and everything is passing
tests, the prerelease branch should be made into a release branch.

This can be done by renaming the prerelease branch,

```
git branch -m prereleases/vXYZ releases/vXYZ
```

or by creating a new releases branch from the prerelases branch:

```
git branch releases/vXYZ
git checkout releases/vXYZ
```

### Create Tags

After the new releases branch is created, a tag should be created
that points to the head commit of the release branch.

Create a git tag:

```
git tag vXYZ.0
git push origin vXYZ.0
```

This will add the new version tag to the "Releases" page of 
the package's Github repository

### Testing Release from Github

Test downloading anad installing the new release from Github:

```
wget https://github.com/rainbow-mind-machine/boring-mind-machine/archive/vXYZ.zip
unzip vXYZ.zip

...see installation instructions...
```

### Set Up PyPI Account

Ensure you have a PyPI account before you upload the new release to 
PyPI and make it pip-installable. To set up an account on PyPI:

```
python setup.py register
```

add the following to ~/.pypirc:

**~/.pypirc**:

```
[distutils]
index-servers =
    pypi

[pypi]
username:charlesreid1
password:YOURPASSWORDHERE
```

### Upload Release to PyPI

PyPI = Python Package Index (where pip looks by default)

When ready, the steps to release on PyPI are as follows:

* Create a distribution locally
* Upload the distribution to PyPI
* Test the new release in a virtual environment

Start by making a distribution package bundle:

```
$ python setup.py sdist
```

Upload it to pypi:

```
$ python setup.py sdist upload
```

Test it out with virutalenv:

```
  $ virtualenv vp && cd vp
  $ source bin/activate
  $ bin/pip install boringmindmachine
```

### Set Up Dockerhub Account

Before updating the latest container image available for the project on Dockerhub,
you should create a Dockerhub account. There is no command line interface needed,
as Dockerhub builds are configured via the web interface and webhooks.

### Update Container Image on Dockerhub

To update the latest container image available for the project on Dockerhub,
add the new version's tag to the list of versions that Dockerhub will build.
You can also configure webhooks so that new images are built whenever a
particular branch is updated, but tagged versions won't change much so they
only require building once.

## Useful Links

Guide to packaging a minimal Python application:

* <http://python-packaging.readthedocs.io/en/latest/minimal.html>

Uploading distributions to PyPI:

* <https://packaging.python.org/guides/migrating-to-pypi-org/#uploading>

Example scripts and guide:

* <https://gist.github.com/gboeing/dcfaf5e13fad16fc500717a3a324ec17>

