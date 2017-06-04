### Roles
A role is a way to define certain patterns and processes that exist across nodes in an organization as belonging to a single job function.

Up until this point we have assigned recipes to be run for each node.  
- Instead of updating run_lists for a node, all we have to do is update a role on the server.  
- This prevents up from having to manually touch all nodes that need the change.  

A role is essentially a listing of recipes and attributes that are to be executed on a node. Instead of assigning a run list for each node, we assign the node a role. A base role can be assigned inside of a roles run_list.  

#### Role management with knife  

In the chef-repo/roles directory, create a role file called "base":  
`$ vim base.rb`  
```
name "base"
description "Base role for all SnapStack nodes"
run_list "recipe[system_users]" "recipe[sudo_group]"
```

Upload the role to the Chef server:  
`$ knife role from file base.rb`  

Add the "base" role to a node:  
`$ knife node run_list add compute1.cist.pitt.edu "role[base]"`  
