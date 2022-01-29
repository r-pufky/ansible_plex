# Plex
Plex Media Server supporting plexpass entitlements.

## Requirements
No additional requirements.

## Role Variables
Settings have been throughly documented for usage.

[defaults/main.yml](https://github.com/r-pufky/ansible_plex/blob/main/defaults/main/main.yml).

### Plex Preferences
If you do **not** have a pre-existing configuration: install without preferences
set and set those values after plex is configured.

Reasonable defaults have been provided but must be updated for your setup before
use.

Location: `/var/lib/plexmediaserver/Library/Application Support/Plex Media Server`

### Ports
All ports and protocols have been defined for the role.

Hosts should only define firewall rules for ports they need.

[defaults/ports.yml](https://github.com/r-pufky/ansible_plex/blob/main/defaults/main/ports.yml).

## Dependencies
N/A

## Example Playbook
host_vars/plex.example.com/vars/plex.yml
``` yaml
plex_library: '/data/media'

ports:
  - {proto: 'tcp', from_ip: 'any', to_port: 32400, direction: 'in', comment: 'plex media server access'}

plex_machine_identifier:                '{{ vault_plex_machine_identifier }}'
plex_processed_machine_identifier:      '{{ vault_plex_processed_machine_identifier }}'
plex_accepted_eula:                     '1'
plex_online_mail:                       'example@example.com'
plex_online_token:                      '{{ vault_plex_online_token }}'
plex_online_username:                   'example'
plex_publish_server_on_plex_online_key: '1'
```

site.yml
``` yaml
- name:   'plex media server'
  hosts:  'plex.example.com'
  become: true
  roles:
     - 'r_pufky.plex'
```

## Issues
Create a bug and provide as much information as possible.

Associate pull requests with a submitted bug.

## License
[AGPL-3.0 License](https://github.com/r-pufky/ansible_plex/blob/main/LICENSE)

## Author Information
https://keybase.io/rpufky
