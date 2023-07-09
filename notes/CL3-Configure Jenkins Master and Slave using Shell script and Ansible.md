## Setup Jenkins Environment

As we are setting up jenkins on docker we should install docker also we are exposing jenkins to external using nginx port number 4000

1. run jenkins-post-install.sh file on master to setup docker, and docker compose
2. to provision jenkins and nginx as a container we should have docker-compose and nginx.conf files.
3. run docker-compose up  
  ```sh
   docker-compose up -d
   ```
4. expose port 4000 in the sg if you haven't done in the terraform code

### Setup Jenkins slave node
To setup we need java, docker, maven, and GitHub on salve system
1. clone slave-setup.sh from rtp3-ars
2. run it to setup salve system

### Setup jenkins master and slave
1. add credentials
2. add slave node

Note: remove this env if you wish to set up DevOps environment using ansible
### Setup jenkins environment using ansible

1. setup ansible, Jenkins master, and slave using terraform
2. install ansible server
``` sudo amazon-linux-extras install ansible2 ```
3. create hosts file in /opt
4. create new or edit ansible.cfg file in the /opt
```vi /etc/ansible/ansible.cfg
   host_key_checking = False //Make it false to avoid checking ssh first time connect message
```
5. copy ssh private keys on /opt onto ansible
6. test the connection using
   ```
     ansible -i hosts all -m ping
  ```
7. clone ansible scripts from rtp3-ars/ansible
8. run jenkins post install.yml on jenkins master node
``` ansible-playbook -i hosts <.yml file> --check //Dry run
    ansible-playbook -i hosts <.yml file>
```
9. run slave-setup.yml on jenkins salve

### Setup jenkins master and slave
#### Run Freestyle Project on Slave node
1.Click on create freestyle project.Check the box restrict where this project can be run and add a slave label
 as shown in image below
 
 
 ![i1](https://user-images.githubusercontent.com/100523955/212520610-1f6a2ba3-c217-47af-9f05-71c5a8b40d09.png)
 
 
 
2.Add the following command in build steps
 ```sh
echo" This is a test run ">>/home/ec2-user/test.txt
```
3.After saving it the freestyle job is executed in slave
#### Verification of test.txt in slave node
1. Login to slave node
2. Go to -->/home/ec2-user
3. Verify if the test.txt file is available.The master and salve connectivity is success

