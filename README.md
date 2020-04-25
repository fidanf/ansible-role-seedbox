Ansible Role: Seedbox
=========

[![Build Status](https://travis-ci.com/fidanf/ansible-role-seedbox.svg?branch=master)](https://travis-ci.com/fidanf/ansible-role-seedbox)

Installs [rtorrent](https://github.com/rakshasa/rtorrent) >= 9.0.7 along with [Flood](https://github.com/Flood-UI/flood) web client on Debian/Ubuntu.
By default Flood will run on port 80 served by Nginx configured as a reverse-proxy.

Tested with :
- Debian 10.x :heavy_check_mark:
- Ubuntu 20.04.x :heavy_check_mark:
- Ubuntu 18.04.x :heavy_check_mark:
 
Requirements
------------

Ansible >=2.8

Role Variables
--------------

```yaml
# rtorrent
rtorrent_user_password: $6$.uRzBMvRZ/Xz$cQCZlmI1JdYrH/uAGsyywypam2wWBAVNB6TqCi8wnFJpiG2UmBZ0FyDbmEdLsziQUaAsAyH8dn3Yxwue1LCL// # 'rtorrent'
rtorrent_port_range: 50000-50000
rtorrent_port_random: no
rtorrent_dht_mode: disable
rtorrent_protocol_pex: no
rtorrent_trackers_use_udp: no

rtorrent_throttle_max_uploads: 100
rtorrent_throttle_max_uploads_global: 250
rtorrent_throttle_min_peers_normal: 20
rtorrent_throttle_max_peers_normal: 60
rtorrent_throttle_min_peers_seed: 30
rtorrent_throttle_max_peers_seed: 80
rtorrent_trackers_numwant: 80
rtorrent_protocol_encryption_set: allow_incoming,try_outgoing,enable_retry

rtorrent_network_max_open_files: 4096
rtorrent_network_max_open_sockets: 1536
rtorrent_network_http_max_open: 48
rtorrent_network_send_buffer_size: 4M
rtorrent_network_receive_buffer.size: 4M

rtorrent_pieces_memory_max_set: 1800M
rtorrent_network_xmlrpc_size_limit_set: 4M

rtorrent_network_scgi_open_port: 5001


# flood
flood_repo: git@github.com:Flood-UI/flood.git
flood_revision: master
flood_directory: /var/www/flood
flood_config_secret: s3cr3t
flood_config_server_port: 3000

# nginx
nginx_server_name: ~.* # hostname or IP to listen for. Default is anything.

```

Dependencies
------------

None but you should rely on another role to install nodejs >=8.x if not already present.

Example Playbook
----------------

```yaml
---
- hosts: seedbox-debian
  become: yes
  gather_facts: yes

  roles:
    - name: geerlingguy.nodejs
      vars: 
        nodejs_version: 10.x
        npm_config_unsafe_perm: true
        nodejs_npm_global_packages:
          - node-gyp

    - name: fidanf.seedbox

```

Example requirements.yml
------------------------

```yaml
- src: https://github.com/fidanf/ansible-role-seedbox
  name: fidanf.seedbox
  version: master

``` 

License
-------

MIT / BSD
