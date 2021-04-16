Role Name
=========

Install and provision Freeswitch service for SIP audio and video calls via networks

Role Variables
--------------
```
freeswitch_language: ru
# You can modify for freeswitch (by defaul en (english))
```

```
freeswitch_default_ports:
  rtp_start_port: 64535
  # You can modify rtp start port for transmition media beetwen users or other SIP PBX (by defaul 64535)
  rtp_end_port: 65535
  # You can modify rtp end port for transmition media beetwen users or other SIP PBX (by defaul 65535)
```

```
sip_users: 
# You can create single users (by default created only one user with number/login - 7009)
  - name: XXX
    phone: 7009
    password: 1234
```

```
sip_user_groups:
# You can create user pool (by default created pool with number/login - from 1200 to 1299 )
  begin: 1200
  end: 1299
```

```
sip_conf_room:
  narrowband:
  # You can create conference with standart quility (Codec G.729, G.711, Software based, etc.) (by default created pool with number/login - from 3000 to 3005 )
    - name: nb_conferences
      begin: 3000
      end: 3005
  wideband:
  # You can create conference with wideband quility (Codec G.722, Speex, 16kHz, etc.) (by default created pool with number/login - from 3100 to 3105 ) 
    - name: wb_conferences
      begin: 3100
      end: 3105
  ultrawideband:
  # You can create conference with wideband quility (Codec G.722, Speex, 32 kHz, etc.) (by default created pool with number/login - from 3200 to 3205 ) 
    - name: uwb_conferences
      begin: 3200
      end: 3205
  mono_cd_quility:
  # You can create conference with wideband quility (Codec G.722, Speex, 48 kHz, mono sound, etc.) (by default created pool with number/login - from 3300 to 3305 ) 
    - name: cdquality_conferences
      begin: 3300
      end: 3305
  stereo_cd_quility:
  # You can create conference with wideband quility (Codec G.722, Speex, 48 kHz, stereo sound, etc.) (by default created pool with number/login - from 3500 to 3505 ) 
    - name: cdquality_stereo_conferences
      begin: 3500
      end: 3505
```

Example Playbook
----------------

```
  - hosts: all
    become: yes
    roles:
      - freeradius
```

License
-------

MIT

## Quick Start Guide

### Required dependencies (VirtualBox, Vagrant, Ansible)

  1. Download and install [VirtualBox](https://www.virtualbox.org/wiki/Downloads);
  2. Download and install [Vagrant](http://www.vagrantup.com/downloads.html);
  3. [Mac/Linux only] Install [Ansible](http://docs.ansible.com/intro_installation.html);
  4. Install ansible-galaxy community.docker [community.docker](https://docs.ansible.com/ansible/latest/collections/community/docker/docker_login_module.html).

### How to local run service
  1. Download this project and put it wherever you want;
  2. Open Terminal, cd to this directory;
  3. Check variables in directory `tests/vagrant.yml`;
  4. Add DOCKER_USER, DOCKER_PASS, DOCKER_MAIL in environment. It's env's from your account in dockehub;
  5. Type in `vagrant up`.

### How use role in dependencies
example of `meta/main.yml`

```yaml
galaxy_info:
  author: Sergey Kurbatov
  description: Ansible Role Infra
  license: MIT
  min_ansible_version: 2.4

  platforms:
  - name: Ubuntu
    versions:
    - all
dependencies:
  - src: 'git+ssh://git@github.com:skurbatov/freeswitch.git'
    vars:
      sip_users:
        - name: Hulio Eg
          phone: 7666
          password: 1234
      sip_user_groups:
        begin: 7000
        end: 7200
      sip_conf_room:
        wideband:
          - name: megaconf
            begin: 3600
            end: 3605
```

### Numbers for test and registration
```
  1200-1299,7009 - Extension for registration
  2000-2002 - Sample dial groups
  3000-3005 - Narrowband conferences (8kHz)
  3100-3105 - Wideband conferences (16kHz)
  3200-3205 - Ultra-wideband conferences (32kHz)
  3300-3305 - CD-quality conferences with mono sound (48kHz)
  3500-3505 - CD-quality conferences with stereo sound (48kHz)
  4000 - Voicemail retrieval
  5000 - Sample IVR
  5900 - Call park
  5901 - Call park retrieval
  9888 - FreeSWITCH conference
  9992 - Information application # TODO
  9996 - Echo test # TODO
  9999 - Music on hold # TODO
```

## About the Author

This project was created by [Sergey Kurbatov](https://skurbatov.github.io/).