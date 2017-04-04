### Chef Terminology

Recipes:  
> Fundamental configuration element within an organization.

Cookbook:  
> Defines a scenario and is the fundamental unit of configuration and policy distribution. Written in Ruby. Recipes are stored in cookbooks and can have dependencies of other recipes.

Chef-Client:  
> Agent that runs locally on the node that is registered with the chef server.

Convergence:  
> Occurs when chef-client configures the system/node based off information collected from chef-server.

Configuration Drift:  
> Occurs when the node state does not reflect the updated state of policies/configurations on the chef server.

Resources:  
>A statement of configuration policy within a recipe. Describes the desired state of an element in the infrastructure and steps needed to configure.

Provider:  
> Defines the steps that are needed to bring the peice of the system from its current state to the desired state.  

Attributes:  
> Specific details about the node, used by chef-client to understand current state of the node, the state of the node on the previous chef-client run, and the state of the node at the end of the client run.  

Data-bags:  
> A global variable stored as JSON data and is accessible from the Chef server.  

Workstation:  
> A computer configured with Knife and used to synchronize with chef-repo and interact with chef server.

Knife:  
> Command line tool which provides an interface between the local chef-repo and chef-server.  

Knife is used for:  
- Creating cookbooks  
- Uploading cookbooks to chef server  
- Managing roles and run_lists  
- Searching chef-server node object data  
- Bootstrapping nodes  
- Essentially everything we need to do as a DevOps administrator  

client.rb:  
> Configuration file for the chef-client located at /etc/chef/client.rb on each node.  

Ohai:  
> Tool used to detect attributes (kernel, NIC, memory, CPU, etc.) on a node and then provide attributes to chef-client at the state of every chef-client run.  

Node Object:  
> Consists of run-list and node attributes that describe states of the node.

Chef-Repo:  
> Located on the workstation and installed with the starter kit, should be synchronized with a version control system and stores Cookbooks, roles, data bags, environments, and configuration files.  

Organization:  
> Used in chef enterprise server to restrict access to objects, nodes environments, roles data-bags, etc.

Environments:  
> Used to organize environments (Prod/Staging/Dev/QA) generally used with cookbook versions.  

Idempotence:  
> Means a recipe can run multiple times on the same system and the results will always be identical.
