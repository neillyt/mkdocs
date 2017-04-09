### Chef Configuration Concepts

> Policy determines what state a node should be in, resource executes steps to bring the node into that state by mapping to a provider (such as yum or apt-get)

#### Policy
- A collection of system configurations that you define (roles/databags/environments)  
- The policy states the state that each resource should  be in but not how to get there  
- chef-client will pull the policy and configure the node so that it matches the state of the policy  

Policy concept examples:  
- If it should be installed  
- If it is not installed then install it  
- If it is already installed then do nothing  
- A file should exist - if not, create it  
- If a file exists but does not hvae correct content  

#### Resources
> Resources define the desired state for a single confugration item present on a node that is under management by Chef

- Performs the configuration on a node and maps to providers  
- Recipes are stored in cookbooks  
- Building blocks of Chef configuration  
- When chef-client is run on a node, the resource is executed by the provider which is handled by Chef and the OS itself  
- Information as to what provider to use (ie what package manager to use) is populated when Ohai is run at the start of each chef-client run    

Most common resources:  
**Package**: Used to manage packages such as installing packages  
**Template**: Used to manage the contents of a Ruby template in the cookbook  
**Service**: Manage system services - what run levels to start the service in, current state of the service (running/stopped/etc)  
