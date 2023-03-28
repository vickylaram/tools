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

# Installation

### Bioconda

You can install `nf-core/tools` from [bioconda](https://bioconda.github.io/recipes/nf-core/README.html).

First, install conda and configure the channels to use bioconda
(see the [bioconda documentation](https://bioconda.github.io/index.html#usage)).
Then, just run the conda installation command:

```bash
conda install nf-core
```

Alternatively, you can create a new environment with both nf-core/tools and nextflow:

```bash
conda create --name nf-core python=3.8 nf-core nextflow
conda activate nf-core
```

### Python Package Index

`nf-core/tools` can also be installed from [PyPI](https://pypi.python.org/pypi/nf-core/) using pip as follows:

```bash
pip install nf-core
```

### Docker image

There is a docker image that you can use to run `nf-core/tools` that has all of the requirements packaged (including Nextflow) and so should work out of the box. It is called [`nfcore/tools`](https://hub.docker.com/r/nfcore/tools) _**(NB: no hyphen!)**_

You can use this container on the command line as follows:

```bash
docker run -itv `pwd`:`pwd` -w `pwd` -u $(id -u):$(id -g) nfcore/tools
```

- `-i` and `-t` are needed for the interactive cli prompts to work (this tells Docker to use a pseudo-tty with stdin attached)
- The `-v` argument tells Docker to bind your current working directory (`pwd`) to the same path inside the container, so that files created there will be saved to your local file system outside of the container.
- `-w` sets the working directory in the container to this path, so that it's the same as your working directory outside of the container.
- `-u` sets your local user account as the user inside the container, so that any files created have the correct ownership permissions

After the above base command, you can use the regular command line flags that you would use with other types of installation.
For example, to launch the `viralrecon` pipeline:

```bash
docker run -itv `pwd`:`pwd` -w `pwd` -u $(id -u):$(id -g) nfcore/tools launch viralrecon -r 1.1.0
```

If you use `$NXF_SINGULARITY_CACHEDIR` for downloads, you'll also need to make this folder and environment variable available to the continer:

```bash
docker run -itv `pwd`:`pwd` -w `pwd` -u $(id -u):$(id -g) -v $NXF_SINGULARITY_CACHEDIR:$NXF_SINGULARITY_CACHEDIR -e NXF_SINGULARITY_CACHEDIR nfcore/tools launch viralrecon -r 1.1.0
```

#### Docker bash alias

The above base command is a bit of a mouthful to type, to say the least.
To make it easier to use, we highly recommend adding the following bash alias to your `~/.bashrc` file:

```bash
alias nf-core="docker run -itv `pwd`:`pwd` -w `pwd` -u $(id -u):$(id -g) nfcore/tools"
```

Once applied (you may need to reload your shell) you can just use the `nf-core` command instead:

```bash
nf-core list
```

#### Docker versions

You can use docker image tags to specify the version you would like to use. For example, `nfcore/tools:dev` for the latest development version of the code, or `nfcore/tools:1.14` for version `1.14` of tools.
If you omit this, it will default to `:latest`, which should be the latest stable release.

If you need a specific version of Nextflow inside the container, you can build an image yourself.
Clone the repo locally and check out whatever version of nf-core/tools that you need.
Then build using the `--build-arg NXF_VER` flag as follows:

```bash
docker build -t nfcore/tools:dev . --build-arg NXF_VER=20.04.0
```

### Development version

If you would like the latest development version of tools, the command is:

```bash
pip install --upgrade --force-reinstall git+https://github.com/nf-core/tools.git@dev
```

If you intend to make edits to the code, first make a fork of the repository and then clone it locally.
Go to the cloned directory and install with pip (also installs development requirements):

```bash
pip install --upgrade -r requirements-dev.txt -e .
```

### Using a specific Python interpreter

If you prefer, you can also run tools with a specific Python interpreter.
The command line usage and flags are then exactly the same as if you ran with the `nf-core` command.
Note that the module is `nf_core` with an underscore, not a hyphen like the console command.

For example:

```bash
python -m nf_core --help
python3 -m nf_core list
~/my_env/bin/python -m nf_core create --name mypipeline --description "This is a new skeleton pipeline"
```

### Using with your own Python scripts

The tools functionality is written in such a way that you can import it into your own scripts.
For example, if you would like to get a list of all available nf-core pipelines:

```python
import nf_core.list
wfs = nf_core.list.Workflows()
wfs.get_remote_workflows()
for wf in wfs.remote_workflows:
    print(wf.full_name)
```

Please see [https://nf-co.re/tools-docs/](https://nf-co.re/tools-docs/) for the function documentation.

### Automatic version check

nf-core/tools automatically checks the web to see if there is a new version of nf-core/tools available.
If you would prefer to skip this check, set the environment variable `NFCORE_NO_VERSION_CHECK`. For example:

```bash
export NFCORE_NO_VERSION_CHECK=1
```