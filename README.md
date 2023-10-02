Cribl
=========

This role will be able to install Cribl and configure the node(s) to be either a worker node or a leader node. The role will also be able configure a pair of Linux Virtual Machines to be a Cribl Leader HA pair.

Requirements
------------

Virtual machine must be configured with the correct OS version and base system requirements specified in the Cribl sizing documents. 

In order to configure a HA Leader configuration. The two nodes will need to have the NFS mount already configured and mounted at the specified mount point. Not having this volume mounted will cause the playbook to fail for that node(s).

Specifiying no varables for the role will only install Cribl and set the ownership of the files to the 'cribl' user. The node will not be configured as a leader or worker node and no systemd service will be configured.


Role Variables
--------------

| Variable | Requried | Description | Default |
| cribl_path | no | Location in which to untar the 'cribl' directory |  /opt |
| cribl_user | no | Linux User and Group to run Cribl | cribl |
| cribl_leaderhost | worker - yes | DNS/IP of the Leader node that the workers will join | leader.cribl.io |
| cribl_leaderport | no | Port for Leader/Worker communications | 4200 |
| cribl_leaderauth | no | Auth Token for Workers to join Leader. Default value is the out of the box value | criblmaster |
| cribl_workergroup | no | Worker Group to join the worker nodes to | default |
| cribl_worker | no | Set to a true value in order to configure the node(s) as Cribl Worker node(s) | false | 
| cribl_leader | no | Set to a true value in order to configure the node(s) as a Cribl Leader node(s) | false |
| cribl_failovervol | no | Set to the path of the failover volume. This value is only required if setting up a Leader Node HA Pair | none |

Dependencies
------------

None


Example Playbook
----------------

    - hosts: workers 
      user: myuser
      become: yes
      tasks:
        - import_role:
            name: cribl
          vars:
            cribl_workergroup: mygroup
            cribl_worker: yes
            cirbl_leaderhost: leader.mydomain.com

    - hosts: leader 
      user: myuser
      become: yes
      tasks:
        - import_role:
            name: cribl
          vars:
            cribl_leader: yes

    - hosts: install_only 
      user: myuser
      become: yes
      tasks:
        - import_role:
            name: cribl

License
-------

BSD

Author Information
------------------

Ryan Hennessy currently works at Cribl as a Staff Solutions Engineer. 
rhennessy@cribl.io

