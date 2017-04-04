### Chef

#### A recipe:
* Created using Ruby
* Is a collection of resources defined using patterns; helper code is added using Ruby
* Must define everything that is required to configure part of a system
* Must be stored in a cookbook
* May be included in a recipe (include_recipe)
* May use the results of a search query and read the contents of a data bag
* May tag a node to facilitate the creation of arbitrary groupings
* Must be added to a run-list before it can be used by the chef-client
* Is always executed in the same order as listed in a run-list
* If included multiple times in a run-list, will only be executed once
* By default, resources are executed in the other they are defined. This can be overridden.


### Directives
"Directives" can change the order in which resources are executed.

#### Notifies directive
>a notification property that allows a resource to notify another resource to take action when its state changes.

Example: Notice the service "httpd" resource type at the top of the file - it currently has no actual instructions on what to do, so it will do nothing. The next part, the cookbook file looks at the /etc/httpd/conf/httpd.conf file and if the file does not already have the configurations listed below, changes will  be made to the file. When the notifies property runs, it will send a restart notification to the service "httpd" resource name.

Example of the notifies directive:

```
service "httpd" do
end
```

```
cookbook_file "/etc/httpd/conf/httpd.conf" do
    owner 'root'
    group 'root'
    mode '0644'
    source 'httpd.conf'
    notifies :restart, "service[httpd]"
end
```

#### Subscribes directive
> a notification property that allows a resource to listen to another resource and then take action if the state of the resource being listened to changes.

In the example below, the subscribes listens for the change. By default, the service resource type is not set to do anything other than subscribe. The cookbook_file resource type looks at the /etc/httpd/conf/httpd.conf and will make changes if the configuration does not match. If, and only if, changes are made to the /etc/httpd/conf/httpd.conf file then the service resource type will execute a reload.

Example of the subscribes directive:

```
service "httpd" do
    subscribes: reload, "cookbook_file[/etc/httpd/conf/httpd.conf]"
end
```

```
cookbook_file "/etc/httpd/conf/httpd.conf" do
    owner 'root'
    group 'root'
    mode '0644'
    source 'httpd.conf'
end
```

### Run List
>A run-list is a list of cookbooks/recipes that are to be executed on a specific node.

Example:  
_run_list "recipe[chrony]", "recipe[packages]", "recipe[firewall_rules]"_

Chef-client will execute the example recipes in the other that they are presented - chrony, packages, and then fiirewall_rules. If a recipe is assigned to a node more than one time, the recipe will still only be executed one time on that node.

Sometimes a cookbook can have 50 recipes in it. If you want to include a specific recipe inside a cookbook:

```
run list recipe[cookbook::recipe]
run_list recipe[chrony::recipe]","recipe[packages::recipe]","recipe[firewall_rules::recipe]","recipe[chrony::recipe]"
```

### include_recipe
>A recipe can include a recipe from a different cookbook. For example, including the motd_recipe in an already existing recipe:
include_recipe 'cookbook_name::recipe_name'
