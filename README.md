# F5 SumoLogic Refactor

![banner]()

![badge]()
![badge]()
[![license](https://img.shields.io/github/license/:user/:repo.svg)](LICENSE)
[![standard-readme compliant](https://img.shields.io/badge/readme%20style-standard-brightgreen.svg?style=flat-square)](https://github.com/RichardLitt/standard-readme)

This document serves as an admendment to the well documented configuration guides for iApp and 
SumoLogic covering the work conducted with the assistence of Versent and SumoLogic development.

## Table of Contents

- [Security](#security)
- [Background](#background)
- [Install](#install)
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

This configuration supports the deployment pattern as below:

![alt text](https://github.com/thing/here "Deployment Example")

### Any optional sections

## Prerequisites

To support this deployment pattern the following components are required:

* F5 BIP-IP (physical or VE)
* SumoLogic configured [HTTP Hosted Collector](https://help.sumologic.com/03Send-Data/Hosted-Collectors "Hosted Collectors").
* F5 Toolchain Components"
    * [F5 Application Services v3 (AS3)](https://clouddocs.f5.com/products/extensions/f5-appsvcs-extension/latest/)
    * [F5 Telemetry Streaming (TS)](https://clouddocs.f5.com/products/extensions/f5-telemetry-streaming/latest/)
* [Postman](https://www.postman.com/)

Optional

## Install

This module depends upon a knowledge of [Markdown]().

```
```

### Any optional sections

## Usage

```
```

Note: The `license` badge image link at the top of this file should be updated with the correct `:user` and `:repo`.

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

[MIT © Richard McRichface.](../LICENSE)
