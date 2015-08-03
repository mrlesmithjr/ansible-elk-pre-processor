Role Name
=========

Installs ELK Stack Role (ELK-Pre-Processor)

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------

````
clear_logstash_config: false
config_logstash: false
logstash_config_dir: /etc/logstash/conf.d
logstash_log_dir: /var/log/logstash
reset_logstash_config: false
logstash_configs:
  - 000_inputs
  - 001_filters
  - 200_filters_syslog
  - 210_filters_pfsense
  - 220_filters_esxi
  - 230_filters_vcenter
  - 240_filters_quagga
#  - 250_filters_vmware_nsx
  - 999_outputs
logstash_file_inputs:
  - path: /var/log/nginx/access.log
    type: nginx-access
  - path: /var/log/nginx/error.log
    type: nginx-error
  - path: /var/log/mail.log
    type: postfix-log
  - path: /var/log/redis/redis-server.log
    type: redis-server
logstash_grok_patterns:
  - IPTABLES
  - NGINXERROR
logstash_inputs:
  - prot: tcp
    port: 10514
    type: syslog
  - prot: tcp
    port: 1514
    type: VMware
  - prot: tcp
    port: 1515
    type: vCenter
  - prot: tcp
    port: 1517
    type: Netscaler
  - prot: tcp
    port: 28778
    type: elasticsearch-curator
  - prot: tcp
    format: json
    port: 3515
    type: eventlog
  - prot: tcp
    codec: json_lines
    port: 3525
    type: iis
  - prot: tcp
    codec: json
    port: '{{ rundeck_logstash_port }}'
    type: rundeck
logstash_outputs:
  - output: redis
    output_host: '{{ logstash_server_fqdn }}'
````

Dependencies
------------

````
mrlesmithjr.ntp
mrlesmithjr.rsyslog
mrlesmithjr.snmpd
mrlesmithjr.timezone
mrlesmithjr.logstash
mrlesmithjr.dnsmasq
````
Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: mrlesmithjr.elk-pre-processor }

License
-------

BSD

Author Information
------------------

Larry Smith Jr.
- @mrlesmithjr
- http://everythingshouldbevirtual.com
- mrlesmithjr [at] gmail.com
