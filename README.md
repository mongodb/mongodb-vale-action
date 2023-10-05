# Vale Styles and GitHub Action for the TDBX Team

This directory contains the current set of [vale](https://github.com/errata-ai/vale) rules
that the Driver Docs team uses to avoid breaking rules defined in the
[MongoDB Documentation Style Guide](https://www.mongodb.com/docs/meta/style-guide/).
These rules represent a subset of the rules that the team should follow,
 and have been selected due to their low rate of false positives.

Additional existing and new rules should be added after we determine that
they effectively identify style guide issues and generate low numbers of
false positives.

:warning: Changes between rulesets:
> Although the rules share similar names and directory structure to the
> ones in the ``main`` branch of ``mongodb-vale``, the logic contained within
> them varies due to changes in severity levels and improvements in identifying
> issues. These were modified to simplify the TDBX workflow.

Continue reading to see how to install the GitHub action and how to
install the rules for running vale on the CLI.

# GitHub Action

## Install

1.  To install the GitHub action into a repository, copy the GitHub action
file ``vale-tdbx.yml`` into the ``.github/workflows/`` directory in your
documentation repository.

2.  Follow your team's GitHub workflow to merge this change into the ``main`` or
``master`` branch.

After you merge these changes into the ``main`` or ``master`` branch, the
action should automatically run whenever you commit new files. You can
verify that the action is added by clicking the **Actions** tab and
confirming that "vale-checks" appears under "All workflows" in the left
pane.

If successfully added, this action runs on commits to forks owned by
authorized contributors of that docs repository.

## Check the Output

The output shows you the files you changed in the pull request (PR) and any
rule violations that ``vale`` identifies.

The GitHub action reports any issues that ``vale`` or the GitHub action
encountered in the following ways:

- Adds comments to your PR when your additions or changes include style guide
issues.

- Displays a red "X" next to your commits if either GitHub action failed for
an unexpected reason or if any style guide issues were found in your PR.

- Logs errors in the **Actions** tab of the docs repo. You need to find and
select the job that corresponds to the one you recently committed. To view
this information from this view, click the **tdbx-vale** step of the job and
then the arrow next to the text "Running vale with reviewdog 🐶 ..."

An error should look something like this:

```
{"message": "[MongoDB.AvoidSubjunctive] Avoid the subjunctive 'should'.", "location": {"path": "source/whats-new.txt", "range": {"start": {"line": 73, "column": 63}}}, "severity": "ERROR"}
```

The ``message`` field identifies the rule that the file violates.

The ``path`` field identifies the file in which the error occurs
and the ``range`` field identifies the specific part of the file.

Run your local copy of ``vale`` to verify the result.
Fix the errors and push a new commit to run the action again.

## GitHub Action Dependencies

This action currently depends on the following GitHub actions:

- [actions/checkout@master](https://github.com/actions/checkout)
- [masesgroup/retrieve-changed-files@v2](https://github.com/masesgroup/retrieve-changed-files/releases/tag/v2)
- [vale-action@reviewdog](https://github.com/errata-ai/vale-action)
- Custom logic that copies files from this branch of the repository.

# Install Vale Rules for the CLI

1. [Install vale](https://vale.sh/docs/vale-cli/installation/](https://github.com/10gen/mongodb-vale/tree/main).

2. Install the ``vale`` rules and ``vale.ini`` configuration file.
The most convenient way to keep your ``vale`` binary configured with
the same rules as this action is to fork and clone this repository,
and reference the file paths in your vale configuration.

One way to accomplish this is to create an alias that points to
your installation of ``vale`` and to pass it the location
of the ``vale.ini`` file contained in this repository. If you use
the current GitHub repository file structure, you can declare the
following alias in your shell resource file (e.g. ``.zshrc``):

```bash
alias vale-tdbx="vale --config='/path/to/repo/.vale.ini"
```

After sourcing the change to your shell or starting a new one, you should be
able to run the command on a file or directory:

```sh
vale-tdbx <path to file or directory>
```

:warning: The ``vale.ini`` file is customized in this branch
> You must be using this branch of the repo in order for ``vale`` to
> process with the TDBX ruleset. Other branches may not include this file
> or contain a different version, which could cause unexpected results or
> errors.

The ``.vale.ini`` file expects a relative file structure that
references the location of the ``yml`` rule files.
The files are structured to match the ``vale-action@reviewdog`` GitHub
action recommended [Repository Structure](https://github.com/errata-ai/vale-action/blob/reviewdog/README.md#repository-structure).

If you need to run ``vale`` from a configuration file in a different
location, make sure that it references the correct set of rules.

If you run ``vale`` with a different set of rules, you can create
a separate alias that references a different ``vale.ini`` file.
