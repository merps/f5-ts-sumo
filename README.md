# F5 SumoLogic Refactor

![banner]()

![badge]()
![badge]()
[![license](https://img.shields.io/github/license/:merps/:f5-ts-sumo.svg)](LICENSE)
[![standard-readme compliant](https://img.shields.io/badge/readme%20style-standard-brightgreen.svg?style=flat-square)](https://github.com/RichardLitt/standard-readme)

This document serves as an admendment to the well documented configuration guides for iApp and 
SumoLogic covering the work conducted with the assistence of Versent and SumoLogic development.

## Table of Contents

- [Security](#security)
- [Background](#background)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
    - [AWS](#aws)
    - [SumoLogic](#sumologic)
- [Configuration](#configuration)
- [Usage](#usage)
- [API](#api)
- [Contributing](#contributing)
- [License](#license)


## Security

F5 Telemetry Streaming and the connection to SumoLogic is sent to a preconfigured HTTP Collector, 
as such egress is required for HTTP.


## Background

This work was undertaken to support Telemetry Streaming for Internet accessable BIG-IP's to, initially,
replicate existing Anayltics iApp.

As this is merely an admendment, how-to guide, it will only document variations to already well 
documented procedures for both SumoLogic and F5.

This configuration outline currently only supports the deployment pattern as detailed in the diagram below,

![alt text](https://github.com/thing/here "Reference Deployment")


## Prerequisites

To support this deployment pattern the following components are required:

* F5 BIP-IP (physical or VE)
* SumoLogic configured [HTTP Hosted Collector](https://help.sumologic.com/03Send-Data/Hosted-Collectors)
* F5 Toolchain Components:
    * [F5 Application Services v3 (AS3)](https://clouddocs.f5.com/products/extensions/f5-appsvcs-extension/latest/)
    * [F5 Telemetry Streaming (TS)](https://clouddocs.f5.com/products/extensions/f5-telemetry-streaming/latest/)
* [Postman](https://www.postman.com/)
* [Terraform CLI](https://www.terraform.io/docs/cli-index.html)
* [git](https://git-scm.com/)
* [AWS CLI](https://aws.amazon.com/cli/) access.
* [AWS Access Credentials](https://docs.aws.amazon.com/general/latest/gr/aws-security-credentials.html)


## Installation 

This section will over both the provisioning of the previously mentioned architecture using Terrafrom along
with how-to steps for SumoLogic Hosted Collectors

### AWS

The deployment environment used for development is coovered in detail [F5 AWAF Demo](https://github.com/merps/f5devops/f5-swg-aws),
this is a AWS Deployment example of AutoScaling AWAF. For simplicity, steps replicate this deployment are as follows;

First, clone the repo:

```
git clone https://github.com/merps/f5devops.git

```


Second, create a [tfvars](https://www.terraform.io/docs/configuration/variables.html) file in the following format to deploy the environment;

#### Inputs
Name | Description | Type | Default | Required
---|---|---|---|---
cidr | CIDR Range for VPC | String | *NA* | **Yes**
region | AWS Deployment Region | String | *NA* | **Yes**
azs | AWS Avaliability Zones | List | *NA* | **Yes** 
secops-profile | SecurityOperations AWS Profile | String | `default` | **Yes**
customer | Customer/Client Short name used for AWS Tag/Naming | String | `customer` | No
environment | Environment Shortname name used for AWS Tag/Naming | String | `demo` | No
project | Project Shortname name used for AWS Tag/Naming | String | `project` | No
ec2_key_name | EC2 KeyPair for Instance Creation | String | *NA* | **Yes**


Third, intialise and plan the terraform deployment as follows:

```
cd secure/
terraform init
terraform plan --vars-file ../variables.tfvars
```

this will produce and display the deployment plan using the previously created `varibles.tfvars` file.


Then finally to deploy the successfuly plan;
```
terraform apply --vars-file ../variables.tfvars
```



> **_NOTE:_**  This architecture deploys two c4.2xlage PAYG BIG-IP Marketplace instances, it is 
recommended to perfperform a `terraform destroy --vars-file` to not incur excessive usage costs 
outside of free tier.




This deployment also covers the provisioning of the additional F5 prerequeset components so required for 
deployment example covered in the [F5 AWAF Demo](https://github.com/merps/f5devops/f5-swg-aws)]


### SumoLogic

Provisioning of hosted collectors can be found locate [here](https://help.sumologic.com/03Send-Data/Hosted-Collectors "Hosted Collectors").


## Configuration

### Any optional sections

# Usage

### Any optional sections

## API

### Any optional sections

## More optional sections

## Contributing

See [the contributing file](CONTRIBUTING.md)!

PRs accepted.

Small note: If editing the Readme, please conform to the [standard-readme](https://github.com/RichardLitt/standard-readme) specification.

### Any optional sections

## License

[MIT © merps.](../LICENSE)
