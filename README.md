# My Ansible Config / Tutorial



## Steps for ansible setup to to server
## --------------------------------------
1 - make sure OpenSSH is installed on the worksation and servers

2 - connect ro each server from the worksation , 
    answer "yes" to inital connection prompt

        ssh root@<ip of server>

3 - create an SSH key pair (with a passphrase) for your normal user account
     
        me:$ ssh-keygen -t ed25519 -C "Comment about the key"
    

4 - copy that key to each server
        
        me:$ ssh-copy-id -i ~/.ssh/id_ed25519.pub root@<ip of server>


5 - check at server that your key is added
        
        server:$ cat .ssh/authorized_keys 
        

6 - create an ssh key  (with not a passphrase) that is speceific to ansible 
        
        me:$ ssh-keygen -t ed25519 -C "Comment for ansible the key"


7 - be careful to give the key diffrent name to be saved and 
    not overwride another key ~/.ssh/ansible
        

8 - copy that key to each server
    
        me:$ ssh-copy-id -i ~/.ssh/ansible.pub root@<ip of server>


9 - connect to server with ansible key
        
        me:$ ssh -i ~/.ssh/ansible  root@<ip of server>






==================================================================
Side Note : to cache the passphrase =>  me:$ eval $(ssh-agent)
==================================================================


## ad-hoc Commands

-m : module
-a : argument

#### access ansible to all ssh file  

##### ping inventory of web servers

    ansible all --key-file ~/.ssh/ansible -i inventory -m ping

#### after set the ansible.cfg file like that 
[defaults]
inventory = inventory
private_key_file = ~/.ssh/ansible

now you just need to run 
  
    ansible all -m ping


#### to get list of all hosts
    
    ansible all --list-hosts


#### to gather facts about server
    
    ansible all -m gather_facts
   
### make change example ( ubuntu / debian   update cache )

    ansible all -m apt -a update_cache 
    
### if you are not root user  ( ask for sudo password )

    ansible all -m apt -a update_cache --become --ask-become-pass

       

### install a package in all our modules by apt  ( vim-nox package ) 

    ansible all -m apt -a name=vim-nox 
    
