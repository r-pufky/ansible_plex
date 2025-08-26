# Plex
Plex Media Server supporting PlexPass entitlements.

## Requirements
[supported platforms](https://github.com/r-pufky/ansible_plex/blob/main/meta/main.yml)

## Role Variables
[defaults](https://github.com/r-pufky/ansible_plex/tree/main/defaults/main)

## Ports
All ports and protocols have been defined for the role.

[defaults/ports.yml](https://github.com/r-pufky/ansible_plex/blob/main/defaults/main/ports.yml)

## Dependencies
**galaxy-ng** roles cannot be used independently. Part of
[r_pufky.srv](https://github.com/r-pufky/ansible_collection_srv) collection.

## New Plex Installation
Read defaults documentation.

### Plex Online Token
Obtain your `plex_cfg_online_token` via [this support article.](https://support.plex.tv/articles/204059436-finding-an-authentication-token-x-plex-token/)

Alternatively, the initial manual setup process to be run locally. Use a SSH
tunnel to access the server-side configuration page; if it cannot be accessed
locally.

``` bash
ssh -L 32400:localhost:32400 {plex_host}
```

Connect to http://localhost:32400/web and run through configuration steps:
* Select media libraries to use.
* Sign-in on server (upper right --> sign-in)
* Select server and claim (claim now --> claim server)
* Once claimed, the access token and configured preferences are located in
  `Preferences.xml`.

### Plex auto-generated hardware values
Plex generates unique UUID's per installation to uniquely identify the server.

Retention of auto-generated values enables Plex role usage without needing to
care about the unique per-machine identifiers for Plex configurations while
still applying desired configuration state. This can be left on and the
auto-generated values will always be used.

If you do **not** have a pre-existing configuration, enable retention of
auto-generated values, execute the role, and place auto-generated values into
the host config:

``` yaml
- name: 'New Plex installation'
  ansible.builtin.include_role:
    name: 'r_pufky.srv.plex'
  vars:
    plex_srv_keep_auto_generated_values: true
```

`{plex_srv_application_support_dir}/Plex Media Server/Preferences.xml`
``` xml
<Preferences MachineIdentifier="..." ProcessedMachineIdentifier="..." AnonymousMachineIdentifier="..." HardwareDevicePath="..." .../>
```

host_vars/plex.example.com/vars/plex.yml
``` yaml
plex_srv_keep_auto_generated_values: false
plex_cfg_machine_identifier: '...'
plex_cfg_processed_machine_identifier: '...'
plex_cfg_anonymous_machine_identifier: '...'
plex_cfg_hardware_device_path: '...'
```

This will enable idempotent role application on different machines.

## Example Playbook
host_vars/plex.example.com/vars/plex.yml
``` yaml
plex_cfg_online_username: 'example'
plex_cfg_online_mail: 'example@example.com'
plex_cfg_online_token: '{{ vault_plex_cfg_online_token }}'
plex_srv_keep_auto_generated_values: false
plex_cfg_machine_identifier: '{{ vault_plex_cfg_machine_identifier }}'
plex_cfg_processed_machine_identifier: '{{ vault_plex_cfg_processed_machine_identifier }}'
plex_cfg_anonymous_machine_identifier: '{{ vault_plex_cfg_anonymous_machine_identifier }}'
plex_cfg_online_home: true
plex_cfg_publish_server_on_plex_online_key: '1'
plex_srv_transcode_memory: '1G'
plex_cfg_transcoder_temp_directory: '/tmp'
plex_cfg_transcoder_tone_mapping: true
plex_cfg_hardware_accelerated_codecs: true
plex_cfg_hardware_accelerated_encoders: true
plex_cfg_transcoder_hevc_encoding: true
plex_cfg_transcoder_hevc_optimize: true
```

Configure Plex server.
``` yaml
- name: 'Manage Plex'
  ansible.builtin.include_role:
    name: 'r_pufky.srv.plex'
```

## Development
Configure [environment](https://github.com/r-pufky/ansible_collection_srv/blob/main/docs/dev/environment/README.md)

Run all unit tests:
``` bash
molecule test --all
```

### Releases
Release format: **{OS}-{SERVICE}-{ROLE}**

Each type inherits the versioning system used; defaulting to schematic
versioning.

`12.0.0-2.0.3-1.0.0`

* 12.0.0 - Debian 12 (bookworm).
* 2.0.3 - Service/app version.
* 1.0.0 - Role version.

Releases are branched on Debian releases:

* **[13.x.x](https://github.com/r-pufky/ansible_plex)**: 13 Trixie.
* **[12.x.x](https://github.com/r-pufky/ansible_plex/tree/12.x)**: 12 Bookworm.

### Issues
Create a bug and provide as much information as possible.

Associate pull requests with a submitted bug.

## License
[AGPL-3.0 License](https://www.tldrlegal.com/license/gnu-affero-general-public-license-v3-agpl-3-0)
 [(direct link)](https://github.com/r-pufky/ansible_plex/blob/main/LICENSE)

## Author Information
PGP Fingerprint: [466EEC2B67516C7117C85CE3A0BC35D16698BAB9](https://keys.openpgp.org/vks/v1/by-fingerprint/466EEC2B67516C7117C85CE3A0BC35D16698BAB9)
| [github gist](https://gist.github.com/r-pufky/a8df36977c55b5bb20829267c4c49d22)
