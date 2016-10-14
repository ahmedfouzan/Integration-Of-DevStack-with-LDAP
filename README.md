# Integration-Of-DevStack-with-LDAP
Integrating LDAP as identity and authorization backend for DevStack by replacing MySQL

To know more about DevStack https://github.com/openstack-dev/devstack

Steps to integrate DevStack with LDAP :

1) Cloning DevStack Liberty

    git clone https://github.com/openstack-dev/devstack.git -b stable/liberty
  
  Before running the above command install 'git' if not installed
  
    sudo apt-get install git

2) Go into DevStack

    cd devstack

3) Create a file ‘localrc’ in the 'devstack' directory and add the below content to it

    ##[[local|localrc]]
    ADMIN_PASSWORD=password
    MYSQL_PASSWORD=password
    RABBIT_PASSWORD=password
    SERVICE_PASSWORD=password
    SERVICE_TOKEN=password
    ENABLED_SERVICES=key,n-api,n-crt,n-obj,n-cpu,n-net,n-cond,cinder,c-sch,c-api,c-vol,n-sch,n-novnc,n-xvnc,n-cauth,horizon,mysql,rabbit,ldap
    KEYSTONE_IDENTITY_BACKEND=ldap
    KEYSTONE_CLEAR_LDAP=yes
    LDAP_PORT=389
    LOGFILE=/opt/stack/logs/stackvg.sh.log
    VERBOSE=True
    LOG_COLOR=True
    SCREEN_LOGDIR=/opt/stack/logs
    HOST_IP=Your IP
    LDAP_PASSWORD=password
    
  OR
  
  Download the 'localrc' file from this repository and paste it in the 'devstack' directory

4) Execute the following command

    ./stack.sh

  This will take around 15 - 20 minutes, largely depending on the speed of your internet connection. Many git packages will be installed during this process.
  
  At the end of this you should see something like this
  
    Horizon is now available at http://10.0.2.15/ 
    Keystone is serving at http://10.0.2.15:5000/v2.0/ 
    Examples on using novaclient command line is in exercise.sh 
    The default users are: admin and demo 
    The password: password 
    ldap password : password 
    This is your host ip: 10.0.2.15 
    stack.sh completed in 269 seconds. 

5) Install PHPldapadmin

    sudo apt-get install phpldapadmin

6) Configure PHPldapadmin

    sudo gedit /etc/phpldapadmin/config.php
    
7) Search for the following sections and modify them accordingly

  Replace dc=test,dc=com with dc=openstack,dc=org

    $servers->setValue('server','base',array('dc=test,dc=com'));

  Replace cn=admin,dc=test,dc=com with cn=Manager,dc=openstack, dc=org

    $servers->setValue('login','bind_id','cn=admin,dc=test,dc=com');

  Make the following true and uncomment

    $config->custom->appearance['hide_template_warning'] = true;

8) Modify the file by executing the following command

    sudo gedit /usr/share/phpldapadmin/lib/TemplateRender.php

  Replace password_hash with password_hash_custom

    $default = $this->getServer()->getValue(‘appearance’,’password_hash’);
    
    
 You have successfully integrated LDAP with DevStack.
 
 You may go ahead and log in to LDAP and create user. With the same user id you may log in to horizon.
 
 Whatever users you create in LDAP appears in Horizon and viceversa.
