# ![nf-core/tools](../docs/images/nfcore-tools_logo_light.png#gh-light-mode-only) ![nf-core/tools](../docs/images/nfcore-tools_logo_dark.png#gh-dark-mode-only) <!-- omit in toc -->

[![Python tests](https://github.com/nf-core/tools/workflows/Python%20tests/badge.svg?branch=master&event=push)](https://github.com/nf-core/tools/actions?query=workflow%3A%22Python+tests%22+branch%3Amaster)
[![codecov](https://codecov.io/gh/nf-core/tools/branch/master/graph/badge.svg)](https://codecov.io/gh/nf-core/tools)
[![code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)
[![code style: prettier](https://img.shields.io/badge/code%20style-prettier-ff69b4.svg)](https://github.com/prettier/prettier)
[![Imports: isort](https://img.shields.io/badge/%20imports-isort-%231674b1?style=flat&labelColor=ef8336)](https://pycqa.github.io/isort/)

[![install with Bioconda](https://img.shields.io/badge/install%20with-bioconda-brightgreen.svg)](https://bioconda.github.io/recipes/nf-core/README.html)
[![install with PyPI](https://img.shields.io/badge/install%20with-PyPI-blue.svg)](https://pypi.org/project/nf-core/)
[![Get help on Slack](http://img.shields.io/badge/slack-nf--core%20%23tools-4A154B?logo=slack)](https://nfcore.slack.com/channels/tools)

A python package with helper tools for the nf-core community.

> **Read this documentation on the nf-core website: [https://nf-co.re/tools](https://nf-co.re/tools)**

## Table of contents <!-- omit in toc -->

- [`nf-core` tools installation](#installation)
- [`nf-core` tools update](#update-tools)
- [`nf-core list` - List available pipelines](#listing-pipelines)
- [`nf-core launch` - Run a pipeline with interactive parameter prompts](#launch-a-pipeline)
- [`nf-core download` - Download pipeline for offline use](#downloading-pipelines-for-offline-use)
- [`nf-core licences` - List software licences in a pipeline](#pipeline-software-licences)
- [`nf-core create` - Create a new pipeline with the nf-core template](#creating-a-new-pipeline)
- [`nf-core lint` - Check pipeline code against nf-core guidelines](#linting-a-workflow)
- [`nf-core schema` - Work with pipeline schema files](#pipeline-schema)
- [`nf-core bump-version` - Update nf-core pipeline version number](#bumping-a-pipeline-version-number)
- [`nf-core sync` - Synchronise pipeline TEMPLATE branches](#sync-a-pipeline-with-the-template)
- [`nf-core modules` - commands for dealing with DSL2 modules](#modules)

  - [`modules list` - List available modules](#list-modules)
    - [`modules list remote` - List remote modules](#list-remote-modules)
    - [`modules list local` - List installed modules](#list-installed-modules)
  - [`modules info` - Show information about a module](#show-information-about-a-module)
  - [`modules install` - Install modules in a pipeline](#install-modules-in-a-pipeline)
  - [`modules update` - Update modules in a pipeline](#update-modules-in-a-pipeline)
  - [`modules remove` - Remove a module from a pipeline](#remove-a-module-from-a-pipeline)
  - [`modules patch` - Create a patch file for a module](#create-a-patch-file-for-a-module)
  - [`modules create` - Create a module from the template](#create-a-new-module)
  - [`modules create-test-yml` - Create the `test.yml` file for a module](#create-a-module-test-config-file)
  - [`modules lint` - Check a module against nf-core guidelines](#check-a-module-against-nf-core-guidelines)
  - [`modules test` - Run the tests for a module](#run-the-tests-for-a-module-using-pytest)
  - [`modules bump-versions` - Bump software versions of modules](#bump-bioconda-and-container-versions-of-modules-in)
  - [`modules mulled` - Generate the name for a multi-tool container image](#generate-the-name-for-a-multi-tool-container-image)

- [`nf-core subworkflows` - commands for dealing with subworkflows](#subworkflows)
  - [`subworkflows list` - List available subworkflows](#list-subworkflows)
    - [`subworkflows list remote` - List remote subworkflows](#list-remote-subworkflows)
    - [`subworkflows list local` - List installed subworkflows](#list-installed-subworkflows)
  - [`subworkflows info` - Show information about a subworkflow](#show-information-about-a-subworkflow)
  - [`subworkflows install` - Install subworkflows in a pipeline](#install-subworkflows-in-a-pipeline)
  - [`subworkflows update` - Update subworkflows in a pipeline](#update-subworkflows-in-a-pipeline)
  - [`subworkflows remove` - Remove a subworkflow from a pipeline](#remove-a-subworkflow-from-a-pipeline)
  - [`subworkflows create` - Create a subworkflow from the template](#create-a-new-subworkflow)
  - [`subworkflows create-test-yml` - Create the `test.yml` file for a subworkflow](#create-a-subworkflow-test-config-file)
  - [`subworkflows test` - Run the tests for a subworkflow](#run-the-tests-for-a-subworkflow-using-pytest)
- [Citation](#citation)

The nf-core tools package is written in Python and can be imported and used within other packages.
For documentation of the internal Python functions, please refer to the [Tools Python API docs](https://nf-co.re/tools-docs/).

## Update tools

It is advisable to keep nf-core/tools updated to the most recent version. The command to update depends on the system used to install it, for example if you have installed it with conda you can use:

```bash
conda update nf-core
```

if you used pip:

```bash
pip install --upgrade nf-core
```

Please refer to the respective documentation for further details to manage packages, as for example [conda](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-pkgs.html#updating-packages) or [pip](https://packaging.python.org/en/latest/tutorials/installing-packages/#upgrading-packages).

### Activate shell completions for nf-core/tools

Auto-completion for the `nf-core` command is available for bash, zsh and fish. To activate it, add the following lines to the respective shell config files.

| shell | shell config file                         | command                                            |
| ----- | ----------------------------------------- | -------------------------------------------------- |
| bash  | `~/.bashrc`                               | `eval "$(_NF_CORE_COMPLETE=bash_source nf-core)"`  |
| zsh   | `~/.zshrc`                                | `eval "$(_NF_CORE_COMPLETE=zsh_source nf-core)"`   |
| fish  | `~/.config/fish/completions/nf-core.fish` | `eval (env _NF_CORE_COMPLETE=fish_source nf-core)` |

After a restart of the shell session you should have auto-completion for the `nf-core` command and all its sub-commands and options.

> **NB:** The added line will run the command `nf-core` (which will also slow down startup time of your shell). You should therefore either have the nf-core/tools installed globally.
> You can also wrap it inside `if type nf-core > /dev/null; then ` \<YOUR EVAL CODE LINE\> `fi` for bash and zsh or `if command -v nf-core &> /dev/null eval (env _NF_CORE_COMPLETE=fish_source nf-core) end` for fish. You need to then source the config in your environment for the completions to be activated.

> **NB:** If you see the error `command not found compdef` , be sure that your config file contains the line `autoload -Uz compinit && compinit` before the eval line.