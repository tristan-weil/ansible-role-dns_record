# Ansible Role: dns_record

An Ansible Role that allows to update domains records.

Currently supported providers:
- Gandi (gandi)
- nsupdate (nsupdate)

Common record's type should be supported. 
In addition, the following types are also supported:
- SSHFP

[![Actions Status](https://github.com/tristan-weil/ansible-role-dns_record/workflows/molecule/badge.svg?branch=master)](https://github.com/tristan-weil/ansible-role-dns_record/actions)

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

Mandatory variables:

| Variable      | Description |
| :------------ | :---------- |
| dns_record_providers | a dictionnary of <*provider*> |
| dns_record_list | a list of <*record*> |

Optional variables:

| Variable      | Default | Description |
| :------------ | :------ | :---------- |
| dns_record_default_domain | machine's domain | default domain to use in <*record*> |
| dns_record_default_hostname | machine's hostname | default hostname to use in <*record*> |
| dns_record_default_ttl | 3600 | default TTL to use in <*record*> |
| dns_record_default_type | | default type to use in <*record*> |
| dns_record_default_value | | default value to use in <*record*> |
| dns_record_default_state | present | *present/absent*  default state to use in <*record*> |
| dns_record_default_config | {} | defaut extra config to use in <*record*> |

## <*provider*>

A *provider* configures the access to a DNS provider.

Mandatory variables:

| Variable      | Description |
| :------------ | :---------- |
| name | the name of the provider |

Optional variables:

| Variable      | Default | Description |
| :------------ | :------ | :---------- |

Additionnal variables:
- <*provider gandi*>
- <*provider nsupdate*>

### <*provider gandi*> 

Mandatory variables:

| Variable      | Description |
| :------------ | :---------- |
| auth_token | the auth token |

Optional variables:

| Variable      | Default | Description |
| :------------ | :------ | :---------- |

### <*provider nsupdate*>

Mandatory variables:

| Variable      | Description |
| :------------ | :---------- |
| key_name | the name of the key |
| key_secret | the secret |

Optional variables:

| Variable      | Default | Description |
| :------------ | :------ | :---------- |
| key_algorithm | hmac-sha512 | the algorithm of the secret |
| server | 127.0.0.1 | the address of the server |
| port | 53 | the port of the server |

## <*record*>

A *record* represents a common record.

Mandatory variables:

| Variable      | Description |
| :------------ | :---------- |
| provider | the provider to use (must be registred) |
| domain | the domain |
| hostname | the hostname (short) |
| type | the type of the record (A, AAAA, etc.) |

Optional variables:

| Variable      | Default | Description |
| :------------ | :------ | :---------- |
| value | | the value of record (can be generated) |
| state | present | *present/absent* to add or delete a record |

Additionnal variables:
- <*record SSHFP*>

## <*record SSHFP*>

For the SSHFP record, the *value* variable is not needed as it will be generated.

Mandatory variables:

| Variable      | Description |
| :------------ | :---------- |
| sshfp_pub_key_path | the path of the public key |
| sshfp_pub_key_type | the key algorithm type |
| sshfp_pub_key_hash | the key algorithm hash |

Optional variables:

| Variable      | Default | Description |
| :------------ | :------ | :---------- |

## Example Playbook

    - hosts: 'webservers'
      tasks:
        - import_role: 
            name: 'ansible-role-dns_record'
          vars:
            dns_record_providers:
              gandi:
                name: 'gandi'
                auth_token: "xxxxx"

            dns_record_list:
              - provider: 'gandi'
                domain: 'terror.ninja'
                hostname: "{{ ansible_facts['hostname'] }}"
                type: 'SSHFP'
                sshfp_pub_key_path: '/etc/ssh/ssh_host_ed25519_key.pub'
                sshfp_pub_key_type: 'ed25519'
                sshfp_pub_key_hash: 'sha256'
            
              - provider: 'gandi'
                domain: 'terror.ninja'
                hostname: "{{ ansible_facts['hostname'] }}"
                type: 'A'
                value: "{{ ansible_eth0.ipv4.address }}"

## Todo

Find a way to test the role.

Add more providers / types.

## Dependencies

See [requirements_galaxy.yml](https://github.com/tristan-weil/ansible-role-dns_record/blob/master/requirements_galaxy.yml)

## Supported platforms

See [meta/main.yml](https://github.com/tristan-weil/ansible-role-dns_record/blob/master/meta/main.yml)

## License

See [LICENSE.md](LICENSE.md)
