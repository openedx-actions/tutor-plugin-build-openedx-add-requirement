<img src="https://avatars.githubusercontent.com/u/40179672" width="75">

[![hack.d Lawrence McDaniel](https://img.shields.io/badge/hack.d-Lawrence%20McDaniel-orange.svg)](https://lawrencemcdaniel.com)
[![discuss.overhang.io](https://img.shields.io/static/v1?logo=discourse&label=Forums&style=flat-square&color=ff0080&message=discuss.overhang.io)](https://discuss.overhang.io)
[![docs.tutor.overhang.io](https://img.shields.io/static/v1?logo=readthedocs&label=Documentation&style=flat-square&color=blue&message=docs.tutor.overhang.io)](https://docs.tutor.overhang.io)<br/>
[![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white)](https://aws.amazon.com/)
[![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![Kubernetes](https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white)](https://kubernetes.io/)

# tutor-plugin-build-openedx-add-requirement

Github Action that uses Tutor to add a Python requirement, expressed as either a Github repository or a PyPi package name with an optional version constraint. Adds the requirement to private.txt and if necessary, downloads the repository.

This action was originally created to work seamlessly as a supporting action for [openedx-actions/tutor-plugin-build-openedx](https://github.com/openedx-actions/tutor-plugin-build-openedx) but it should also work with your own custom workflows.

## Usage

```yaml
name: Example workflow

on: workflow_dispatch

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      # required antecedent
      - uses: actions/checkout@v3.5.0

      # required antecedent
      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v2
        with:
          aws-access-key-id: ${{ secrets.THE_NAME_OF_YOUR_AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.THE_NAME_OF_YOUR_AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-2

      # install and configure tutor
      - name: Configure Github workflow environment
        uses: openedx-actions/tutor-k8s-init@v1.0.8

      # THIS ACTION WITH A PYPI PACKAGE:
      - name: Add django-debug-toolbar
        uses: openedx-actions/tutor-plugin-build-openedx-add-requirement@v1.0.5
        with:
          pip-package: django-debug-toolbar
          pip-package-version: ">=3.7.0"

      # THIS ACTION WITH A GITHUB REPOSITORY:
      #   repository-token is an optional input with a default value of ''
      - name: Add the edx-ora2 XBlock
        uses: openedx-actions/tutor-plugin-build-openedx-add-requirement@v1.0.7
        with:
          repository: edx-ora2
          repository-organization: openedx
          repository-ref: master
          repository-token: ${{ secrets.PAT }}

      #
      # ... more configuration stuff ...
      #

      # Build your openedx container
      - name: Build the image and upload to AWS ECR
        uses: openedx-actions/tutor-plugin-build-openedx@v1.0.0
```
