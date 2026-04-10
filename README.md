# Satisfactory
Satisfactory dedicated server.

## [Requirements][i]
Requires [r_pufky.game][g] galaxy-ng collection.

  Resource | Minimum | Recommended
 ----------|---------|-------------
  CPU      | 4c/4t   | 4c/8t @3.8Ghz+ (Intel i5-3570+ or Ryzen 5 3600+).
  RAM      | 12GB    | 16GB+ (4+ players).
  Disk     | 5GB     | 6GB.

Heavily favors single core speed. Do not use **kvm64** VM processor types.

> Tasks [potentially touching Network Mounted Filesystems][p] will be run as
> the task user and fallback to the service user. Manage these locations
> externally if these fail.

## Role Variables
Detailed variable use documented in defaults. See usage for role operation.

* [defaults][j] - User configurable options.

* [ports][k] - Ports are **not** managed (defined for external use).

## Usage
A new server [must be claimed to be used][l]. Once claimed a game may be
created on the server to host. These settings are stored in a
[binary save (.sav)][m] file and not externally editable.

### Claim a server
Client Game ➔ Server Manager ➔ Add server
* Set server name.
* Set admin password.
* Create or load a game to host.

### Feature Flags
Tasks are gated by feature flags and executed in the following order.

  Step | Flag                    | Notes
 ------|-------------------------|-------
  1    | satisfactory_flg_update | Update server on launch or if already installed.
  2    | satisfactory_flg_config | Set configuration files.
  3    | satisfactory_flg_backup | Enable local scheduled backup.

### Example Playbooks
Server will automatically populate any required .ini file on initial shutdown
if it does not exist on service start. Any pre-existing .ini file will not be
touched and assumed correct.

After the first-run setup [the server must be claimed](#claim-a-server).

#### Default Server

``` yaml
- name: 'Configure a default satisfactory server with backups.'
  ansible.builtin.include_role:
     name: 'r_pufky.game.satisfactory'
  vars:
    satisfactory_flg_backup: true
```

#### Custom Server
[Configuration files][o] will be interpreted as templates, allowing for vault
use of server configuration files. Files in this directory will be sync'ed to
the server. See [examples in files][n].

``` yaml
- name: 'Satisfactory with .'
  ansible.builtin.include_role:
     name: 'r_pufky.game.satisfactory'
  vars:
    satisfactory_flg_backup: true
    satisfactory_flg_config: true
    satisfactory_cfg_d: 'host_vars/sat.example.com/config'
```

## Development
Configure [environment][a].

``` bash
# Run all tests.
molecule test --all
```

Testing variables:

  Variable            | Type | Description
 ---------------------|------|-------------
  molecule_flg_inject | bool | Disable **get_url** to inject files locally.

### [Releases][b]

  Release | Debian | Ansible | Notes
 ---------|--------|---------|-------
  2.x.x   | 13     | 2.20    | Ansible 2.20, feature flags, and semantic versioning.
  1.x.x   | 12     | 2.11    | Migration from private repository.

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

[f]: https://github.com/r-pufky/ansible_satisfactory/blob/main/LICENSE
[g]: https://github.com/r-pufky/ansible_collection_game
[i]: https://github.com/r-pufky/ansible_satisfactory/blob/main/meta/main.yml
[j]: https://github.com/r-pufky/ansible_satisfactory/tree/main/defaults/main/main.yml
[k]: https://github.com/r-pufky/ansible_satisfactory/blob/main/defaults/main/ports.yml
[l]: https://satisfactory.wiki.gg/wiki/Dedicated_servers#Claiming_the_Server_and_Starting_a_Game
[m]: https://satisfactory.wiki.gg/wiki/Save_files
[n]: https://github.com/r-pufky/ansible_satisfactory/tree/main/files
[o]: https://satisfactory.wiki.gg/wiki/Dedicated_servers/Configuration_files
[p]: https://r-pufky.github.io/ansible_docs/best_practice/patterns/#network-mounts