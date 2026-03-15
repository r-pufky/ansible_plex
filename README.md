# Plex
Plex Media Server supporting PlexPass entitlements.

## [Requirements][i]
Requires [r_pufky.media][g] galaxy-ng collection. See
[reference documentation][h] for troubleshooting and config variables.

Install size: ~250MB

## Role Variables
Detailed variable use documented in defaults. See usage for role operation.

* [defaults][j] - User configurable options.

* [ports][k] - Ports are **not** managed (defined for external use).

## Usage

### Feature Flags
Tasks are gated by feature flags and executed in the following order.

  Step | Flag                 | Notes
 ------|----------------------|-------
  1    | plex_flg_backup      | Backup existing config, DB's, and exit.
  2    | plex_flg_restore     | Restore existing config, DB's, and exit.
  3    | plex_flg_maintenance | Preform role maintenance tasks.
  4    | plex_flg_install     | Install required packages, users, etc.
  5    | plex_flg_config      | Install user-defined config.
  7    | plex_flg_auto_update | Configure automatic plex update checks.
  8    | plex_flg_db_repair   | Configure automatic DB repair.

### Example Playbooks

#### New Deployments
Deploy role. [Manually sign-in to Plex to generate entitlement token][l].

``` yaml
- name: 'New Plex Deployment'
  ansible.builtin.include_role:
    name: 'r_pufky.media.plex'
```

Once configured take a backup of Plex configuration.

``` yaml
- name: 'Backup Plex configuration'
  ansible.builtin.include_role:
    name: 'r_pufky.media.plex'
  vars:
    plex_flg_backup: true
    plex_cfg_backup_dir: 'host_vars/plex/data'
```

See [Existing Deployments](#existing-deployments) for reproducible
deployments.

#### Existing Deployments
Configuration can optionally be templated and saved on the ansible controller
to make future Plex deployments reproducible. Static files may also be used if
there is no concern about secrets leaking.

``` yaml
- name: 'Deploy pre-configured Plex service'
  ansible.builtin.include_role:
    name: 'r_pufky.media.plex'
  vars:
    plex_flg_config: true
    plex_cfg_preferences: 'host_vars/plex/Preferences.xml.j2'
```

## Development
Configure [environment][a].

``` bash
# Run all tests.
molecule test --all
```

Testing variables:

  Variable          | type | Description
 -------------------|------|-------------
  url_inject_enable | bool | Disable **get_url** to inject files locally.

### [Releases][b]
Focused on service deployment with templated configuration to minimize role
churn due to inconsistent and rapid rolling release cycle.

  Release | Debian | Ansible | Plex          | Notes
 ---------|--------|---------|---------------|-------
  11.x.x  | 13     | 2.20    | 1.43.0.10492  | Update to Ansible 2.20 collection dependencies.
  10.x.x  | 13     | 2.20    | 1.43.0.10492  | Add restore feature flag.
  9.x.x   | 13     | 2.20    | 1.43.0.10467  | Ansible 2.20, feature flags, and semantic versioning.
  8.x.x   | 13     | 2.18    | 1.43.0.10389  | Last 'fully managed' config.
  7.x.x   | 13     | 2.18    | 1.42.2.10156  | Data annotations V3.
  6.x.x   | 13     | 2.18    | 1.42.1.10060  | Migrate to Debian Trixie.
  5.x.x   | 12     | 2.18    | 1.42.1.10060  | Data annotations V2.
  4.x.x   | 12     | 2.18    | 1.41.8.9834   | Data annotations.
  3.x.x   | 12     | 2.18    | 1.41.7.9799   | Use common role libraries.
  2.x.x   | 12     | 2.18    | 1.41.4.9399   | Ansible 2.18 support.
  1.x.x   | 12     | 2.12    | 1.25.4.5487   | Migration from private repository.

## Issues
Create a bug and provide as much information as possible.

Associate pull requests with a submitted bug.

## License
[AGPL-3.0 License][c] | [direct link][f]

## Author Information
PGP: [466EEC2B67516C7117C85CE3A0BC35D16698BAB9][d] | [github gist][e]


[a]: https://r-pufky.github.io/ansible_docs
[b]: https://semver.org/spec/v2.0.0
[c]: https://www.tldrlegal.com/license/gnu-affero-general-public-license-v3-agpl-3-0
[d]: https://keys.openpgp.org/vks/v1/by-fingerprint/466EEC2B67516C7117C85CE3A0BC35D16698BAB9
[e]: https://gist.github.com/r-pufky/a8df36977c55b5bb20829267c4c49d22

[f]: https://github.com/r-pufky/ansible_plex/blob/main/LICENSE
[g]: https://github.com/r-pufky/ansible_collection_media
[h]: http://r-pufky.github.io/docs/media/plex
[i]: https://github.com/r-pufky/ansible_plex/blob/main/meta/main.yml
[j]: https://github.com/r-pufky/ansible_plex/tree/main/defaults/main/main.yml
[k]: https://github.com/r-pufky/ansible_plex/blob/main/defaults/main/ports.yml
[l]: https://r-pufky.github.io/docs/media/plex