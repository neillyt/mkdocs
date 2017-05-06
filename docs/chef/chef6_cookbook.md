### Building an Apache Cookbook
> A cookbook has a group of recipes - recipes are the desired configurations that are applied to our node.  

The cookbook will be created in your chef-repo and will contain a directory called recipes. First we need to create a cookbook using knife. This will create a directory that has all of the static, default cookbook files. There will be a default recipe called **default.rb**:  
`$ knife cookbook create <name of cookbook>`  
`$ knife cookbook create apache`  

Edit the **default.rb** recipe to make it download Apache:  

```
package "httpd" do
  action :install
end

service "httpd" do
  [action :enable, :start]
end
```

Using knife, upload the cookbook:  
`$ knife cookbook upload apache`  

If chef-client was run on a node, it would not yet execute the apache cookbook because it is not part of our policy (aka run_list) yet. Using knife, add the cookbook to our policy. Note below that we are not naming a recipe, we are naming a cookbook - this is because we only modified the **default** recipe of the cookbook:  
`$ knife node run_list add <node name> "recipe[apache]"`

To insert a recipe into a run list and to have it execute before another recipe, use the "-b" flag. In the following example, we are inserting a new recipe, security, before the apache recipe.  
`$ knife node run_list add <node name> -b "recipe[apache] recipe[security]"`

### Chef Attributes, Recipe, and Template

#### Defining attributes

By default, every cookbook has an attributes folder. The **default.rb** in the attributes folder is the default attributes for a cookbook. Using attributes, we can define different settings for different apache sites.  
`$ vim default.rb`

```
default["apache"]["sites"]["tomneilly"] = { "port" => 80, "domain" => "tomneilly.com" }  
default["apache"]["sites"]["surface"] = { "port" => 80, "domain" => "surface.tomneilly.com" }
```

#### Defining a template
A template is a file that contains static and dynamic content that will be reused on different hosts. The static content stays the same on each host while the dynamic content is created using the information defined in the cookbook's attributes.  

Navigate to the cookbooks templates directory and create a new template file called "vhost.erb". We must use the Ruby evaluator **<%= @variable_name %>** to pull variables from our template and our recipe.
`$ vim templates/vhost.erb`  

To reference variables from our template, we must use <%= @our_variable %>
```
#vhost template file

<VirtualHost *:<%= @port %> >
    ServerName <%= @domain %> >
    <Directory <%= @document_root %> >
    </Directory>
</VirtualHost>
```

Now that we have defined some attributes, let's update our default.rb recipe to loop through the attributes and make the appropriate changes. Let's incorporate a ruby loop that will apply changes for each attribute (where sitename would be tomneilly or surface, and data would be the port and domain). Use the *directory* resource to make sure to create the folders if they do not already exist. Once the recipe is complete, upload the cookbook using knife.
```
package "httpd" do
  action :install
end

node["apache"]["sites"].each do |sitename, data|
  document_root = "/content/sites/#{sitename}/"

  directory document_root do
    mode "0755"
    recursive true
  end

  template "/etc/httpd/conf.d/#{sitename}.conf" do
    source "vhost.erb"
    mode "0644"
    variables(
        :document_root => document_root,
        :port => data["port"],
        :domain => data["domain"]
      )
    notifies :restart, "service[httpd]"
  end

end

service "httpd" do
  [action :enable, :start]
end
```

Upload the cook:  
`$ knife cookbook upload apache`  

Run a convergence:  
`$ chef-client`
