# ![nf-core/tools](docs/images/nfcore-tools_logo_light.png#gh-light-mode-only) ![nf-core/tools](docs/images/nfcore-tools_logo_dark.png#gh-dark-mode-only) <!-- omit in toc -->

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

- [`nf-core` tools installation](./readme_sub/tools_installtion.md)
- [`nf-core` tools update](./readme_sub/tools_update.md)
- [`nf-core list` - List available pipelines](./readme_sub/listing_pipelines.md)
- [`nf-core launch` - Run a pipeline with interactive parameter prompts](./readme_sub/launch_pipeline.md)
- [`nf-core download` - Download pipeline for offline use](./readme_sub/downloading_pipeline_off.md)
- [`nf-core licences` - List software licences in a pipeline](./readme_sub/pipeline_software_licences.md)
- [`nf-core create` - Create a new pipeline with the nf-core template](./readmes/creating_a_new_pipeline.md)
- [`nf-core lint` - Check pipeline code against nf-core guidelines](./readme_sub/linting_a_workflow.md)
- [`nf-core schema` - Work with pipeline schema files](./readme_sub/pipeline_schema.md)
- [`nf-core bump-version` - Update nf-core pipeline version number](./readme_sub/bumping_a_pipeline.md)
- [`nf-core sync` - Synchronise pipeline TEMPLATE branches](./readme_sub/sync_a_pipeline.md)
- [`nf-core modules` - commands for dealing with DSL2 modules](./readme_sub/modules/modules.md)

  - [`modules list` - List available modules](./readme_sub/modules/list_modules/list_installed_modules.md)
    - [`modules list remote` - List remote modules](./readme_sub/modules/list_modules/list_remote_modules.md)
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



## Modules

