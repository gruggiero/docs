---
title: terraform-aws-vpc
description: Terraform module that defines a VPC with Internet Gateway.
---

# Terraform AWS VPC

|                  |                                                                                                                                                |
|:-----------------|:-----------------------------------------------------------------------------------------------------------------------------------------------|
| GitHub Repo      | <https://github.com/cloudposse/terraform-aws-vpc>                                                                                              |
| Terraform Module | terraform-aws-vpc                                                                                                                              |
| Release          | [![Release](https://img.shields.io/github/release/cloudposse/terraform-aws-vpc.svg)](https://github.com/cloudposse/terraform-aws-vpc/releases) |
| Build Status     | [![Build Status](https://travis-ci.org/cloudposse/terraform-aws-vpc.svg)](https://travis-ci.org/cloudposse/terraform-aws-vpc)                  |

# Usage

## Quick Start Example

### HCL

```hcl
module "vpc" {
  source                           = "git::https://github.com/cloudposse/terraform-aws-vpc.git?ref=master"
  domain_name                      = "example.com"
  proces_domain_validation_options = "true"
  ttl                              = "300"
}
```

## Full Example

Here's a complete example using [`terraform-aws-dynamic-subnets`](https://github.com/cloudposse/terraform-aws-dynamic-subnets.git).

### HCL

```hcl
module "vpc" {
  source                           = "git::https://github.com/cloudposse/terraform-aws-vpc.git?ref=master"
  domain_name                      = "example.com"
  proces_domain_validation_options = "true"
  ttl                              = "300"
}

module "dynamic_subnets" {
  source             = "git::https://github.com/cloudposse/terraform-aws-dynamic-subnets.git?ref=master"
  availability_zones = "${var.availability_zones}"
  namespace          = "${var.namespace}"
  name               = "${var.name}"
  stage              = "${var.stage}"
  region             = "${var.region}"
  vpc_id             = "${module.vpc.vpc_id}"
  igw_id             = "${module.vpc.igw_id}"
  cidr_block         = "${module.vpc.vpc_cidr_block}"
}
```

# Inputs

| Name                               | Default       | Description                                                                      | Required |
|:-----------------------------------|:--------------|:---------------------------------------------------------------------------------|:---------|
| `assign_generated_ipv6_cidr_block` | `false`       | Requests an Amazon-provided IPv6 CIDR block with a /56 prefix length for the VPC | No       |
| `name`                             | ``            | Name (e.g. `bastion` or `db`)                                                    | Yes      |
| `attributes`                       | `[]`          | Additional attributes (e.g. `policy` or `role`)                                  | No       |
| `tags`                             | `{}`          | Additional tags (e.g. `map("BusinessUnit","XYZ")`                                | No       |
| `delimiter`                        | `-`           | Delimiter to be used between `name`, `namespace`, `stage`, etc.                  | No       |
| `cidr_block`                       | `10.0.0.0/16` | CIDR for the VPC                                                                 | No       |
| `enable_classiclink`               | `false`       | A boolean flag to enable/disable ClassicLink for the VPC                         | No       |
| `enable_classiclink_dns_support`   | `false`       | A boolean flag to enable/disable ClassicLink DNS Support for the VPC             | No       |
| `enable_dns_hostnames`             | `true`        | A boolean flag to enable/disable DNS hostnames in the VPC                        | No       |
| `enable_dns_support`               | `true`        | A boolean flag to enable/disable DNS support in the VPC                          | No       |
| `instance_tenancy`                 | ``            | A tenancy option for instances launched into the VPC                             | No       |
| `namespace`                        | ``            | Namespace (e.g. `cp` or `cloudposse`)                                            | Yes      |
| `stage`                            | ``            | Stage (e.g. `prod`, `dev`, `staging`)                                            | Yes      |

# Outputs

| Name                            | Description                                                     |
|:--------------------------------|:----------------------------------------------------------------|
| `igw_id`                        | The ID of the Internet Gateway                                  |
| `ipv6_cidr_block`               | The IPv6 CIDR block                                             |
| `vpc_cidr_block`                | The CIDR block of the VPC                                       |
| `vpc_default_network_acl_id`    | The ID of the network ACL created by default on VPC creation    |
| `vpc_default_route_table_id`    | The ID of the route table created by default on VPC creation    |
| `vpc_default_security_group_id` | The ID of the security group created by default on VPC creation |
| `vpc_id`                        | The ID of the VPC                                               |
| `vpc_ipv6_association_id`       | The association ID for the IPv6 CIDR block                      |
| `vpc_main_route_table_id`       | The ID of the main route table associated with this VPC.        |
