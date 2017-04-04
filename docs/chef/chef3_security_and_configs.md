### Chef Configuration Files, Chef Repository, and Chef Client
> There are 3 main files needed for a Chef workstation - the knife.rb configuration, the client.pem key file, and the validator.pem key file.

### Bootstrap Process
The bootstrap process performs the following:  
- downloads and installs Chef  
- downloads and installs Knife  
- downloads and installs Ruby  
- copies the organization's validator.pem file to the host (/etc/chef)    
- copies the client.pem file to the host  (/etc/chef)  
- copies the client.rb file to the host (/etc/chef)  

`$ knife bootstrap <servername or ip> -x <username> -P <password> -N <FQDN by default or alternative node name>`

### knife.rb

One of the most important configuration files is our _knife.rb_ file. The _knife.rb_ file keeps track of our the following:  
- current_dir  
- log_level  
- log_location  
- node_name  
- client_key  
- validation_client_name  
- validation_key  
- chef_server_url  
- cache_type  
- cache_options  
- cookbook_path  

### client.pem  

The client.pem is what is used for our node to connect to the Chef server. The client.pem is a copy of the validation.pem key. The validation.pem key should be deleted as leaving it is a security risk. If the client.pem file does not exist, the convergence of any recipe will fail.

### The chef-repo  
> Every organization has one chef-repo. The chef-server is in charge of the chef-repo. Workstations can have local repos which can be used to contribute to the chef-server's chef-repo.

chef-repo:  
- has all of the cookbooks for the organization  
- is maintained on the chef-server  
- every workstation has a copy of the repo on it  
- is maintained by Git or some other version control system  

### The chef-client
> The chef-client is an agent that runs on every single Chef node. When the chef-client runs, it performs many steps to bring the node into the desired state. When the chef-client runs, it does a "convergence" - it syncs up with the chef-server. During the sync it builds up the node to the state it needs to be in (using **Ohai***) and captures all the changes made to the host. All of the cookbooks are downloaded to the node. The chef-client should run automatically at appropriate intervals (30 or 60 mins). This ensures that the nodes will always be up-to-date with the latest cookbooks and configurations.

chef-client run:  
- authenticate against the chef-server using the client.pem  
- builds the node object and runs ohai  
- synchronizes with the chef-server (sends node object information and receives cookbooks/policies)  
- executes/compiles the desired policies  
- runs the node object   
- completes

#### client.rb  

client.rb is our chef-client configuration file that should be on every node. Common configuration contents of the client.rb file include:  
- log_level  
- log_location  
- chef_server_url  
- validation_client_name  
- node_name  

### ohai
> Ohai is the program that collects all of the metrics and information from the node. This is also a command-line tool. Ohai builds the "node object" and sends that information to the chef-server. The node object is built before and after chef-client is run in order record the state of the node before and after.  

### Chef Servers and Nodes
> A node can be anything that is able to run Chef: a phone, a switch, APIs, Unix, FreeBSD, Windows server, etc.

#### Chef Server
> A Chef server stores all policy configuration information for nodes. The node uses the client.pem file (created by validator.pem) during the chef-client run in order to authenticate against the chef-server.
