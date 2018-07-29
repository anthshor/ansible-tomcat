## Deploying tomcat with ansible

### Dependencies

 - Ansible host to run from
 - Hosts to deploy to.

### Example deployment

The following example shows two VMs web1, web2 to deploy to from the ansible host.

```
[ansible@ansible-vm tomcat]$ cat /etc/ansible/hosts
[web]
web1
web2
```

The IP address are defined in /etc/hosts 
```
192.168.33.15 web1
192.168.33.16 web2
```


Runing the playbook

```
[ansible@ansible-vm tomcat]$ ansible-playbook tomcat-play.yml

PLAY [web] **************************************************************************************************************************************

TASK [Gathering Facts] **************************************************************************************************************************
ok: [web2]
ok: [web1]

TASK [roles/tomcat : create software home] ******************************************************************************************************
changed: [web2]
changed: [web1]

TASK [roles/tomcat : Copy tomcat zip file] ******************************************************************************************************
changed: [web2]
changed: [web1]

TASK [roles/tomcat : Install unzip] *************************************************************************************************************
changed: [web2]
changed: [web1]

TASK [roles/tomcat : Unpack tomcat] *************************************************************************************************************
changed: [web2]
changed: [web1]

TASK [roles/tomcat : Install java if it is not installed] ***************************************************************************************
changed: [web2]
changed: [web1]

TASK [roles/tomcat : Create the webapp group] ***************************************************************************************************
changed: [web2]
changed: [web1]

TASK [roles/tomcat : Create the webapp user] ****************************************************************************************************
changed: [web2]
changed: [web1]

TASK [roles/tomcat : Create setenv.sh from template] ********************************************************************************************
changed: [web1]
changed: [web2]

TASK [roles/tomcat : Create tomcat.service from template] ***************************************************************************************
changed: [web1]
changed: [web2]

TASK [roles/tomcat : Modify ownership] **********************************************************************************************************
changed: [web2]
changed: [web1]

TASK [roles/tomcat : Modify permissions] ********************************************************************************************************
changed: [web1] => (item=startup.sh)
changed: [web2] => (item=startup.sh)
changed: [web1] => (item=catalina.sh)
changed: [web2] => (item=catalina.sh)
changed: [web1] => (item=shutdown.sh)
changed: [web2] => (item=shutdown.sh)

TASK [roles/tomcat : enable tomcat startup] *****************************************************************************************************
changed: [web2]
changed: [web1]

TASK [roles/tomcat : Add firewall rule http] ****************************************************************************************************
changed: [web1]
changed: [web2]

TASK [roles/tomcat : Add firewall rule for port 8080] *******************************************************************************************
changed: [web1]
changed: [web2]

TASK [roles/tomcat : Restart firewalld] *********************************************************************************************************
changed: [web1]
changed: [web2]

PLAY RECAP **************************************************************************************************************************************
web1                       : ok=16   changed=15   unreachable=0    failed=0
web2                       : ok=16   changed=15   unreachable=0    failed=0
```

Verify to show the Apache tomcat "If you're seeing this, you've successfully installed Tomcat. Congratulations!" page

 - http://192.168.33.15:8080/
 - http://192.168.33.16:8080/