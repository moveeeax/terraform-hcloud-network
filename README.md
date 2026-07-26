# terraform-hcloud-network

> Declare the private network once, hand out subnet IDs to everything else.

**Status:** 🚧 In development

## Overview

Terraform module that manages a Hetzner Cloud private network and its subnets. It creates one network with a configurable IP range and renders subnets from a list variable, exposing the network and subnet IDs for server attachment.

## Features

- Creates one `hcloud_network` with an explicit RFC 1918 `ip_range`
- Renders `hcloud_network_subnet` resources with `for_each` over a subnet list, so adding a subnet never re-creates its neighbours
- Per-subnet network zone and type, defaulting to `cloud` in `eu-central`
- Validates that every subnet range is contained in the network range before the API sees it
- Outputs the network ID and a name-to-ID map of subnets for server and route modules
- Optional route and exposed-route-to-vSwitch toggles kept out of the happy path

## Stack

Terraform + the hetznercloud/hcloud provider.

## Usage

```hcl
module "network" {
  source = "github.com/moveeeax/terraform-hcloud-network"

  name     = "prod"
  ip_range = "10.0.0.0/16"

  subnets = [
    { name = "app", ip_range = "10.0.1.0/24", network_zone = "eu-central" },
    { name = "db",  ip_range = "10.0.2.0/24", network_zone = "eu-central" },
  ]

  labels = {
    env = "prod"
  }
}
```

## License

MIT
