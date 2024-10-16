.. _overview:

======================
    tmt
======================

Test Management Tool


Description
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The ``tmt`` python module and command-line tool implement the
Metadata Specification which allows storing all needed test
execution data directly within a git repository. In this way, it
makes testing independent on any external test management system.

The Flexible Metadata Format ``fmf`` is used to store data in both
human and machine readable way close to the source code. Thanks to
inheritance and elasticity metadata are organized in the structure
efficiently, preventing unnecessary duplication.

The tool provides a user-friendly way to create, debug and easily
run tests from your laptop across different environments. It also
allows to easily convert old metadata, list and filter available
tests and verify them against the L1 specification.

Plans are used to group tests and configure individual test steps
defined by the L2 specification. They describe how to select tests
for execution, how to provision the environment, how to prepare it
for testing or how the test results should be reported.

Stories, defined by the L3 specification, can be used to track
implementation, test and documentation coverage for individual
features or requirements. Thanks to this you can track everything
in one place, including the project implementation progress.


Synopsis
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Command line usage is straightforward::

    tmt command [options]


Examples
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Let's see which tests, plans and stories are available::

    tmt

Initialize the metadata tree in the current directory, optionally
with example content based on templates::

    tmt init
    tmt init --template base

Run all or selected steps for each plan::

    tmt run
    tmt run discover
    tmt run prepare execute

List tests, show details, check against the specification::

    tmt test ls
    tmt test show
    tmt test lint

Create a new test, import test metadata from other formats::

    tmt test create
    tmt test import

List plans, show details, check against the specification::

    tmt plan ls
    tmt plan show
    tmt plan lint

List stories, check details, show coverage status::

    tmt story ls
    tmt story show
    tmt story coverage

Many commands support regular expression filtering and other
specific options::

    tmt story ls cli
    tmt story show create
    tmt story coverage --implemented

Check help message of individual commands for the full list of
available options.


Options
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Here is the list of the most frequently used commands and options.

Run
---

The `run` command is used to execute test steps. By default all
test steps are run. See the L2 Metadata specification for detailed
description of individual steps. Here is a brief overview:

discover
    Gather information about test cases to be executed.

provision
    Provision an environment for testing or use localhost.

prepare
    Prepare the environment for testing.

execute
    Run tests using the specified executor.

report
    Provide test results overview and send reports.

finish
    Perform the finishing tasks and clean up provisioned guests.


Test
----

Manage tests (L1 metadata). Check available tests, inspect their
metadata, gather old metadata from various sources and stored them
in the new fmf format.

ls
    List available tests.
show
    Show test details.
lint
    Check tests against the L1 metadata specification.
create
    Create a new test based on given template.
import
    Convert old test metadata into the new fmf format.


Plan
----

Manage test plans (L2 metadata). Search for available plans.
Explore detailed test step configuration.

ls
    List available plans.
show
    Show plan details.
lint
    Check plans against the L2 metadata specification.


Story
-----

Manage user stories. Check available user stories. Explore
coverage (test, implementation, documentation).

ls
    List available stories.
show
    Show story details.
coverage
    Show code, test and docs coverage for given stories.
export
    Export selected stories into desired format.


Utils
-----

Various utility options.

--root PATH
    Path to the metadata tree, current directory used by default.

--verbose
    Print additional information.

--debug
    Turn on debugging output.

Check help message of individual commands for the full list of
available options.


.. _install:

Install
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The main ``tmt`` package provides the core features with a minimal
set of dependencies::

    sudo dnf install tmt

In order to enable additional functionality, such as particular
provision or report plugins, install the respective subpackage::

    sudo dnf install tmt-test-convert
    sudo dnf install tmt-report-html
    sudo dnf install tmt-provision-container
    sudo dnf install tmt-provision-virtual

If you don't care about disk space and want to have all available
features right at hand install everything::

    sudo dnf install tmt-all

For RHEL 8 and CentOS 8, first make sure that you have enabled the
EPEL repository::

    sudo dnf install epel-release
    sudo dnf install tmt

Impatient to try the fresh features as soon as possible? Install
the latest greatest version from the ``copr`` repository::

    sudo dnf copr enable psss/tmt
    sudo dnf install tmt

