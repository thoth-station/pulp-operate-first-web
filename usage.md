---
title: Pulp Python package index
description: Pulp Python package index
---

This document states basic usage of the Pulp instance hosted on Operate First.
The Pulp instance is available at
[pulp.operate.first.cloud](https://pulp.operate-first.cloud/pulp/api/v3/status/)
and is used at Red Hat to host Python packages as an alternative to the
publicly available [PyPI](https://pypi.org/). Unlike PyPI, Pulp hosts multiple
Python package indexes that can be specific to teams or user needs.

## System requirements for consuming Python packages

**Minimum requirements:** RHEL 7 + SCL Python and pip

**Optimal requirements:** RHEL 8, UBI 8 or Fedora which is not EOL

## Requesting a Python package index on Operate First Pulp

1. Request a repository on the Operate first Pulp Python package index for your package by
[creating a request at
operate-first/support](https://github.com/operate-first/support/issues/new/choose)
using the "*New Python package index request*" template.

2. An admin of the Operate first Pulp Python package index will create the index
and provide the url for the explicit package in the ticket.

## Consuming Python packages from Pulp Python index

To consume packages from the Pulp Python package index, point pip to it using
[--index-url](https://pip.pypa.io/en/stable/cli/pip_install/#install-index-url):

```bash
pip install --index-url https://pulp.operate-first.cloud/pypi/<index-name>/simple/ --extra-index-url https://pypi.org/simple
```

[--extra-index-url](https://pip.pypa.io/en/stable/cli/pip_install/#cmdoption-extra-index-url)
(optional) will make sure that packages that are not found on the specified
Pulp index are looked up on [PyPI](https://pypi.org/).

You can also use [Pipenv](https://pipenv.pypa.io/) and [configure Python
package indexes in
Pipfile](https://pipenv.pypa.io/en/latest/advanced/#specifying-package-indexes).

**NOTE:** Both cases are prone to dependency confusion attacks so mind
dependencies you install.

**Experimental:** You can use [Thoth](https://thoth-station.ninja) to consume
Python packages. See its [strict index
configuration](https://thoth-station.ninja/docs/developers/adviser/experimental_features.html#strict-index-configuration).

## Publishing Python packages to Pulp Python index

Currently, only [AICoE-CI](https://github.com/AICoE/aicoe-ci) can publish
Python packages to Pulp as Pulp does not support RBAC. Thus AICoE-CI acts as an
authority for publishing Python packages to Pulp indexes.
Follow instructions:

- Install [aicoe-ci](https://github.com/apps/aicoe-ci) GitHub app in your GitHub repository.
- Include `.aicoe-ci.yaml` configuration.yaml, a sample can be found [here](https://github.com/thoth-station/aicoe-ci-pulp-upload-example/blob/master/.aicoe-ci.yaml).
- Edit the snippet below with your explicit **index name** and add it in `.aicoe-ci.yaml` configuration yaml

```yaml
package-upload:
  - instance: pulp
    pulp-index: <index-name>
    allow-sdist: true
    allow-bdist: true
```

- Create a tag in GitHub and the app will upload the package to operate first pulp python index.

Example: [thoth-station/aicoe-ci-pulp-upload-example](https://github.com/thoth-station/aicoe-ci-pulp-upload-example)
to see how to configure AICoE-CI
to publish Python packages to your Pulp index.
