### Data Bags

A data bag is a global variable that is stored in JSON data and accessible from the chef server. A data bag is indexed for searching and can be loaded by a recipe or accessed during search. Each JSON file is a data bag item.

Types of data stored in data bags:  
- Users to be added to a system  
- Admins to be added to sudo  
- API/DB credentials (more secure and better than environment attributes for credentials)  
- Much more  

Create a data bag for users and groups:  
`$ knife data bag create users`  
`$ knife data bag create groups`  

List all of your data bags:  
`$ knife data bag list`

Create a data bag item (thn16.json) for creating a user. Note that each data bag item MUST have a unique "id" field inside of it:    
```
$ vim thn16.json
{
  "id:"thn16",
  "comment": "Tom Neilly",
  "home": "/home/thn16",
  "shell": "/bin/bash"
}
```

Add the 'thn16' data item to the _users_ data bag:  
`$ knife data bag from file users thn16.json`

```
$ knife data bag from file users thn16.json
Updated data_bag_item[users::thn16]
```

Show all of the data items in the "users" data bag:  
`$ knife data bag show users`

Show the specific properties of the data item:  
`$ knife data bag show users thn16`  

```
$ knife data bag show users thn16
WARNING: Unencrypted data bag detected, ignoring any provided secret options.
comment: Tom Neilly
gid:     0
home:    /home/thn16
id:      thn16
shell:   /bin/bash
```

### Build a User Cookbook to Use a Data Bag  

Create a "system_users" cookbook:  
`$ knife cookbook create "system_users"`  

Navigate to _/cookbooks/system_users/recipes_ and edit the default.rb. The "search" command says "search all data items in the _users_". The "*:*" are wildcards, saying "return all attributes of each data item". The "each" loop says, "for each data object returned, run the _user_ resource with these attributes".
```
search(:users, "*:*").each do |data|
 user data["id"] do
   comment data["comment"]
   home data["home"]
   shell data["shell"]
   end
end
```

Upload the cookbook:  
`$ knife cookbook upload system_users`  

Add the recipe to a node's run_list:  
`$ knife node run_list add compute1.cist.pitt.edu system_users`  

Log into the node and run chef-client to deploy the recipe:  
`$ ssh compute1.cist.pitt.edu`  
`$ sudo chef-client`  


### Build a Groyp Cookbook to Use a Data Bag  

Create a "wheel_group" cookbook. In this cookbook, data["members"] refers to an array that we defined in our data item. This will apply the group resource to all users in the members array.  
`$ knife cookbook create sudo_group`  

```
search(:groups, "*:*").each do |data|
  group data["id"] do
    action :modify
    members data["members"]
    append true
  end
end
```

Upload the cookbook and add it to the node's run_list:  
`$ knife cookbook upload sudo_group`
`$ knife node run_list add compute1.cist.pitt.edu sudo_group`