Not sure, just want to try out how it works? Experiment safely and
easily inside a container::

    podman run -it --rm quay.io/testing-farm/tmt bash
    podman run -it --rm quay.io/testing-farm/tmt-all bash

When installing using ``pip`` you might need to install additional
packages on your system::

    sudo dnf install gcc {python3,libvirt,krb5,libpq}-devel
    pip install --user tmt

Note: You can omit the ``--user`` flag if in a virtual environment.


Shell Completion
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The rpm package includes a system wide script which enables the
command line completion for ``bash`` so no additional config
should be needed. If you use a different installation method or
prefer another shell, see the instructions below.

For Bash, add this to ``~/.bashrc``::

    eval "$(_TMT_COMPLETE=source_bash tmt)"

For Zsh, add this to ``~/.zshrc``::

    eval "$(_TMT_COMPLETE=source_zsh tmt)"

For Fish, add this to ``~/.config/fish/completions/tmt.fish``::

    eval (env _TMT_COMPLETE=source_fish tmt)

Open a new shell to enable completion. Or run the ``eval`` command
directly in your current shell to enable it temporarily.


Develop
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In order to experiment, play with the latest bits and develop
improvements it is best to use a virtual environment::

    mkvirtualenv tmt
    git clone https://github.com/psss/tmt
    cd tmt
    pip install -e .

Install ``python3-virtualenvwrapper`` to easily create and enable
virtual environments using ``mkvirtualenv`` and ``workon``. Note
that if you have freshly installed the package you need to open a
new shell session to enable the wrapper functions.

The main ``tmt`` package contains only the core dependencies. For
building documentation, testing changes, importing/exporting test
cases or advanced provisioning options install the extra deps::

    pip install '.[docs]'
    pip install '.[tests]'
    pip install '.[convert]'
    pip install '.[provision]'

Or simply install all extra dependencies to make sure you have
everything needed for the tmt development ready on your system::

    pip install '.[all]'


Exit Codes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The following exit codes are returned from ``tmt run``. Note that
you can use the ``--quiet`` option to completely disable output
and only check for the exit code.

0
    At least one test passed, there was no fail, warn or error.
1
    There was a fail or warn identified, but no error.
2
    Errors occured during test execution.
3
    No test results found.


Variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The following environment variables can be used to modify
behaviour of the ``tmt`` command.

TMT_DEBUG
    Enable the desired debug level. Most of the commands support
    levels from 1 to 3. However, some of the plugins go even
    deeper when needed.

NO_COLOR
    Disable colors in the terminal output. Output only plain,
    non-colored text. See https://no-color.org/ for more
    information.


Links
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Git:
https://github.com/psss/tmt

Docs:
http://tmt.readthedocs.io/

Stories:
https://tmt.readthedocs.io/en/latest/stories.html

Issues:
https://github.com/psss/tmt/issues

Releases:
https://github.com/psss/tmt/releases

Copr:
http://copr.fedoraproject.org/coprs/psss/tmt

PIP:
https://pypi.org/project/tmt/

Travis:
https://travis-ci.org/psss/tmt

Coveralls:
https://coveralls.io/github/psss/tmt

Metadata Specification:
https://tmt.readthedocs.io/en/latest/spec.html

Flexible Metadata Format:
http://fmf.readthedocs.io/

Packit & Testing Farm:
https://packit.dev/testing-farm/


Authors
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Petr Šplíchal, Miro Hrončok, Alexander Sosedkin, Lukáš Zachar,
Petr Menšík, Leoš Pol, Miroslav Vadkerti, Pavel Valena, Jakub
Heger, Honza Horák, Rachel Sibley, František Nečas, Michal
Ruprich, Martin Kyral, Miloš Prchlík, Tomáš Navrátil, František
Lachman, Patrik Kis, Ondrej Mosnáček, Andrea Ficková, Denis
Karpelevich, Michal Srb, Jan Ščotka, Artem Zhukov and Vinzenz
Feenstra.


Copyright
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Copyright (c) 2019 Red Hat, Inc.

This program is free software; you can redistribute it and/or
modify it under the terms of the MIT License.
