### Getting Started and Bootstrapping a Node
> The bootstrap process connects to the node you are bootstrapping, downloads and installs Chef and Ohai (Ohai is used to build the node attributes and objects) and performs the first chef-client run (the first convergence) which creates the client.pem key which is needed for the node to authenticate against the Chef server.

#### Configure the Node   

Enable root SSH access by editing /etc/ssh/sshd_config:  
`PermitRootLogin yes`  

Restart SSHD:  
`$ systemctl restart sshd`  

#### Configure the Workstation  

Install chef on the workstation:  
`$ curl -L https://www.opscode.com/chef/install.sh | sudo bash`

#### Bootstrap the Node From the Workstation  

Navigate to the chef repo and execute the knife bootstrap command:  
`$ knife bootstrap <hostname> -x root -P <password>`  
