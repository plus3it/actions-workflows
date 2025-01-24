## actions-workflows

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](http://keepachangelog.com/) and this project adheres to [Semantic Versioning](http://semver.org/).

### [1.6.3](https://github.com/plus3it/actions-workflows/releases/tag/1.6.3)

**Released**: 2025.01.24

*   Supports passing lint target through release workflow

### [1.6.2](https://github.com/plus3it/actions-workflows/releases/tag/1.6.2)

**Released**: 2024.08.16

*   Updates actions/checkout steps to use current v4 release, since EL7 containers
    no longer need to be supported.

### [1.6.1](https://github.com/plus3it/actions-workflows/releases/tag/1.6.1)

**Released**: 2024.07.02

*   Quotes inputs for lint action to support multiple targets

### [1.6.0](https://github.com/plus3it/actions-workflows/releases/tag/1.6.0)

**Released**: 2024.06.04

**Summary**:

*   Updates salt workflow for EL8 to use the Alma Linux 8 container. CentOS
    Stream 8 was discontinued as of June 1, 2024. Along with this
    discontinuation, the CentOS maintainers removed the CentOS Stream 8 yum
    repositories. Lack of repository-access breaks the ability to adequately
    customize the CI container, causing job failures. Explored image-options
    from Red Hat and both the Rocky and Alma projects. Found that the Red Hat
    images' repositories lacked critical RPMs. Found that the Official
    containers from Alma Linux seem to be getting updated with greater
    frequency than those from Rocky, so opted to switch to Alma.
*   Adds initial hooks for a salt workflow for EL9. Chose containers from the
    Alma project as a hedge against premature-discontinuation of the CentOS
    repos and because the Red Hat 9 container-images similarly lack critical
    RPMs.

### [1.5.0](https://github.com/plus3it/actions-workflows/releases/tag/1.5.0)

**Released**: 2024.02.22

**Summary**:

*   Updates salt workflows with option to install python requirements into salt
    runtime

### [1.4.1](https://github.com/plus3it/actions-workflows/releases/tag/1.4.1)

**Released**: 2024.02.21

**Summary**:

*   Updates Github Actions workflow to use previous version of action/checkout
    to v3 instead of v4 to support EL7 containers. Also updates dependabot config
    to ignore future action/checkout releases.

### [1.4.0](https://github.com/plus3it/actions-workflows/releases/tag/1.4.0)

**Released**: 2023.11.06

**Summary**:

*   Updates Windows Salt test to support retrieving the latest salt release for
    a given salt major version. (The Linux Salt test already works this way.)

### [1.3.2](https://github.com/plus3it/actions-workflows/releases/tag/1.3.2)

**Released**: 2023.08.14

**Summary**:

*   Remove install of docker-compose, it is no longer present in tardigrade-ci

### [1.3.1](https://github.com/plus3it/actions-workflows/releases/tag/1.3.1)

**Released**: 2023.07.17

**Summary**:

*   Uses bash on Windows runners to validate steps using shellcheck

### [1.3.0](https://github.com/plus3it/actions-workflows/releases/tag/1.3.0)

**Released**: 2023.06.26

**Summary**:

*   Adds reusable workflow for testing salt formulas on Windows

### [1.2.1](https://github.com/plus3it/actions-workflows/releases/tag/1.2.1)

**Released**: 2023.06.20

**Summary**:

*   Improves the logic getting the current version to avoid multiple returns

### [1.2.0](https://github.com/plus3it/actions-workflows/releases/tag/1.2.0)

**Released**: 2023.05.22

**Summary**:

*   Adds reusable workflow for testing salt formulas on Linux

### [1.1.0](https://github.com/plus3it/actions-workflows/releases/tag/1.1.0)

**Released**: 2023.03.27

**Summary**:

*   Adds option to pass PYTEST_ARGS to mockstack/pytest target

### [1.0.1](https://github.com/plus3it/actions-workflows/releases/tag/1.0.1)

**Released**: 2023.02.06

**Summary**:

*   Allows test job to be optional in release workflow

### [1.0.0](https://github.com/plus3it/actions-workflows/releases/tag/1.0.0)

**Released**: 2023.01.24

**Summary**:

*   Initial release
