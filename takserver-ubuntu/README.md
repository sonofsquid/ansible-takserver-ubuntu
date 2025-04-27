takserver-ubuntu
=========

Install takserver 5.3, single-server on an Ubuntu Machine.  
Not an expert so bear with me here  

Tested on: 
 - Ubuntu desktop: 24.04 Noble  

Based on the Tak_Server_Configuration_Guide.pdf available on tak.gov. Sets up tak server exactly as directed. 

Intended as a fresh install on a fresh ubuntu machine  

Notes:  
- Configure desired variables for cert metadata, CA/Server cert names, and Admin/User certs in the vars/main.yml file. 
- defaults are currently built into the individual playbooks/jinja templates.

Known Issues:  
- Adding admin privlages to admin users task sometimes fails. Retry and pause built-into task. Currently last task in "config-ufw".
- When spinning up new takserver, random postgresql db password is created and stored in xml file. Currently "config-coreconfig" reads that password from the backup xml file, then saves it as a variable to apply in the jinja template later.
- final task to configure UFW may interfere with previous settings (need to test)

Future Improvements:  
- Remove unnecassary takserver restarts/daemon-reloads (Currently based exactly off the Tak_Server_Configuration_Guide.pdf)  
- Add intermediary cert creation.
- Create version for RHEL/Rocky.  
- Move default values into defaults/main.yml.  


Requirements
------------

- Ubuntu LTS 24.04 Noble  

- SSH installed / server reachable from Ansible controller: 
  - On takserver machine:  
    `sudo apt install ssh -y`

- inventory file with ability to become super user.

- `takserver_5.3-RELEASE24_all.deb` file from TAK.gov 
  - Place this in the `files/` directory   

Role Variables
--------------

<!-- A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well. -->

All definable variables set in: `vars/main.yml`  
`user_cert_name` and `admin_cert_name` variabels are lists. Specify as many users/admins as required.

```yaml
script_path: /opt/tak/certs/cert-metadata.sh

country_name: US
state_name: VA
city_name: XX
org_name: XX
org_unit_name: XX

# cert vars
certs_script_path: /opt/tak/certs/
makeRoot_script_path: /opt/tak/certs/makeRootCa.sh

ca_cert_name: 'root-{{ ansible_hostname }}'
server_cert_name: takserver
user_cert_name: ['user01', 'user02', 'user03']
admin_cert_name: ['admin']
```

Dependencies
------------

<!-- A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles. -->

No third party modules currently used.  

Example Playbook
----------------

<!-- Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 } -->

Call the Role and all tasks  

```yaml
- name: Setup Takserver on Ubunutu
  hosts: all
  become: true

  roles:
    - takserver-ubuntu
```

Call a specific task from the role. Ex prereqs only  

```yaml
- name: Run only the cert-creation portion of takserver3
  hosts: all
  become: true

  roles:
    - role: takserver-ubuntu
      tasks_from: prereq.yml
```


License
-------

Place_holder

Author Information
------------------

<!-- An optional section for the role authors to include contact information, or a website (HTML is not allowed). -->

73 de Q