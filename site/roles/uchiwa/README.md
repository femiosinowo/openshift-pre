uchiwa
======

Role to install Uchiwa dashboard for Sensu.

The configuration of the role is done in such way that it should not be necessary
to change the role for any kind of configuration. All can be done either by
changing role parameters or by declaring completely new configuration as a
variable. That makes this role absolutely universal. See the examples below for
more details.

Please report any issues or send PR.


Examples
--------

```
# Example of usage with default configuration
- hosts: myhost1
  roles:
    - uchiwa

# Example of how to adapt the default configuration
- hosts: myhost2
  vars:
    uchiwa_sensu_site: "DataCentre 1"
    uchiwa_sensu_ssl_enabled: "true"
    uchiwa_sensu_timeout: 5
    uchiwa_uchiwa_user: "uchiwa"
    uchiwa_uchiwa_password: "password"
  roles:
    - uchiwa

# Example of how to add more Sensu servers
- hosts: myhost3
  vars:
    uchiwa_sensu__custom:
      - name: Sensu2
        ...
      - name: Sensu3
        ...
  roles:
    - uchiwa
```

See [Uchiwa documentation](https://uchiwa.io/#/docs/config) for more information
about the configuration.


Role variables
--------------

List of variables used by the role:

```
# YUM repo URL
uchiwa_yumrepo_url: http://repos.sensuapp.org/yum/el/$releasever/$basearch/

# Custom yumrepo params
uchiwa_yumrepo_params: {}

# Package to be installed (exlicit version can be specified here)
uchiwa_pkg: uchiwa

# Allow uchiwa to bind to TCP port <1024
uchiwa_allow_low_port: false


# Default values of the Sensu part of the config
uchiwa_sensu_site: Sensu
uchiwa_sensu_host: 127.0.0.1
uchiwa_sensu_ssl_enabled: "false"
uchiwa_sensu_port: 4567
uchiwa_sensu_user: ""
uchiwa_sensu_pass: ""
uchiwa_sensu_path: ""
uchiwa_sensu_timeout: 5000

# Default Sensu part of the config
uchiwa_sensu__default:
  - name: "{{ uchiwa_sensu_site }}"
    host: "{{ uchiwa_sensu_host }}"
    ssl: "{{ uchiwa_sensu_ssl_enabled }}"
    port: "{{ uchiwa_sensu_port }}"
    user: "{{ uchiwa_sensu_user }}"
    pass: "{{ uchiwa_sensu_pass }}"
    path: "{{ uchiwa_sensu_path }}"
    timeout: "{{ uchiwa_sensu_timeout }}"

# Custom Sensu part of the config
uchiwa_sensu__custom: []

# Final Sensu part of the config
uchiwa_sensu: "{{
  uchiwa_sensu__default +
  uchiwa_sensu__custom }}"

# Default values of the Uchiwa part of the config
uchiwa_uchiwa_host: 0.0.0.0
uchiwa_uchiwa_port: 3000
uchiwa_uchiwa_user: admin
uchiwa_uchiwa_pass: admin
uchiwa_uchiwa_refresh: 10000

# Default Uchiwa part of the config
uchiwa_uchiwa__default:
  host: "{{ uchiwa_uchiwa_host }}"
  user: "{{ uchiwa_uchiwa_user }}"
  pass: "{{ uchiwa_uchiwa_pass }}"
  port: "{{ uchiwa_uchiwa_port }}"
  refresh: "{{ uchiwa_uchiwa_refresh }}"

# Custom Uchiwa part of the config
uchiwa_uchiwa__custom: {}

# Final Uchiwa part of the config
uchiwa_uchiwa: "{{
  uchiwa_uchiwa__default.update(uchiwa_uchiwa__custom) }}{{
  uchiwa_uchiwa__default }}"

# Default config
uchiwa_config__default:
  sensu: "{{ uchiwa_sensu }}"
  uchiwa: "{{ uchiwa_uchiwa }}"

# Custom config
uchiwa_config__custom: {}

# Final config
uchiwa_config: "{{
  uchiwa_config__default.update(uchiwa_config__custom) }}{{
  uchiwa_config__default }}"
```


Dependencies
------------

*- [`config_encoder_filters`](https://github.com/jtyr/ansible-config_encoder_filters)


License
-------

MIT


Author
------

Jiri Tyr
