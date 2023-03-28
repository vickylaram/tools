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

## Pipeline schema

nf-core pipelines have a `nextflow_schema.json` file in their root which describes the different parameters used by the workflow.
These files allow automated validation of inputs when running the pipeline, are used to generate command line help and can be used to build interfaces to launch pipelines.
Pipeline schema files are built according to the [JSONSchema specification](https://json-schema.org/) (Draft 7).

To help developers working with pipeline schema, nf-core tools has three `schema` sub-commands:

- `nf-core schema validate`
- `nf-core schema build`
- `nf-core schema docs`
- `nf-core schema lint`

### Validate pipeline parameters

Nextflow can take input parameters in a JSON or YAML file when running a pipeline using the `-params-file` option.
This command validates such a file against the pipeline schema.

Usage is `nf-core schema validate <pipeline> <parameter file>`. eg with the pipeline downloaded [above](#download-pipeline), you can run:

<!-- RICH-CODEX
working_dir: tmp
before_command: 'echo "{input: myfiles.csv, outdir: results}" > nf-params.json'
timeout: 10
after_command: rm nf-params.json
-->

![`nf-core schema validate nf-core-rnaseq/workflow nf-params.json`](docs/images/nf-core-schema-validate.svg)

The `pipeline` option can be a directory containing a pipeline, a path to a schema file or the name of an nf-core pipeline (which will be downloaded using `nextflow pull`).

### Build a pipeline schema

Manually building JSONSchema documents is not trivial and can be very error prone.
Instead, the `nf-core schema build` command collects your pipeline parameters and gives interactive prompts about any missing or unexpected params.
If no existing schema is found it will create one for you.

Once built, the tool can send the schema to the nf-core website so that you can use a graphical interface to organise and fill in the schema.
The tool checks the status of your schema on the website and once complete, saves your changes locally.

Usage is `nf-core schema build -d <pipeline_directory>`, eg:

<!-- RICH-CODEX
working_dir: tmp/nf-core-nextbigthing
timeout: 10
before_command: sed '25,30d' nextflow_schema.json > nextflow_schema.json.tmp && mv nextflow_schema.json.tmp nextflow_schema.json
-->

![`nf-core schema build --no-prompts`](docs/images/nf-core-schema-build.svg)

There are four flags that you can use with this command:

- `--dir <pipeline_dir>`: Specify a pipeline directory other than the current working directory
- `--no-prompts`: Make changes without prompting for confirmation each time. Does not launch web tool.
- `--web-only`: Skips comparison of the schema against the pipeline parameters and only launches the web tool.
- `--url <web_address>`: Supply a custom URL for the online tool. Useful when testing locally.

### Display the documentation for a pipeline schema

To get an impression about the current pipeline schema you can display the content of the `nextflow_schema.json` with `nf-core schema docs <pipeline-schema>`. This will print the content of your schema in Markdown format to the standard output.

There are four flags that you can use with this command:

- `--output <filename>`: Output filename. Defaults to standard out.
- `--format [markdown|html]`: Format to output docs in.
- `--force`: Overwrite existing files
- `--columns <columns_list>`: CSV list of columns to include in the parameter tables

### Add new parameters to the pipeline schema

If you want to add a parameter to the schema, you first have to add the parameter and its default value to the `nextflow.config` file with the `params` scope. Afterwards, you run the command `nf-core schema build` to add the parameters to your schema and open the graphical interface to easily modify the schema.

The graphical interface is oganzised in groups and within the groups the single parameters are stored. For a better overview you can collapse all groups with the `Collapse groups` button, then your new parameters will be the only remaining one at the bottom of the page. Now you can either create a new group with the `Add group` button or drag and drop the paramters in an existing group. Therefor the group has to be expanded. The group title will be displayed, if you run your pipeline with the `--help` flag and its description apears on the parameter page of your pipeline.

Now you can start to change the parameter itself. The description is a short explanation about the parameter, that apears if you run your pipeline with the `--help` flag. By clicking on the dictionary icon you can add a longer explanation for the parameter page of your pipeline. If you want to specify some conditions for your parameter, like the file extension, you can use the nut icon to open the settings. This menu depends on the `type` you assigned to your parameter. For intergers you can define a min and max value, and for strings the file extension can be specified.

After you filled your schema, click on the `Finished` button in the top rigth corner, this will automatically update your `nextflow_schema.json`. If this is not working you can copy the schema from the graphical interface and paste it in your `nextflow_schema.json` file.

### Update existing pipeline schema

Important for the update of a pipeline schema is, that if you want to change the default value of a parameter, you should change it in the `nextflow.config` file, since the value in the config file overwrites the value in the pipeline schema. To change any other parameter use `nf-core schema build --web-only` to open the graphical interface without rebuilding the pipeline schema. Now, you can change your parameters as mentioned above but keep in mind that changing the parameter datatype is depending on the default value you specified in the `nextflow.config` file.

### Linting a pipeline schema

The pipeline schema is linted as part of the main pipeline `nf-core lint` command,
however sometimes it can be useful to quickly check the syntax of the JSONSchema without running a full lint run.

Usage is `nf-core schema lint <schema>`, eg:

<!-- RICH-CODEX
working_dir: tmp/nf-core-nextbigthing
-->

![`nf-core schema lint nextflow_schema.json`](docs/images/nf-core-schema-lint.svg)