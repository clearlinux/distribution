

## Clear Linux OS - Software Contribution Guide

Clear Linux OS is composed of many different open source software projects and welcomes all contributors to improve the project.

Before contributing, please review and abide by the [Code of Conduct](https://01.org/blogs/2018/intel-covenant-code).

Ensure any contributions align with Clear Linux philosophies on [stateless](https://clearlinux.org/features/stateless) and [security](https://clearlinux.org/documentation/clear-linux/concepts/security).

### Contributing to an existing software package

Before you start, consider if it would be better to contribute upstream.
Clear Linux will benefit from improvements to upstream software just
like everyone else. 

Note: for the Linux kernel package specifically, please see the [kernel development](https://clearlinux.org/documentation/clear-linux/guides/maintenance/kernel-development) documentation.

#### Clone the package repository 
 1. [Setup a development environment on a Clear Linux OS system](https://clearlinux.org/documentation/clear-linux/guides/maintenance/autospec#setup-environment-to-build-source).
 2. Identify and clone the repository corresponding to the software package by running `make clone_<PACKAGENAME>`. This will clone the repsoitory from https://github.com/clearlinux-pkgs which contain existing files and directories created by the [autospec](https://clearlinux.org/documentation/clear-linux/concepts/autospec-about) tooling.
 
#### Change compilation and packaging settings
If required, separately submit changes to a package's autospec configuration before submitting any source code changes so the Clear Linux release pipeline gets updated as expected.

 3. Modify the [autospec control files](https://github.com/clearlinux/autospec#control-files) to change compilation and packaging settings. 
 4. Create one or more patch files from the package's autospec directory using `git-format-patch` (see the [patching source code](#patching-source-code) section)
 5.  Email the patches for consideration to [dev@lists.clearlinux.org](mailto:dev@lists.clearlinux.org). 


#### Change the source code
 6. Modify the source code (see the [patching source code](#patching-source-code) section). 
 7. Run `make autospec` to regenerate the `.spec`file and build RPMs.
 8. Test the newly packaged software on a Clear Linux OS system.
 10. Email only the source code patch files for consideration to [dev@lists.clearlinux.org](mailto:dev@lists.clearlinux.org). Be sure to include the package name in the email subject and body.

If accepted, the Clear Linux team will update the package in Clear Linux OS and make the patches available at: https://github.com/clearlinux-pkgs 


### Contributing a new software package

Make sure the licensing of the package you want to include allows Clear Linux to package and distribute it. Licensing should use a common identifier and format as found at https://spdx.org/licenses/ and ideally be a permissive license. 

 1. [Setup a development environment on a Clear Linux OS system](https://clearlinux.org/documentation/clear-linux/guides/maintenance/autospec#setup-environment-to-build-source).
 2. Generate an autospec directory for the software package using the [autospec](https://clearlinux.org/documentation/clear-linux/guides/maintenance/autospec#example-2-build-a-new-rpm) tooling on Clear Linux.
 3. Create a new git repository in the folder containing the autospec package workspace.
 4. Create a single patch file using [`git-format-patch`](https://git-scm.com/docs/git-format-patch).[1]
 5. Email the patch that adds the suggested package for consideration to [dev@lists.clearlinux.org](mailto:dev@lists.clearlinux.org).
 6. Include suggestions for which bundle(s) the package should be added to and any ideas for tests that would be relevant.

If accepted, the Clear Linux team will add the package to Clear Linux OS and make the repository available at: https://github.com/clearlinux-pkgs 


### Contributing to Clear Linux development tooling

For projects under https://github.com/clearlinux, refer to the README and contribution guidelines in those repositories. 


## Patching source code


Source code from upstream software packages can be patched in different ways. As a developer, your choice to use one of these patching workflows depends on the existing methodology that is in use by the upstream project already, and the required change.

### Patching source using `git format-patch`

Using the `git format-patch` is the strongly preferred as patching workflow and
will increase the likelihood of patches being accepted.

1.  Run `make sources` to download the upstream software source code.
2. Extract the software source code archive. (`tar -xvf <PACKAGE>.tar.xz`)
3. Create a new local git repository and initialize. (`git init && git add -A && git commit -m "Initial commit"`)
4. Apply any existing patches from the package's autospec repository. ( `git am ../*.patch`)
5. Make any desired code modifications and track them with `git`. (`git add -A && git commit`)
6. Create one or more patch files. ([`git-format-patch -<N>`](https://git-scm.com/docs/git-format-patch))[1]
7. Copy the patch file(s) into the  package's autospec directory from step #2. (`cp *.patch ../`)
8. Add the patch file names to `series` file in the package's autospec directory. (`echo "0001-example-fix--for-bug.patch" >> series`)


### Patching source using `diff`

1.  Run `make sources` to download the upstream software source code.
2. Create a directory `a/`with the original source code.
   - Extract the software source code archive. (`tar -xvf <PACKAGE>.tar.xz`)
   - Apply any existing patches from the package's autospec repository. (`patch -p1 ../*.patch`)
3. Create a second directory `b/`with the new modifications. 
    - Extract the software source code archive. (`tar -xvf <PACKAGE>.tar.xz`)
    - Make any desired code modifications.
4. Run a `diff` between the two directories and create a patch file. (`diff -Nru a/ b/ > 0001-example-fix--for-bug.patch`)
5. Add the patch file names to `series` file in the package's autospec directory. (`echo "0001-example-fix--for-bug.patch" >> series`)


### Patching source using `quilt`

[`quilt`](http://savannah.nongnu.org/projects/quilt) is a patch management tool used by some software projects to manage many patches or patch sets. 

See the links below for more information on how `quilt` can be used: 
- http://www.shakthimaan.com/downloads/glv/quilt-tutorial/quilt-doc.pdf 
- https://wiki.debian.org/UsingQuilt 


## Reporting Bugs

Please submit any issues in upstream packages to their respective projects.
Bugs or issues related to Clear Linux Distribution may be submitted here: https://github.com/clearlinux/distribution/issues 


## Reporting Security Concerns

Visit https://01.org/security for best practices on reporting security issues.



[1] Make sure your [git user.name](https://help.github.com/en/articles/setting-your-username-in-git) and [git user.email](https://help.github.com/en/articles/setting-your-commit-email-address-in-git) are set correctly as they may appear in public patch files.