With the advent of [Nextflow DSL2](https://www.nextflow.io/docs/latest/dsl2.html), we are creating a centralised repository of modules.
These are software tool process definitions that can be imported into any pipeline.
This allows multiple pipelines to use the same code for share tools and gives a greater degree of granulairy and unit testing.

The nf-core DSL2 modules repository is at <https://github.com/nf-core/modules>

### Custom remote moduless

The modules supercommand comes with two flags for specifying a custom remote:

- `--git-remote <git remote url>`: Specify the repository from which the modules should be fetched as a git URL. Defaults to the github repository of `nf-core/modules`.
- `--branch <branch name>`: Specify the branch from which the modules should be fetched. Defaults to the default branch of your repository.

For example, if you want to install the `fastqc` module from the repository `nf-core/modules-test` hosted at `gitlab.com`, you can use the following command:

```terminal
nf-core modules --git-remote git@gitlab.com:nf-core/modules-test.git install fastqc
```

Note that a custom remote must follow a similar directory structure to that of `nf-core/moduleś` for the `nf-core modules` commands to work properly.

The directory where modules are installed will be prompted or obtained from `org_path` in the `.nf-core.yml` file if available. If your modules are located at `modules/my-folder/TOOL/SUBTOOL` your `.nf-core.yml` should have:

```yaml
org_path: my-folder
```

Please avoid installing the same tools from two different remotes, as this can lead to further errors.

The modules commands will during initalisation try to pull changes from the remote repositories. If you want to disable this, for example
due to performance reason or if you want to run the commands offline, you can use the flag `--no-pull`. Note however that the commands will
still need to clone repositories that have previously not been used.

### Private remote repositories

You can use the modules command with private remote repositories. Make sure that your local `git` is correctly configured with your private remote
and then specify the remote the same way you would do with a public remote repository.

### List modules

The `nf-core modules list` command provides the subcommands `remote` and `local` for listing modules installed in a remote repository and in the local pipeline respectively. Both subcommands allow to use a pattern for filtering the modules by keywords eg: `nf-core modules list <subcommand> <keyword>`.

#### List remote modules

To list all modules available on [nf-core/modules](https://github.com/nf-core/modules), you can use
`nf-core modules list remote`, which will print all available modules to the terminal.

<!-- RICH-CODEX
working_dir: tmp/nf-core-nextbigthing
head: 25
-->

![`nf-core modules list remote`](docs/images/nf-core-modules-list-remote.svg)

#### List installed modules

To list modules installed in a local pipeline directory you can use `nf-core modules list local`. This will list the modules install in the current working directory by default. If you want to specify another directory, use the `--dir <pipeline_dir>` flag.

<!-- RICH-CODEX
working_dir: tmp/nf-core-nextbigthing
head: 25
-->

![`nf-core modules list local`](docs/images/nf-core-modules-list-local.svg)

## Show information about a module

For quick help about how a module works, use `nf-core modules info <tool>`.
This shows documentation about the module on the command line, similar to what's available on the
[nf-core website](https://nf-co.re/modules).

<!-- RICH-CODEX
working_dir: tmp/nf-core-nextbigthing
-->

![`nf-core modules info abacas`](docs/images/nf-core-modules-info.svg)

### Install modules in a pipeline

You can install modules from [nf-core/modules](https://github.com/nf-core/modules) in your pipeline using `nf-core modules install`.
A module installed this way will be installed to the `./modules/nf-core/modules` directory.

<!-- RICH-CODEX
working_dir: tmp/nf-core-nextbigthing
-->

![`nf-core modules install abacas`](docs/images/nf-core-modules-install.svg)

You can pass the module name as an optional argument to `nf-core modules install` instead of using the cli prompt, eg: `nf-core modules install fastqc`. You can specify a pipeline directory other than the current working directory by using the `--dir <pipeline dir>`.

There are three additional flags that you can use when installing a module:

- `--force`: Overwrite a previously installed version of the module.
- `--prompt`: Select the module version using a cli prompt.
- `--sha <commit_sha>`: Install the module at a specific commit.

### Update modules in a pipeline

You can update modules installed from a remote repository in your pipeline using `nf-core modules update`.

<!-- RICH-CODEX
working_dir: tmp/nf-core-nextbigthing
-->

![`nf-core modules update --all --no-preview`](docs/images/nf-core-modules-update.svg)

You can pass the module name as an optional argument to `nf-core modules update` instead of using the cli prompt, eg: `nf-core modules update fastqc`. You can specify a pipeline directory other than the current working directory by using the `--dir <pipeline dir>`.

There are five additional flags that you can use with this command:

- `--force`: Reinstall module even if it appears to be up to date
- `--prompt`: Select the module version using a cli prompt.
- `--sha <commit_sha>`: Install the module at a specific commit from the `nf-core/modules` repository.
- `--preview/--no-preview`: Show the diff between the installed files and the new version before installing.
- `--save-diff <filename>`: Save diffs to a file instead of updating in place. The diffs can then be applied with `git apply <filename>`.
- `--all`: Use this flag to run the command on all modules in the pipeline.

If you don't want to update certain modules or want to update them to specific versions, you can make use of the `.nf-core.yml` configuration file. For example, you can prevent the `star/align` module installed from `nf-core/modules` from being updated by adding the following to the `.nf-core.yml` file:

```yaml
update:
  https://github.com/nf-core/modules.git:
    nf-core:
      star/align: False
```

If you want this module to be updated only to a specific version (or downgraded), you could instead specifiy the version:

```yaml
update:
  https://github.com/nf-core/modules.git:
    nf-core:
      star/align: "e937c7950af70930d1f34bb961403d9d2aa81c7"
```

This also works at the repository level. For example, if you want to exclude all modules installed from `nf-core/modules` from being updated you could add:

```yaml
update:
  https://github.com/nf-core/modules.git:
    nf-core: False
```

or if you want all modules in `nf-core/modules` at a specific version:

```yaml
update:
  https://github.com/nf-core/modules.git:
    nf-core: "e937c7950af70930d1f34bb961403d9d2aa81c7"
```

Note that the module versions specified in the `.nf-core.yml` file has higher precedence than versions specified with the command line flags, thus aiding you in writing reproducible pipelines.

### Remove a module from a pipeline

To delete a module from your pipeline, run `nf-core modules remove`.

<!-- RICH-CODEX
working_dir: tmp/nf-core-nextbigthing
-->

![`nf-core modules remove abacas`](docs/images/nf-core-modules-remove.svg)

You can pass the module name as an optional argument to `nf-core modules remove` instead of using the cli prompt, eg: `nf-core modules remove fastqc`. To specify the pipeline directory, use `--dir <pipeline_dir>`.

### Create a patch file for a module

If you want to make a minor change to a locally installed module but still keep it up date with the remote version, you can create a patch file using `nf-core modules patch`.

<!-- RICH-CODEX
working_dir: tmp/nf-core-nextbigthing
before_command:  sed "s/process_medium/process_low/g" modules/nf-core/modules/fastqc/main.nf > modules/nf-core/modules/fastqc/main.nf.patch && mv modules/nf-core/modules/fastqc/main.nf.patch modules/nf-core/modules/fastqc/main.nf
-->

![`nf-core modules patch fastqc`](docs/images/nf-core-modules-patch.svg)

The generated patches work with `nf-core modules update`: when you install a new version of the module, the command tries to apply
the patch automatically. The patch application fails if the new version of the module modifies the same lines as the patch. In this case,
the patch new version is installed but the old patch file is preserved.

When linting a patched module, the linting command will check the validity of the patch. When running other lint tests the patch is applied in reverse, and the original files are linted.

### Create a new module

This command creates a new nf-core module from the nf-core module template.
This ensures that your module follows the nf-core guidelines.
The template contains extensive `TODO` messages to walk you through the changes you need to make to the template.

You can create a new module using `nf-core modules create`.

This command can be used both when writing a module for the shared [nf-core/modules](https://github.com/nf-core/modules) repository,
and also when creating local modules for a pipeline.

Which type of repository you are working in is detected by the `repository_type` flag in a `.nf-core.yml` file in the root directory,
set to either `pipeline` or `modules`.
The command will automatically look through parent directories for this file to set the root path, so that you can run the command in a subdirectory.
It will start in the current working directory, or whatever is specified with `--dir <directory>`.

The `nf-core modules create` command will prompt you with the relevant questions in order to create all of the necessary module files.

<!-- RICH-CODEX
working_dir: tmp
before_command: git clone https://github.com/nf-core/modules.git && cd modules
fake_command: nf-core modules create fastqc --author @nf-core-bot  --label process_low --meta --force
-->

![`cd modules && nf-core modules create fastqc --author @nf-core-bot  --label process_low --meta --force`](docs/images/nf-core-modules-create.svg)

### Create a module test config file

All modules on [nf-core/modules](https://github.com/nf-core/modules) have a strict requirement of being unit tested using minimal test data.
To help developers build new modules, the `nf-core modules create-test-yml` command automates the creation of the yaml file required to document the output file `md5sum` and other information generated by the testing.
After you have written a minimal Nextflow script to test your module `tests/modules/<tool>/<subtool>/main.nf`, this command will run the tests for you and create the `tests/modules/<tool>/<subtool>/test.yml` file.

<!-- RICH-CODEX
working_dir: tmp/modules
extra_env:
  PROFILE: 'conda'
-->

![`nf-core modules create-test-yml fastqc --no-prompts --force`](docs/images/nf-core-modules-create-test.svg)

### Check a module against nf-core guidelines

Run the `nf-core modules lint` command to check modules in the current working directory (pipeline or nf-core/modules clone) against nf-core guidelines.

Use the `--all` flag to run linting on all modules found. Use `--dir <pipeline_dir>` to specify another directory than the current working directory.

<!-- RICH-CODEX
working_dir: tmp/modules
before_command: sed 's/1.13a/1.10/g' modules/multiqc/main.nf > modules/multiqc/main.nf.tmp && mv modules/multiqc/main.nf.tmp modules/multiqc/main.nf
-->

![`nf-core modules lint multiqc`](docs/images/nf-core-modules-lint.svg)

### Run the tests for a module using pytest

To run unit tests of a module that you have installed or the test created by the command [`nf-core modules create-test-yml`](#create-a-module-test-config-file), you can use `nf-core modules test` command. This command runs the tests specified in `modules/tests/software/<tool>/<subtool>/test.yml` file using [pytest](https://pytest-workflow.readthedocs.io/en/stable/).

You can specify the module name in the form TOOL/SUBTOOL in command line or provide it later by prompts.

<!-- RICH-CODEX
working_dir: tmp/modules
timeout: 30
extra_env:
  PROFILE: 'conda'
-->

![`nf-core modules test samtools/view --no-prompts`](docs/images/nf-core-modules-test.svg)

### Bump bioconda and container versions of modules in

If you are contributing to the `nf-core/modules` repository and want to bump bioconda and container versions of certain modules, you can use the `nf-core modules bump-versions` helper tool. This will bump the bioconda version of a single or all modules to the latest version and also fetch the correct Docker and Singularity container tags.

<!-- RICH-CODEX
working_dir: tmp/modules
-->

![`nf-core modules bump-versions fastqc`](docs/images/nf-core-modules-bump-version.svg)

If you don't want to update certain modules or want to update them to specific versions, you can make use of the `.nf-core.yml` configuration file. For example, you can prevent the `star/align` module from being updated by adding the following to the `.nf-core.yml` file:

```yaml
bump-versions:
  star/align: False
```

If you want this module to be updated only to a specific version (or downgraded), you could instead specifiy the version:

```yaml
bump-versions:
  star/align: "2.6.1d"
```

### Generate the name for a multi-tool container image

When you want to use an image of a multi-tool container and you know the specific dependencies and their versions of that container, for example, by looking them up in the [BioContainers hash.tsv](https://github.com/BioContainers/multi-package-containers/blob/master/combinations/hash.tsv), you can use the `nf-core modules mulled` helper tool. This tool generates the name of a BioContainers mulled image.

<!-- RICH-CODEX
working_dir: tmp/modules
after_command: cd ../../ && rm -rf tmp
-->

![`nf-core modules mulled pysam==0.16.0.1 biopython==1.78`](docs/images/nf-core-modules-mulled.svg)

## Subworkflows

After the launch of nf-core modules, we can provide now also nf-core subworkflows to fully utilize the power of DSL2 modularization.
Subworkflows are chains of multiple module definitions that can be imported into any pipeline.
This allows multiple pipelines to use the same code for a the same tasks, and gives a greater degree of reusability and unit testing.

To allow us to test modules and subworkflows together we put the nf-core DSL2 subworkflows into the `subworkflows` directory of the modules repository is at <https://github.com/nf-core/modules>.

### Custom remote subworkflows

The subworkflows supercommand released in nf-core/tools version 2.7 comes with two flags for specifying a custom remote repository:

- `--git-remote <git remote url>`: Specify the repository from which the subworkflows should be fetched as a git URL. Defaults to the github repository of `nf-core/modules`.
- `--branch <branch name>`: Specify the branch from which the subworkflows should be fetched. Defaults to the default branch of your repository.

For example, if you want to install the `bam_stats_samtools` subworkflow from the repository `nf-core/modules-test` hosted at `gitlab.com` in the branch `subworkflows`, you can use the following command:

```bash
nf-core subworkflows --git-remote git@gitlab.com:nf-core/modules-test.git --branch subworkflows install bam_stats_samtools
```

Note that a custom remote must follow a similar directory structure to that of `nf-core/modules` for the `nf-core subworkflows` commands to work properly.

The directory where subworkflows are installed will be prompted or obtained from `org_path` in the `.nf-core.yml` file if available. If your subworkflows are located at `subworkflows/my-folder/SUBWORKFLOW_NAME` your `.nf-core.yml` file should have:

```yaml
org_path: my-folder
```

Please avoid installing the same tools from two different remotes, as this can lead to further errors.

The subworkflows commands will during initalisation try to pull changes from the remote repositories. If you want to disable this, for example due to performance reason or if you want to run the commands offline, you can use the flag `--no-pull`. Note however that the commands will still need to clone repositories that have previously not been used.

### Private remote repositories

You can use the subworkflows command with private remote repositories. Make sure that your local `git` is correctly configured with your private remote
and then specify the remote the same way you would do with a public remote repository.

### List subworkflows

The `nf-core subworkflows list` command provides the subcommands `remote` and `local` for listing subworkflows installed in a remote repository and in the local pipeline respectively. Both subcommands allow to use a pattern for filtering the subworkflows by keywords eg: `nf-core subworkflows list <subworkflow_name> <keyword>`.

#### List remote subworkflows

To list all subworkflows available on [nf-core/modules](https://github.com/nf-core/modules), you can use
`nf-core subworkflows list remote`, which will print all available subworkflows to the terminal.

<!-- RICH-CODEX
working_dir: tmp/nf-core-nextbigthing
head: 25
-->

![`nf-core subworkflows list remote`](docs/images/nf-core-subworkflows-list-remote.svg)

#### List installed subworkflows

To list subworkflows installed in a local pipeline directory you can use `nf-core subworkflows list local`. This will list the subworkflows install in the current working directory by default. If you want to specify another directory, use the `--dir <pipeline_dir>` flag.

<!-- RICH-CODEX
working_dir: tmp/nf-core-nextbigthing
head: 25
-->

![`nf-core subworkflows list local`](docs/images/nf-core-subworkflows-list-local.svg)

## Show information about a subworkflow

For quick help about how a subworkflow works, use `nf-core subworkflows info <subworkflow_name>`.
This shows documentation about the subworkflow on the command line, similar to what's available on the
[nf-core website](https://nf-co.re/subworkflows).

<!-- RICH-CODEX
working_dir: tmp/nf-core-nextbigthing
-->

![`nf-core subworkflows info bam_rseqc`](docs/images/nf-core-subworkflows-info.svg)

### Install subworkflows in a pipeline

You can install subworkflows from [nf-core/modules](https://github.com/nf-core/modules) in your pipeline using `nf-core subworkflows install`.
A subworkflow installed this way will be installed to the `./subworkflows/nf-core` directory.

<!-- RICH-CODEX
working_dir: tmp/nf-core-nextbigthing
-->

![`nf-core subworkflows install bam_rseqc`](docs/images/nf-core-subworkflows-install.svg)

You can pass the subworkflow name as an optional argument to `nf-core subworkflows install` like above or select it from a list of available subworkflows by only running `nf-core subworkflows install`.

There are four additional flags that you can use when installing a subworkflow:

- `--dir`: Pipeline directory, the default is the current working directory.
- `--force`: Overwrite a previously installed version of the subworkflow.
- `--prompt`: Select the subworkflow version using a cli prompt.
- `--sha <commit_sha>`: Install the subworkflow at a specific commit.

### Update subworkflows in a pipeline

You can update subworkflows installed from a remote repository in your pipeline using `nf-core subworkflows update`.

<!-- RICH-CODEX
working_dir: tmp/nf-core-nextbigthing
-->

![`nf-core subworkflows update --all --no-preview`](docs/images/nf-core-subworkflows-update.svg)

You can pass the subworkflow name as an optional argument to `nf-core subworkflows update` like above or select it from the list of available subworkflows by only running `nf-core subworkflows update`.

There are six additional flags that you can use with this command:

- `--dir`: Pipeline directory, the default is the current working directory.
- `--force`: Reinstall subworkflow even if it appears to be up to date
- `--prompt`: Select the subworkflow version using a cli prompt.
- `--sha <commit_sha>`: Install the subworkflow at a specific commit from the `nf-core/modules` repository.
- `--preview/--no-preview`: Show the diff between the installed files and the new version before installing.
- `--save-diff <filename>`: Save diffs to a file instead of updating in place. The diffs can then be applied with `git apply <filename>`.
- `--all`: Use this flag to run the command on all subworkflows in the pipeline.
- `--update-deps`: Use this flag to automatically update all dependencies of a subworkflow.

If you don't want to update certain subworkflows or want to update them to specific versions, you can make use of the `.nf-core.yml` configuration file. For example, you can prevent the `bam_rseqc` subworkflow installed from `nf-core/modules` from being updated by adding the following to the `.nf-core.yml` file:

```yaml
update:
  https://github.com/nf-core/modules.git:
    nf-core:
      bam_rseqc: False
```

If you want this subworkflow to be updated only to a specific version (or downgraded), you could instead specifiy the version:

```yaml
update:
  https://github.com/nf-core/modules.git:
    nf-core:
      bam_rseqc: "36a77f7c6decf2d1fb9f639ae982bc148d6828aa"
```

This also works at the repository level. For example, if you want to exclude all modules and subworkflows installed from `nf-core/modules` from being updated you could add:

```yaml
update:
  https://github.com/nf-core/modules.git:
    nf-core: False
```

or if you want all subworkflows in `nf-core/modules` at a specific version:

```yaml
update:
  https://github.com/nf-core/modules.git:
    nf-core: "e937c7950af70930d1f34bb961403d9d2aa81c7"
```

Note that the subworkflow versions specified in the `.nf-core.yml` file has higher precedence than versions specified with the command line flags, thus aiding you in writing reproducible pipelines.

### Remove a subworkflow from a pipeline

To delete a subworkflow from your pipeline, run `nf-core subworkflows remove`.

<!-- RICH-CODEX
working_dir: tmp/nf-core-nextbigthing
-->

![`nf-core subworkflows remove bam_rseqc`](docs/images/nf-core-subworkflows-remove.svg)

You can pass the subworkflow name as an optional argument to `nf-core subworkflows remove` like above or select it from the list of available subworkflows by only running `nf-core subworkflows remove`. To specify the pipeline directory, use `--dir <pipeline_dir>`.

### Create a new subworkflow

This command creates a new nf-core subworkflow from the nf-core subworkflow template.
This ensures that your subworkflow follows the nf-core guidelines.
The template contains extensive `TODO` messages to walk you through the changes you need to make to the template.
See the [subworkflow documentation](https://nf-co.re/docs/contributing/subworkflows) for more details around creating a new subworkflow, including rules about nomenclature and a step-by-step guide.

You can create a new subworkflow using `nf-core subworkflows create`.

This command can be used both when writing a subworkflow for the shared [nf-core/modules](https://github.com/nf-core/modules) repository,
and also when creating local subworkflows for a pipeline.

Which type of repository you are working in is detected by the `repository_type` flag in a `.nf-core.yml` file in the root directory,
set to either `pipeline` or `modules`.
The command will automatically look through parent directories for this file to set the root path, so that you can run the command in a subdirectory.
It will start in the current working directory, or whatever is specified with `--dir <directory>`.

The `nf-core subworkflows create` command will prompt you with the relevant questions in order to create all of the necessary subworkflow files.

<!-- RICH-CODEX
working_dir: tmp
before_command: git clone https://github.com/nf-core/modules.git && cd modules
fake_command: nf-core subworkflows create bam_stats_samtools --author @nf-core-bot  --label process_low --meta --force
-->

![`cd modules && nf-core subworkflows create bam_stats_samtools --author @nf-core-bot  --label process_low --meta --force`](docs/images/nf-core-subworkflows-create.svg)

### Create a subworkflow test config file

All subworkflows on [nf-core/modules](https://github.com/nf-core/modules) have a strict requirement of being unit tested using minimal test data.
To help developers build new subworkflows, the `nf-core subworkflows create-test-yml` command automates the creation of the yaml file required to document the output file `md5sum` and other information generated by the testing.
After you have written a minimal Nextflow script to test your subworkflow in `/tests/subworkflow/<subworkflow_name>/main.nf`, this command will run the tests for you and create the `/tests/subworkflow/<tool>/<subtool>/test.yml` file.

<!-- RICH-CODEX
working_dir: tmp/subworkflows
extra_env:
  PROFILE: 'conda'
-->

![`nf-core subworkflows create-test-yml bam_stats_samtools --no-prompts --force`](docs/images/nf-core-subworkflows-create-test.svg)

### Run the tests for a subworkflow using pytest

To run unit tests of a subworkflow that you have installed or the test created by the command [`nf-core subworkflow create-test-yml`](#create-a-subworkflow-test-config-file), you can use `nf-core subworkflows test` command. This command runs the tests specified in `tests/subworkflows/<subworkflow_name>/test.yml` file using [pytest](https://pytest-workflow.readthedocs.io/en/stable/).

You can specify the subworkflow name in the form TOOL/SUBTOOL in command line or provide it later by prompts.

<!-- RICH-CODEX
working_dir: tmp/subworkflows
timeout: 30
extra_env:
  PROFILE: 'conda'
-->

![`nf-core subworkflows test bam_rseqc --no-prompts`](docs/images/nf-core-subworkflows-test.svg)

## Citation

If you use `nf-core tools` in your work, please cite the `nf-core` publication as follows:

> **The nf-core framework for community-curated bioinformatics pipelines.**
>
> Philip Ewels, Alexander Peltzer, Sven Fillinger, Harshil Patel, Johannes Alneberg, Andreas Wilm, Maxime Ulysse Garcia, Paolo Di Tommaso & Sven Nahnsen.
>
> _Nat Biotechnol._ 2020 Feb 13. doi: [10.1038/s41587-020-0439-x](https://dx.doi.org/10.1038/s41587-020-0439-x).
