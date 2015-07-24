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

See the readme in the correct branch for
[tlm](https://github.com/couchbase/tlm) for the exact steps on how to
build the desired version.

## Get Repo

(if you didn't install from homebrew, or aren't running on Mac OS X)

Get the latest version from [the google project
page](https://storage.googleapis.com/git-repo-downloads/repo).

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
