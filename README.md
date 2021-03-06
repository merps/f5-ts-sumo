# F5 SumoLogic Refactor


[![license](https://img.shields.io/github/license/merps/f5-ts-sumo)](LICENSE)
[![standard-readme compliant](https://img.shields.io/badge/readme%20style-standard-brightgreen.svg?style=flat-square)](https://github.com/RichardLitt/standard-readme)

This document serves as an amendment to the well documented configuration guides for iApp and 
SumoLogic covering the work conducted with the assistance of Versent and SumoLogic development.

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
replicate existing Anayltics iApp. As this is merely an admendment, how-to guide, it will only document variations to already well documented procedures for both SumoLogic and F5.

This configuration outline currently only supports the deployment pattern as detailed in the diagram below,

<center><img src=images/config-diagram-autoscale-ltm.png width="400"></center>

## Prerequisites

To support this deployment pattern the following components are required:

* F5 BIP-IP (physical or VE)
* SumoLogic configured [HTTP Hosted Collector](https://help.sumologic.com/03Send-Data/Hosted-Collectors)
* F5 Toolchain Components:
    * [F5 Application Services v3 (AS3)](https://clouddocs.f5.com/products/extensions/f5-appsvcs-extension/latest/)
    * [F5 Telemetry Streaming (TS)](https://clouddocs.f5.com/products/extensions/f5-telemetry-streaming/latest/)
* [Postman](https://www.postman.com/)
* [SumoLogic](https://service.sumologic.com) Administrator account access.
* [Terraform CLI](https://www.terraform.io/docs/cli-index.html)
* [git](https://git-scm.com/)
* [AWS CLI](https://aws.amazon.com/cli/) access.
* [AWS Access Credentials](https://docs.aws.amazon.com/general/latest/gr/aws-security-credentials.html)


## Installation 

This section will over both the provisioning of the previously mentioned architecture using Terrafrom along
with how-to steps for SumoLogic Hosted Collectors.


### *AWS*

The deployment environment used for development is covered in detail [F5 AWAF Demo](https://github.com/merps/f5devops/f5-swg-aws),
this is a AWS Deployment example of AutoScaling AWAF. For simplicity, steps replicate this deployment are as follows;

***a)***    First, clone the repo:
```
git clone https://github.com/merps/f5devops.git
```

***b)***    Second, create a [tfvars](https://www.terraform.io/docs/configuration/variables.html) file in the following format to deploy the environment;

#### Inputs
Name | Description | Type | Default | Required
---|---|---|---|---
cidr | CIDR Range for VPC | String | *NA* | **Yes**
region | AWS Deployment Region | String | *NA* | **Yes**
azs | AWS Availability Zones | List | *NA* | **Yes** 
secops-profile | SecurityOperations AWS Profile | String | `default` | **Yes**
customer | Customer/Client Short name used for AWS Tag/Naming | String | `customer` | No
environment | Environment short-name name used for AWS Tag/Naming | String | `demo` | No
project | Project short-name name used for AWS Tag/Naming | String | `project` | No
ec2_key_name | EC2 KeyPair for Instance Creation | String | *NA* | **Yes**


***c)***    Third, intialise and plan the terraform deployment as follows:
```
cd f5devops/f5-sgw-aws/secure/
terraform init
terraform plan --vars-file ../variables.tfvars
```

this will produce and display the deployment plan using the previously created `variables.tfvars` file.


***d)***    Then finally to deploy the successful plan;
```
terraform apply --vars-file ../variables.tfvars
```

> **_NOTE:_**  This architecture deploys two c4.2xlage PAYG BIG-IP Marketplace instances, it is 
recommended to perform a `terraform destroy` to not incur excessive usage costs outside of free tier.


This deployment also covers the provisioning of the additional F5 prerequeset components so required for 
deployment example covered in the [F5 AWAF Demo](https://github.com/merps/f5devops/f5-swg-aws)


### *SumoLogic*


Provisioning of hosted collectors can be found locate [here](https://help.sumologic.com/03Send-Data/Hosted-Collectors "Hosted Collectors").  As per architecture deployment, to configure HTTP hosted collector for the consumption of TS:


1. [Login](https://service.sumologic.com) with Adminstrator account access.


2. Select Manage Data -> Collections


<center><img src=images/collection.png width="400"></center>


3. Select *"Add Collector"* - to add new Hosted Collection


<center><img src=images/new.png width="400"></center>


4. Select *"Hosted Collector"* for Collector Type.


<center><img src=images/collector_type.png width="400"></center>


5. Populate *"Add Hosted Collector"* dialog box as per below, then save.


<center><img src=images/collector_config.png width="400"></center>


6. Select **"OK"** to confirm new data source configuration


<center><img src=images/new_source.png width="400"></center>


7. Select *"HTTP Logs & Metrics"*


<center><img src=images/source_type.png width="400"></center>


8. Populate source configuration as per below, then save.


<center><img src=images/source_details.png width="400"></center>



9. Save complete *"Endpoint URL"* as vars and url will be used for the configuration of Telemetry
Streaming configuration.


<center><img src=images/source_endpoint.png width="400"></center>



## Configuration

Similar to the previous section this section will serve only as a quick configuration and amendment
to the existing SumoLogic and F5 Telemetry System Installation Guides.

### SumoLogic

For detailed instructions on how to [Import Content in Library](https://help.sumologic.com/05Search/Library/Export-and-Import-Content-in-the-Library) refer to that link.

For consistency, to import test dashboard for TS, perform the following steps;

1. [Login](https://service.sumologic.com) with Administrator account access.

2. Navigate to *"Personal"* folders, select ***Import*** from the options menu;

<center><img src=images/import.png width="400"></center>

3. Enter **Name** into **Content Import** dialog, this must be unique.

<center><img src=images/import_content.png width="400"></center>

4. Paste contents of [f5-sumo-ts.json](http://github.com/merps/f5devops/f5-sumo-ts/f5-sumo-ts.json) in **JSON** dialog.

5. Click import, this is only available if the json is valid.

### F5 BIG-IP

As with Sumo Logic, detailed instructions for the deployment and configuration for TS is located at 
[F5 Telemetry Streaming](https://clouddocs.f5.com/products/extensions/f5-telemetry-streaming/latest/) with detailed [Sumo Logic](https://clouddocs.f5.com/products/extensions/f5-telemetry-streaming/latest/setting-up-consumer.html#sumologic-ref) configuration instructions.

As previously, steps to configure;

1. From the previously created SumoLogic Hosted Collector, extract the follow:

    ***a)*** FQDN = Host

    ***b)*** path = uri

    ***c)*** ciphertext = encrypted token as part of uri

2. Update TS declarations with the variables as extracted from ***Endpoint URL*** as per example;

```json
{
    "class": "Telemetry",
    "TS_System": {
        "class": "Telemetry_System",
        "systemPoller": {
            "interval": 60,
            "enable": true,
            "trace": false,
            "actions": [
                {
                    "setTag": {
                        "tenant": "`T`",
                        "application": "`A`"
                    },
                    "enable": true
                }
            ]
        },
        "enable": true,
        "trace": false,
        "host": "localhost",
        "port": 8100,  # As per local listener deployment - refer to Usage
        "protocol": "http"
    },
    "TS_Listener": {
        "class": "Telemetry_Listener",
        "port": 6514, # As per local listener deployment - refer to Usage
        "enable": true,
        "trace": false,
        "match": "",
        "actions": [
            {
                "setTag": {
                    "tenant": "`T`",
                    "application": "`A`"
                },
                "enable": true
            }
        ]
    },
    "Poller":{ 
       "class":"Telemetry_System_Poller",
       "interval":60,
       "enable":true,
       "trace":false,
       "allowSelfSignedCert":false,
       "host":"localhost",
       "port":8100, # As per local listener deployment - refer to Usage
       "protocol":"http"
    },
    "SumoLogic_Consumer": {
        "class": "Telemetry_Consumer",
        "type": "Sumo_Logic",
        "host": "<SumoLogic Local Endpoint FQDN>",
        "protocol": "https", # Default 
        "port": 443, # Default 
        "enable": true,
        "trace": false,
        "path": "/receiver/v1/http/",
        "passphrase": {
            "cipherText": "<this is a secret>"
        }
    },
    "schemaVersion": "1.6.0"    
}
```
3. Push updated TS declaration to BIG-IP.

> **_NOTE:_** This configuration declares the external consumer for the `telemetry_traffic_log_profile` as
declared in the deployment environment [installation](#installation) section. 


# Usage

To replicate the Telemetry Streaming configuration for other environments this can be achieved by using
the Jinja templates located in [Telemetry Streaming(TS)](src/f5_ts/), these are;

- [as3-common-declaration.j2](src/f5_ts/as3-common-declaration.j2) defines Common BIG-IP Shared TS local listener.
- [ts-declaration.j2](src/f5_ts/ts-declaration.j2) defines TS configuration as outlined above.

For further reference to the use of AS3 and TS templates please refer to the [AS3 User Guide](https://clouddocs.f5.com/products/extensions/f5-appsvcs-extension/latest/userguide/)


## API

### As per TODO


## TODO

List of task to make the process my automated;

- [ ] Create work-flow for Jenkins/GitLab to deploy dashboard
- [x] ~~work-flow improvements for DO/AS3/TS~~  Added [Jinja](https://jinja.palletsprojects.com/en/2.11.x/) templates
- [ ] Usage Instructions(?)



## Contributing

See [the contributing file](CONTRIBUTING.md)!

PRs accepted.

### Any optional sections

## License

[Apache](../LICENSE)
