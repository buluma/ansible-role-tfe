# Ansible role [tfe](https://galaxy.ansible.com/ui/standalone/roles/buluma/tfe/documentation)

Install and configure tfe on your system.

|GitHub|Version|Issues|Pull Requests|Downloads|
|------|-------|------|-------------|---------|
|[![github](https://github.com/buluma/ansible-role-tfe/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-tfe/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-tfe.svg)](https://github.com/buluma/ansible-role-tfe/releases/)|[![Issues](https://img.shields.io/github/issues/buluma/ansible-role-tfe.svg)](https://github.com/buluma/ansible-role-tfe/issues/)|[![PullRequests](https://img.shields.io/github/issues-pr-closed-raw/buluma/ansible-role-tfe.svg)](https://github.com/buluma/ansible-role-tfe/pulls/)|[![Ansible Role](https://img.shields.io/ansible/role/d/buluma/tfe)](https://galaxy.ansible.com/ui/standalone/roles/buluma/tfe/documentation)|

## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/buluma/ansible-role-tfe/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
---
- name: Converge
  hosts: all
  become: true
  gather_facts: true

  roles:
    - role: buluma.tfe
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/buluma/ansible-role-tfe/blob/master/molecule/default/prepare.yml):

```yaml
---
- name: Prepare
  hosts: all
  become: true
  gather_facts: false

  roles:
    - role: buluma.bootstrap
    - role: buluma.core_dependencies
    - role: buluma.docker_ce
    - role: buluma.docker_compose

  post_tasks:
    # The role docker_ce skips starting on Docker hosts.
    - name: Start docker daemon
      ansible.builtin.service:
        name: docker
        state: started
        enabled: true
```

Also see a [full explanation and example](https://buluma.github.io/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/buluma/ansible-role-tfe/blob/master/defaults/main.yml):

```yaml
---
# defaults file for tfe

# Select the image to use for Terraform Enterprise. This includes the version.
# The latest tfe version can be found here:
# https://developer.hashicorp.com/terraform/enterprise/releases/2023/v202303-1
tfe_image: "images.releases.hashicorp.com/hashicorp/terraform-enterprise:v202309-1"

# Paste the license of Terraform Enterprise here. It's a long string.
# If the license is not set or empty, many tasks will be skipped, resulting in
# a non-working Terraform Enterprise instance. Not setting a license can help
# with testing.
tfe_license: ""

# Configure a hostname, used to redirect HTTP(S) requests.
tfe_hostname: "tfe.example.com"

# An encryption password for the TFE application.
tfe_encryption_password: "S0meP@ssword"

# A list of CIDR notated subnets that are allowed to create an "Initial Admin
# Token".
tfe_iact_subnets: []
#   - "10.0.0.0/8"
#   - "192.168.0.0/24"

# The following variables are used to configure the TLS certificate and key for
# the web interface of Terraform Enterprise. The certificate and key should be
# placed in the `files` directory of your playbook.
#
# You can create a self-signed certificate with the following command:
#
# openssl req -x509 -nodes -newkey rsa:4096 -keyout key.pem -out cert.pem \
# -sha256 -days 365
# cp cert.pem bundle.pem
tfe_tls_certificate: "cert.pem"
tfe_tls_key: "key.pem"
tfe_tls_bundle: "bundle.pem"

# You can set the operational mode to either: "disk", "external" or "active-active".
tfe_operational_mode: "active-active"

# Details on the database host. This host should already exist, this role
# does not create a database.
# These variables are required when `tfe_operational_mode` is set to `active-active` or `external`.
tfe_database_host: "tfe.RaNdOm.eu-west-1.rds.amazonaws.com"
tfe_database_user: "tfe"
tfe_database_password: "my_pass_c0mpl.x"
tfe_database_name: "tfe"
tfe_database_parameters: "sslmode=disable"

# Detail on the object storage. This role does not create the bucket.
# These variables are required when `tfe_operational_mode` is set to `active-active` or `external`.
tfe_object_storage_s3_endpoint: ""
tfe_object_storage_s3_use_instance_profile: false
tfe_object_storage_s3_bucket: "SomeBucketName"
tfe_object_storage_s3_access_key_id: ""
tfe_object_storage_s3_secret_access_key: ""
tfe_object_storage_s3_region: "eu-west-1"

# Details on the Redis host. This host should already exist, this role
# does not create a Redis instance.
# These variables are required when `tfe_operational_mode` is set to `active-active`.
tfe_redis_host: "tfe.RaNdOm.0001.euw1.cache.amazonaws.com"
tfe_redis_user: "tfe"
tfe_redis_password: "my_pass_c0mpl.x"
tfe_redis_use_tls: false
tfe_redis_use_auth: false

# The internal Vault requires an internal address of the node.
tfe_vault_cluster_address: "https://{{ ansible_default_ipv4.address }}:8201"
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/buluma/ansible-role-tfe/blob/master/requirements.txt).

## [State of used roles](#state-of-used-roles)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | Version |
|-------------|--------|--------|
|[buluma.bootstrap](https://galaxy.ansible.com/buluma/bootstrap)|[![Ansible Molecule](https://github.com/buluma/ansible-role-bootstrap/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-bootstrap/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-bootstrap.svg)](https://github.com/shadowwalker/ansible-role-bootstrap)|
|[buluma.core_dependencies](https://galaxy.ansible.com/buluma/core_dependencies)|[![Ansible Molecule](https://github.com/buluma/ansible-role-core_dependencies/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-core_dependencies/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-core_dependencies.svg)](https://github.com/shadowwalker/ansible-role-core_dependencies)|
|[buluma.docker_ce](https://galaxy.ansible.com/buluma/docker_ce)|[![Ansible Molecule](https://github.com/buluma/ansible-role-docker_ce/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-docker_ce/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-docker_ce.svg)](https://github.com/shadowwalker/ansible-role-docker_ce)|
|[buluma.docker_compose](https://galaxy.ansible.com/buluma/docker_compose)|[![Ansible Molecule](https://github.com/buluma/ansible-role-docker_compose/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-docker_compose/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-docker_compose.svg)](https://github.com/shadowwalker/ansible-role-docker_compose)|

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://buluma.github.io/) for further information.

Here is an overview of related roles:

![dependencies](https://raw.githubusercontent.com/buluma/ansible-role-tfe/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/buluma):

|container|tags|
|---------|----|
|[Debian](https://hub.docker.com/r/buluma/debian)|bullseye|
|[EL](https://hub.docker.com/r/buluma/enterpriselinux)|8, 9|
|[Fedora](https://hub.docker.com/r/buluma/fedora)|38, 39|
|[Ubuntu](https://hub.docker.com/r/buluma/ubuntu)|all|

The minimum version of Ansible required is 2.12, tests have been done to:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them in [GitHub](https://github.com/buluma/ansible-role-tfe/issues)

## [Changelog](#changelog)

[Role History](https://github.com/buluma/ansible-role-tfe/blob/master/CHANGELOG.md)

## [License](#license)

[Apache-2.0](https://github.com/buluma/ansible-role-tfe/blob/master/LICENSE)

## [Author Information](#author-information)

[Shadow Walker](https://buluma.github.io/)
