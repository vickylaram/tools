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

## Launch a pipeline

Some nextflow pipelines have a considerable number of command line flags that can be used.
To help with this, you can use the `nf-core launch` command.
You can choose between a web-based graphical interface or an interactive command-line wizard tool to enter the pipeline parameters for your run.
Both interfaces show documentation alongside each parameter and validate your inputs.

The tool uses the `nextflow_schema.json` file from a pipeline to give parameter descriptions, defaults and grouping.
If no file for the pipeline is found, one will be automatically generated at runtime.

Nextflow `params` variables are saved in to a JSON file called `nf-params.json` and used by nextflow with the `-params-file` flag.
This makes it easier to reuse these in the future.

The command takes one argument - either the name of an nf-core pipeline which will be pulled automatically,
or the path to a directory containing a Nextflow pipeline _(can be any pipeline, doesn't have to be nf-core)_.

<!-- RICH-CODEX trim_after: "Command line" -->

![`nf-core launch rnaseq -r 3.8.1`](docs/images/nf-core-launch-rnaseq.svg)

Once complete, the wizard will ask you if you want to launch the Nextflow run.
If not, you can copy and paste the Nextflow command with the `nf-params.json` file of your inputs.

```console
INFO     [✓] Input parameters look valid
INFO     Nextflow command:
         nextflow run nf-core/rnaseq -params-file "nf-params.json"


Do you want to run this command now?  [y/n]:
```

### Launch tool options

- `-r`, `--revision`
  - Specify a pipeline release (or branch / git commit sha) of the project to run
- `-i`, `--id`
  - You can use the web GUI for nf-core pipelines by clicking _"Launch"_ on the website. Once filled in you will be given an ID to use with this command which is used to retrieve your inputs.
- `-c`, `--command-only`
  - If you prefer not to save your inputs in a JSON file and use `-params-file`, this option will specify all entered params directly in the nextflow command.
- `-p`, `--params-in PATH`
  - To use values entered in a previous pipeline run, you can supply the `nf-params.json` file previously generated.
  - This will overwrite the pipeline schema defaults before the wizard is launched.
- `-o`, `--params-out PATH`
  - Path to save parameters JSON file to. (Default: `nf-params.json`)
- `-a`, `--save-all`
  - Without this option the pipeline will ignore any values that match the pipeline schema defaults.
  - This option saves _all_ parameters found to the JSON file.
- `-h`, `--show-hidden`
  - A pipeline JSON schema can define some parameters as 'hidden' if they are rarely used or for internal pipeline use only.
  - This option forces the wizard to show all parameters, including those labelled as 'hidden'.
- `--url`
  - Change the URL used for the graphical interface, useful for development work on the website.