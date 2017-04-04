### Knife
> Command line tool which provides an interface between the local chef-repo and chef-server.

Knife is used for:  
- Creating cookbooks  
- Uploading cookbooks to chef server  
- Managing roles and run_lists  
- Searching chef-server node object data  
- Bootstrapping nodes  
- Essentially everything we need to do as a DevOps administrator  

### Boostrapping with Knife

Bootstrapping connects the workstation to the node to perform the following:  
- Knife is installed on the host  
- Ohai is used to detect attributes on a node and report them to chef-client at the start of every client run it is required for chef-client to work and it builds the node object  
- Ruby  
- Chief-client  
- A few other additional items  

To bootstrap a node:  
`$ knife bootstrap <address> -x user -P password -N nodename`  

 Note that in a professional environment, the node name would be the fully qualified domain name.  
