# Which Manifest Do I Use?

## Released Versions of Couchbase Server

When we make a release, we take the manifest emitted from the builder
and store it in the released/ directory.  This manifest only has exact
commit SHAs, so that it explicitly describes which revision was used,
in both Couchbase and external repositories.

It also gives the revision of the "voltron" repo used in the build.
Voltron contains build instructions --- like RPM spec files -- that
are used at the top level before the manifest is used to fetch files
from the source repos.  Because the voltron repo is private, and is
outside the scope of the "repo" tool, it is included in released/
manifests as a comment.

To replicate a released build use a manifest from the released/
directory.

## Versions of Couchbase Server Prior to Release

If you want to build the development branch you should use
"branch-master.xml".

While preparing for a product release, we build using one of the
manifests in the top-level directory. Prior to 2.2.0 the files were
called "branch-_branch-name_.xml", and starting with 2.2.0 we used
files called "rel-_release-name_.xml"

This was to signify a change in process, in which stopped making
release-specific branches (named for the release), unless a such
branch was needed (and is no longer named for the release).  Thus we
had:

          branch-1.8.1.xml
          branch-2.0.1.xml
          branch-2.0.xml
          branch-2.1.0.xml

And going forward we have:

          rel-2.1.1.xml
          rel-2.2.0.xml
          rel-2.2.1.xml
          rel-3.0.0.xml

You will not need to use any of these manifests unless you are
contributing changes towards a Couchbase release.

## Couchbase Experimental Builds

The toy/ directory is used by Couchbase developers for experimental builds,
and so are probably not of interest to anyone not familiar with the context
of the experiment.


# Building With Repo

## Ensure You Have the Dependencies

The list of dependencies differ between the versions you try to build.

### Mac OS X:

Using [homebrew][homebrew] and the ruby that ships with a recent OS X,
you can easily install the dependencies using the following commands:

    sudo easy_install -U pyrex
    brew install bazaar --system
    brew install libevent
    brew install gnupg
    brew install v8
    brew install snappy
    brew install erlang
    brew install icu4c
    brew install automake
    brew install libtool
    brew install google-perftools
    brew install cmake

Make sure that icu's `icu-config` binary is on your PATH when building
couchbase:

    export PATH=/usr/local/opt/icu4c/bin:$PATH

Optionally, you can install repo from homebrew as well:

    brew install repo

### Debian/Ubuntu:

The following works for a clean Debian stable (squeeze) installation (under root):

    aptitude install -y --without-recommends build-essential automake libtool pkg-config check libssl-dev sqlite3 libevent-dev libglib2.0-dev libcurl4-openssl-dev erlang-nox curl erlang-dev erlang-src ruby libmozjs-dev libicu-dev icu-config libsnappy-dev libcurl4-openssl-dev cloog-ppl libv8-dev
    aptitude install -y python-minimal
    aptitude install -y --without-recommends git-core

Note that Debian squeeze ships Erlang R14A, yet Ubuntu 10.4, 10.10 and
even 11.4 ship R13B03. As of this writing, couchbase requires R14B.

To install R14 on Ubuntu, you can grab R14B source package from Debian
Unstable and dpkg-buildpackage'ing it as usual.

Another (any likely preferred) option is to get R14B02 from PPA:
https://launchpad.net/~scattino/+archive/ppa

In order to link with xulrunner on ubuntu (which lacks libmozjs-dev)
you need the following:

    aptitude install -y xulrunner-dev

and then you need to pass extra options to make like this:

    make couchdb_EXTRA_OPTIONS='--with-js-include=/usr/include/xulrunner-1.9.2.16 --with-js-lib=/usr/lib/xulrunner-devel-1.9.2.16/sdk/lib/'

The path seems to vary with version. 'dpkg -l xulrunner-dev' will help
you find the right path.

## Get Repo

(if you didn't install from homebrew, or aren't running on Mac OS X)

Get the latest version from [the google project
page](http://code.google.com/p/git-repo/downloads/list).

## Clone the Manifest

For `<branch_name>` below, you probably want to one of the latest
branches in released when getting started.  As of this writing,
that is `released/2.2.0.xml`
unless you are working on a maintenance or experimental branch.

    $ mkdir couchbase
    $ cd couchbase
    $ repo init -u git://github.com/couchbase/manifest.git -m <branch_name>
    $ repo sync

## Build

    $ make

[homebrew]: https://github.com/mxcl/homebrew
