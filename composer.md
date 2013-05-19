# Composer notes

Some notes in preparation for a talk on Composer.

# Intro

## What is Composer?

* One-line definition
* Compare to PEAR, svn externals, git submodules
* Other languages have had this for awhile: npm (Node), pip (Python), bundler (Ruby)
  Composer is pretty new. (Since when?)

## Installing

## Use cases

Two main use cases:

* managing dependencies in a project 
* distributing a library

# Managing dependencies

## Simple example of getting a dependency package

## Specifying package versions

## Bootstrapping projects

## Adding more dependencies

## Finding packages

## Autoloading

## System dependencies

## composer.lock

# Distributing a library

See https://github.com/composer/composer/blob/master/doc/02-libraries.md

## Create a composer.json for the package

First create a repo for the package.

Then add a composer.json in the root directory:

    {
        "name": "jasongrimes/silex-simpleuser",
        "type": "library",
        "description": "A simple database-backed user provider for Silex, with associated services and controllers.",
        "keywords": ["silex", "user", "user provider", "simple user provider"],
        "homepage": "http://github.com/jasongrimes/silex-simpleuser",
        "license": "MIT",
        "authors": [
            {
                "name": "Jason Grimes",
                "email": "jason@grimesit.com"
            }
        ],
        "require": {
            "php": ">=5.3.0"
        },
        "autoload": {
            "psr-0": {"SimpleUser": "src/"}
        }
    }

## Tag release versions

When publishing the package on packagist, it can infer the version information from the VCS (i.e. git) by looking at tags and branches.

For every tag that looks like a version, a package version of that tag will be created. The pattern must be `X.Y.Z` or `vX.Y.Z` with an optional suffix for RC, beta, alpha, or patch.

Examples of valid tag names:

    1.0.0
    v1.0.0
    1.10.5-RC1
    v4.4.4beta2
    v2.0.0-alpha
    v2.0.4-p1

For every branch, a package *development* version will be created.

If the branch name looks like a version, the version will be ``{branchname}-dev``. For example a branch `2.0` will get a version `2.0.x-dev`. 
If the branch does not look like a version, it will be `dev-{branchname}`. `master` results in a `dev-master` version.

Examples of version branch names:

    1.x
    1.0 (equals 1.0.x)
    1.1.x

You can also alias branch names to version numbers, so they can be matched when comparing version numbers. See https://github.com/composer/composer/blob/master/doc/articles/aliases.md

## Executing scripts with composer



## Custom repositories

Other projects can require the new package before it has been added to packagist by manually defining the repository in the dependent project's composer.json.
Require the "dev-master" version to get the latest sources from the master branch. 

    {
        "repositories": [
            {
                "type": "vcs",
                "url": "http://github.com/jasongrimes/silex-simpleuser"
            }
        ],
        "require": {
            "jasongrimes/silex-simpleuser": "dev-master"
        }
    }

Note: When developing a package alongside another project, it can be easier to use the local sources instead of updating the composer package between changes.
To use the local package sources, remove the package's "require" line from composer.json, and add a line to the "autoload" section pointing to the local package sources.

    "autoload": {
        "psr-0": {
            "SimpleUser": "../../silex-simpleuser/src"
        }
    }

Then regenerate the autoloader configuration with `composer dump-autoload`

### Submitting the package to packagist

Submit the URL of the package's public repository at https://packagist.org/packages/submit 

Set up a github service hook to have the package automatically updated whenever you push, instead of being crawled only once per day.
In your GitHub repository, click the "Settings" button, then "Service Hooks". 
Pick "Packagist" in the list, and add the API key from your packagist profile, plus your Packagist username if it is not the same as on GitHub.
 Check the "Active" box and submit the form.

The search index is updated every five minutes.

