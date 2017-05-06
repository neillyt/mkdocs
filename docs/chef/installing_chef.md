### Installing Chef Server and Workstation

- Go to https://www.chef.io/chef/get-chef/
- Download Chef DK and Chef Server

### Chef DK
Install Chef DK on your workstation. ChefDK gives you the **knife** and **chef** command line interfaces. This

### Setting Up Chef Server  

Set your hostname:  
`$ hostnamectl set-hostname <hostname>`  
`$ reboot now`  

Install Chef:  
`$ yum localinstall <chef-core-server.rpm>`  

Configure Chef:  
`$ chef-server-ctl reconfigure`  
`$ chef-server-ctl install chef-manage`  
`$ chef-manage-ctl reconfigure`  

Open firewall:  
`$ firewall-cmd --add-port=80/tcp --permanent`  
`$ firewall-cmd --add-port=443/tcp --permanent`  
`$ firewall-cmd --reload`  

Turn on and test Chef server:  
`$ chef-server-ctl test`

Bootstrap your first node:  
`$ knife bootstrap <node name> -x root -P <password> -N <node name>`

If you are using HTTPS and do not have a signed certificate, use knife to fetch and trust your self-signed certificate:  
`$ knife ssl check`
`$ knife ssl fetch`

Make sure your "knife.rb" chef_server_url is the same as the URL you are using to access Chef via the browser:  
Example:  
```
current_dir = File.dirname(__FILE__)
log_level                :info
log_location             STDOUT
node_name                "thn16"
client_key               "#{current_dir}/thn16.pem"
chef_server_url          "https://chef/organizations/upb"
cookbook_path            ["#{current_dir}/../cookbooks"]
```

Add a cookbook to your node's run_list. A run-list is a list of cookbooks/recipes that are to be executed on a specific node.  
`$ knife node run_list add <node name> "recipe[apache]"`  
