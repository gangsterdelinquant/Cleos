# eosio
The [eosio](https://buildkite.com/EOSIO/eosio) pipeline is the primary CI/CD pipeline for members of the EOSIO organization developing on the EOSIO/eos repository. The [eosio-build-unpinned](https://buildkite.com/EOSIO/eosio-build-unpinned) pipeline is also executed regularly. The difference in these two pipelines is whether the compiler and other dependencies are pinned to specific versions. The eosio pipeline uses pinned compilers/dependencies while the eosio-build-unpinned pipeline avoids pinning dependencies as much as possible.

<details>
<summary>More Info</summary>

## Index
1. [Configuration](https://github.com/EOSIO/eos/blob/develop/.cicd/README.md#configuration)
   1. [Variables](https://github.com/EOSIO/eos/blob/develop/.cicd/README.md#variables)
   1. [Examples](https://github.com/EOSIO/eos/blob/develop/.cicd/README.md#examples)
1. [Pipelines](https://github.com/EOSIO/eos/blob/develop/.cicd/README.md#pipelines)
1. [See Also](https://github.com/EOSIO/eos/blob/develop/.cicd/README.md#see-also)

## Configuration
Most EOSIO pipelines are run any time you push a commit or tag to an open pull request in [eos](https://github.com/EOSIO/eos), any time you merge a pull request, and nightly. The [eosio-lrt](https://buildkite.com/EOSIO/eosio-lrt) pipeline only runs when you merge a pull request because it takes so long. Long-running tests are also run in the [eosio](https://buildkite.com/EOSIO/eosio) nightly builds, which have `RUN_ALL_TESTS='true'` set.

### Variables
Most pipelines in the organization have several environment variables that can be used to configure how the pipeline runs. These environment variables can be specified when manually triggering a build via the Buildkite UI.

Configure which platforms are run:
```bash
SKIP_LINUX='true|false'              # skip all steps on Linux distros
SKIP_MAC='true|false'                # skip all steps on Mac hardware
```
These will override more specific operating system declarations, and primarily exist to disable one of our two buildfleets should one be sick or the finite macOS agents are congested.

Configure which operating systems are built, tested, and packaged:
```bash
SKIP_AMAZON_LINUX_2='true|false'     # skip all steps for Amazon Linux 2
SKIP_CENTOS_7_7='true|false'         # skip all steps for Centos 7.7
SKIP_CENTOS_8='true|false'           # skip all steps for Centos 8
SKIP_MACOS_10_14='true|false'        # skip all steps for MacOS 10.14
SKIP_MACOS_10_15='true|false'        # skip all steps for MacOS 10.15
SKIP_MACOS_11='true|false'           # skip all steps for MacOS 11
SKIP_UBUNTU_16_04='true|false'       # skip all steps for Ubuntu 16.04
SKIP_UBUNTU_18_04='true|false'       # skip all steps for Ubuntu 18.04
SKIP_UBUNTU_20_04='true|false'       # skip all steps for Ubuntu 20.04
```

Configure which steps are executed for each operating system:
```bash
SKIP_BUILD='true|false'              # skip all build steps
SKIP_UNIT_TESTS='true|false'         # skip all unit tests
SKIP_WASM_SPEC_TESTS='true|false'    # skip all wasm spec tests
SKIP_SERIAL_TESTS='true|false'       # skip all integration tests
SKIP_LONG_RUNNING_TESTS='true|false' # skip all long running tests
SKIP_MULTIVERSION_TEST='true|false'  # skip all multiversion tests
SKIP_SYNC_TESTS='true|false'         # skip all sync tests
SKIP_PACKAGE_BUILDER='true|false'    # skip all packaging steps
```

Configure how the steps are executed:
```bash
PINNED='true|false'                  # use specific versions of dependencies instead of whatever version is provided by default on a given platform
TIMEOUT='##'                         # set timeout in minutes for all steps
```

### Examples
Build and test on Linux only:
```bash
SKIP_MAC='true'
```

Build and test on MacOS only:
```bash
SKIP_LINUX='true'
```

Skip all tests:
```bash
SKIP_UNIT_TESTS='true'
SKIP_WASM_SPEC_TESTS='true'
SKIP_SERIAL_TESTS='true'
SKIP_LONG_RUNNING_TESTS='true'
SKIP_MULTIVERSION_TEST='true'
SKIP_SYNC_TESTS='true'
```

## Pipelines
There are several eosio pipelines that exist and are triggered by pull requests, pipelines, or schedules:

Pipeline | Details
---|---
[eosio](https://buildkite.com/EOSIO/eosio) | Primary pipeline for the EOSIO/eos Github repo. It is triggered when a pull request is created.
[eosio-base-images](https://buildkite.com/EOSIO/eosio-base-images) | Pipeline that ensures all MacOS VM and Docker container builders can be built. It is scheduled for periodic execution.
[eosio-big-sur-beta](https://buildkite.com/EOSIO/eosio-big-sur-beta) | Pipeline that performs a build only using MacOS 11 builders. It is scheduled for periodic execution.
[eosio-build-scripts](https://buildkite.com/EOSIO/eosio-build-scripts) | Pipeline that ensure the build scripts function. It is scheduled for periodic execution.
[eosio-build-unpinned](https://buildkite.com/EOSIO/eosio-build-unpinned) | Pipeline that performs a build without a pinned compiler. It is triggered when a pull request is created.
[eosio-lrt](https://buildkite.com/EOSIO/eosio-lrt) | Pipeline that only executes the long running tests. It is triggered after a pull request is merged.
[eosio-resume-from-state](https://buildkite.com/EOSIO/eosio-resume-from-state) | Pipeline that ensures that built binaries can resume from previous binary versions. It is triggered during pull request builds.
[eosio-sync-from-genesis](https://buildkite.com/EOSIO/eosio-sync-from-genesis) | Pipeline that ensures built code can sync properly. It is triggered during pull request builds.

</details>

## See Also
- [Buildkite Documentation](https://github.com/EOSIO/devdocs/wiki/Buildkite) (internal)
- [EOSIO Resume from State Documentation](https://github.com/EOSIO/auto-eks-sync-nodes/blob/master/pipelines/eosio-resume-from-state/README.md) (internal)
- [#help-automation](https://blockone.slack.com/archives/CMTAZ9L4D) (internal)