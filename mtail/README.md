# mtail Ansible Role

An Ansible role for installing and configuring [mtail](https://github.com/google/mtail), Google's log parser that extracts metrics from application logs for monitoring.

## Features

- Downloads and installs mtail binary from official GitHub releases
- Creates dedicated system user and directories
- Configures mtail as a systemd service
- Supports custom mtail programs for log parsing
- Configurable log files and metrics export

## Requirements

- Ansible 2.9+
- Target systems: RHEL/CentOS, Debian Based Distros

## Usage

```yaml
- name: Install and configure mtail
  hosts: servers
  become: yes
  roles:
    - mtail
  vars:
    mtail_logs:
      - /var/log/nginx/access.log
    mtail_progs:
      - name: nginx_requests
        content: |
             counter nginx_status_codes by code
             
             /^(\S+) \S+ \S+ \[[^]]+\] "\S+ \S+ \S+" (?P<code>\d{3}) / {
                 nginx_status_codes[$code]++
             }
```

## Key Variables

- `mtail_version`: mtail version to install (default: "3.0.3")
- `mtail_logs`: List of log files to monitor
- `mtail_progs`: Custom mtail programs for parsing logs
- `mtail_metrics_port`: Port for metrics export (default: 3903)

## Example

See `examples/playbook.yml` for a complete usage example.
