# Ansible Role: HAProxy

[![Build Status](https://travis-ci.org/pastir/haproxy.svg?branch=master)](https://travis-ci.org/pastir/haproxy)
[![License](https://img.shields.io/badge/license-MIT%20License-brightgreen.svg)](https://opensource.org/licenses/MIT)

Ansible role for installing and configuring HAProxy - a high-performance load balancer and proxy server.

## Requirements

- Ansible 2.9 or higher
- Ubuntu 20.04 (Focal), 22.04 (Jammy), 24.04 (Noble)
- Debian 10 (Buster), 11 (Bullseye), 12 (Bookworm)

## Supported HAProxy Versions

- 2.9.x (Focal 20.04, Jammy 22.04, Noble 24.04)
- 3.0.x (Focal 20.04, Jammy 22.04, Noble 24.04)
- 3.1.x (Noble 24.04)

## Role Variables

Main role variables are defined in `defaults/main.yml`:

```yaml
# HAProxy version
haproxy_major_version: "2.9"
haproxy_version: 2.9.*

# HAProxy configuration
haproxy_config:
  - section: global
    log:
      - /dev/log    local0
      - /dev/log    local1 notice
    chroot: /var/lib/haproxy
    stats:
      - socket: /run/haproxy/admin.sock mode 660 level admin
      - timeout: 30s
    user: haproxy
    group: haproxy
    # SSL settings
    ssl-default-bind-ciphers: |
      ECDHE-ECDSA-AES128-GCM-SHA256:
      ECDHE-RSA-AES128-GCM-SHA256:
      ECDHE-ECDSA-AES256-GCM-SHA384:
      ECDHE-RSA-AES256-GCM-SHA384:
      ECDHE-ECDSA-CHACHA20-POLY1305:
      ECDHE-RSA-CHACHA20-POLY1305:
      DHE-RSA-AES128-GCM-SHA256:
      DHE-RSA-AES256-GCM-SHA384
    ssl-default-bind-ciphersuites: TLS_AES_128_GCM_SHA256:TLS_AES_256_GCM_SHA384:TLS_CHACHA20_POLY1305_SHA256
    ssl-default-bind-options: ssl-min-ver TLSv1.2 no-tls-tickets
```

## Usage

### Basic Installation

```yaml
- hosts: haproxy_servers
  roles:
    - role: pastir.haproxy
```

### Installation with Custom Configuration

```yaml
- hosts: haproxy_servers
  roles:
    - role: pastir.haproxy
      vars:
        haproxy_major_version: "3.0"
        haproxy_version: 3.0.*
        haproxy_config:
          - section: global
            log:
              - /dev/log    local0
              - /dev/log    local1 notice
            chroot: /var/lib/haproxy
            stats:
              - socket: /run/haproxy/admin.sock mode 660 level admin
              - timeout: 30s
            user: haproxy
            group: haproxy
```

## Tags

- `install` - HAProxy installation
- `configure` - HAProxy configuration

## License

MIT

## Author

Role created and maintained by [pastir](https://github.com/pastir)
