Role Name
=========

Package that configures debian openvpn service to work with
https://www.ovpn.com/en service.

How to configure it: 
 
1. Download your configuration package. 
2. Set ``OPENVPN_KEY`` to contents your keys from configration package
3. Set ``OPENVPN_CRT`` to contents of certficate from configuration package
4. Set ``OPENVPN_USERNAME`` and ``OPENVPN_PASSWORD``to your username and passwords
5. Configure configuration files to use: 

    1. For each of the servers you wish to connect add entry in ``OPENVPN_CONFS``
    2. It should look like in the example: 
    
           OPENVPN_CONFS:          
              - name: us-losangeles
                docker_helper: "{{ OPENVPN_DEFAULT_ENABLE_DOCKER_HELP }}"
                verbosity: "{{ OPENVPN_DEFAULT_VERBOSITY }}"
                proto: "{{ OPENVPN_DEFAULT_PROTO }}"
                server_list: |
                  remote something
    
       * `name` is config file name (should be unique)
       * `server_list` is actual list of server you wish to connect to
       * Rest of the options are explained below
       
# Features
       
## Default server
You can set default openvpn connection that you will automatically
connect to. To enable it set: ``OPENVPN_DEFAULT_CONFIG`` to 
``name`` of one of the config files. 

## Docker-friendly openvpn
       
Due to some incompatibility (see: https://stackoverflow.com/q/45692255/7918), 
openvpn may break docker. 

If you set ``OPENVPN_DEFAULT_ENABLE_DOCKER_HELP`` 
(or set ``docker_helper`` to true in one of the configuration files) this role will
fix this problem. 

## Other configuration options

* ``OPENVPN_DEFAULT_VERBOSITY`` sets verbosity of openvpn
* ``OPENVPN_DEFAULT_PROTO`` sets protocol (``udp`` or ``tcp``). 

## Things that need more work

Force ``DNS`` to not leak IP. For now manually set ``resolv.conf`` or 
``NetworkManager``. 

See this for possible solution: https://serverfault.com/a/590722/3441. 


# How to use other provider

Create config file template named `<provider>_openvpn_template`
and then set value: `OPENVPN_CONFIGURATION_TEMPLATE` to `<provider>_openvpn_template`. 

Other changes might ne needed. 


License
-------

BSD

Author Information
------------------

Jacek Bzdak <jacek@askesis.pl>
