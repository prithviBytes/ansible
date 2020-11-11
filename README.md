# Ansible Task

## Configuring Master and Slave


Pre-requisite installation

### 1)Updating Ubuntu and Installing Ansible  
	
	#sudo apt update
	#sudo apt install ansible

### 2)Creating an admin for ansible (Master and Slave)

	#useradd admin
	#passwd xxxx

### 3)Giving sudo access to the Ansible Admin (Master and Slave)

	#visudo

### 4)Setting PasswordAuthentication Yes 

	#vi /etc/ssh/sshd_config

### 5) Restarting the service sshd for the changes to take effect
```	
    #service sshd reload 
```
### 6)Generating shh-key in the master 

	#ssh-keygen

### 7) Copying the generated public key into the slave
```
	#ssh-copy-id <Slave_IP_Address>(Slave's private ip address)
```
### 8) Adding slave to master 
```	
	#vi /etc/ansible/hosts
```
### 9)Testing the configuration with the command
	
	#ansible all -m ping

-------------------------------------------------------------------------------------------------------------------------

##					*** Launching Webserver On Slaves ***

### 1) Writing a playbook 

Name of playbook - apache.yml

```
- hosts: <Slave>
  tasks:
	
	- name: Installing Apache Server
	  become: yes
	  apt: name=apache2 
	       update_cache=yes
	       state=latest

	- name: Starting Server
	  become: yes
	  service:
		name: apache2
		state: started
		enabled: yes
```
 
### 2) Executing the playbook
```
	#ansible-playbook apache.yml 	
```

### 3) Testing if it works on browser

	http://<slave_IP_Address>

	*if this returns the default apache webpage then the configuration was successfull.


--------------------------------------------------------------------------------------------------------------------------

##						***Creating a Vault***


### 1) Creating a safe encrypted file
```
	#ansible-vault create vault.yml
```
	Enter the password for the vault
	
	--the file created will be encrypted 


### 2) Editing the file 
```	
	#ansible-vault edit vault.yml
```
	--Here we can save some sensitive variable which can be used in the ansible playbook.
	  Mentioning the variable in the ansible playbook -- {{ variable_name  }}.
	  Declaring the variable file in the ansible playbook -- varfiles:
									-vault.yml

### 3) Executing the playbook
```
	#ansible-playbook -i hosts playbook_name.yml --ask-vault-pass	
```
