# Gitlab-CI runner module for Puppet

[![Build Status](https://travis-ci.org/voxpupuli/puppet-gitlab_ci_runner.png?branch=master)](https://travis-ci.org/voxpupuli/puppet-gitlab_ci_runner)
[![Puppet Forge](https://img.shields.io/puppetforge/v/puppet/gitlab_ci_runner.svg)](https://forge.puppetlabs.com/puppet/gitlab_ci_runner)
[![Puppet Forge - downloads](https://img.shields.io/puppetforge/dt/puppet/gitlab_ci_runner.svg)](https://forge.puppetlabs.com/puppet/gitlab_ci_runner)
[![Puppet Forge - endorsement](https://img.shields.io/puppetforge/e/puppet/gitlab_ci_runner.svg)](https://forge.puppetlabs.com/puppet/gitlab_ci_runner)
[![Puppet Forge - scores](https://img.shields.io/puppetforge/f/puppet/gitlab_ci_runner.svg)](https://forge.puppetlabs.com/puppet/gitlab_ci_runner)

#### Table of Contents

1. [Overview](#overview)
1. [Usage - Configuration options and additional functionality](#usage)
1. [Limitations - OS compatibility, etc.](#limitations)

## Overview

This module installs and configures the Gitlab CI Runner Package or nodes.

## Usage

Here is an example how to configure Gitlab CI runners using Hiera:

To use the Gitlab CI runners it is required to have the [puppetlabs/docker](https://forge.puppetlabs.com/puppetlabs/docker) module.

`$manage_docker` can be set to false if docker is managed externally.

`$manage_compat` should be set to false in new configurations, as it allows access to configuration keys that have a '\_' in their name.

```yaml
gitlab_ci_runner::concurrent: 4

gitlab_ci_runner::check_interval: 4

gitlab_ci_runner::metrics_server: "localhost:8888"

gitlab_ci_runner::manage_docker: true

gitlab_ci_runner::config_path: "etc/gitlab-runner/config.toml"

gitlab_ci_runner::config_compat: false

gitlab_ci_runner::runners:
  test_runner1:{}
  test_runner2:{}
  test_runner3:
    url: "https://git.alternative.org/ci"
    registration-token: "abcdef1234567890"
    tags-list: "aws,docker,example-tag"

gitlab_ci_runner::runner_defaults:
  url: "https://git.example.com/ci"
  registration-token: "1234567890abcdef"
  executor: "docker"
  docker-image: "ubuntu:focal"
  builds-dir: "/tmp"
  cache-dir: "/tmp"
```

To unregister a specific runner you may use `ensure` param:

```yaml
gitlab_ci_runner::runners:
  test_runner1:{}
  test_runner2:{}
  test_runner3:
    url: "https://git.alternative.org/ci"
    registration-token: "abcdef1234567890"
    ensure: absent
```

## Limitations

The Gitlab CI runner installation is at the moment only tested on:
* CentOS 6/7/8
* Debian 8/9/10
* Ubuntu 16.04/18.04

A runner configuration is currently only applied if the specific runner does not exist in the config file.
