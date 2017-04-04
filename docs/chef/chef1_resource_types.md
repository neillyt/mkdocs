#### A resource has 4 components:
- Resource Type  
- Resource Name  
- Resource Properties  
- Actions to be applied to the resource  

### Resource Types
#### The package resource

Actions:  
:install - install a package  
:nothing - This action does nothing until it is notified by another resource to perform an action  
;purge - Removes the configuration files as well as the package (only for Debian)  
:reconfig - Reconfigures a package  
:remove - Removes a package (also removes configuration files)  
:upgrade - Install a package or ensure an existing package is at the latest version  

This resource tells the node what to do if the httpd package is not yet installed. If the package does not exist, it will install the package. Note: the default action of the package resource type is always to install, hence if there is no action specifically listed, this recipe will install the package by default.  
```
package 'httpd' do               # Resource name = httpd
	action :install				 # Action = install
end
```

#### Installing Multiple Packages at a Time (With the Package Resource Type)
The example below shows how to easily install multiple packages at once. Let's break it down.
- the %w{httpd vim tree emacs} indicates a word array  
- httpd, vim, tree, emacs - these are the packages to be installed  
- .each do - this is equvalent to a for loop. For each package in the word araay, do the action specified  
- pkg - This is a variable to hold the for loop.  
- package - This is the package resource type.  
- action :upgrade - This is the action of the package resource type - says install or make sure package is at newest version.  

```
%w{httpd vim tree emacs}.each do |pkg|
	package pkg do
		action :upgrade
	end
end
```

The above code is equivalent to this:

```
['httpd','vim','emacs'].each do |pkg|
	package pkg do
		action :upgrade
	end
end

```

Also equivalent to this:  

```
package 'httpd' do
	action :upgrade
end

package 'vim' do
	action :upgrade
end

package 'emacs' do
	action :upgrade
end
```

#### Service resource type
The service resource type works with services.
Actions:  
- :disable  
- :enable  
- :nothing  
- :reload  
- :start  
- :restart

In the example below, the service name is not explicitly provided, so the service name will inherit the name of the resource name (httpd). This recipe will make set the httpd service to enabled and started.

```
service 'httpd' do                       # Resource name = apache
	action[:enable,:start]			     
end
```

In the below example, we use a resource name (apache) and then explicitly define a service name (httpd). So in this example, the service name being explicitly defined, httpd will (still) be the service to be enabled and started.

```
service 'apache' do						# Resource name = apache
	service_name 'httpd'				# Service name = httpd
	action[:enable, :start]
end
```

### File resource type
The file resource type is used to create and modify files.
Resource types have a special property - notifies. The notifies property allows a resource to "notify" another resource when the state changes and then allows the actions associated with that resource to perform.

In the example below, the resource type (file) has a resource name of /etc/httpd/vhost.conf. Since the File does not explicitly define a name, (just like the previous example), the name will inherit the resource name of /etc/httpd/vhost.conf.

```
service 'httpd' do							# Service resource name httpd
	action[:enable,:start]					# Actions to be performed
end

file '/etc/httpd/vhost.conf' do				# File resource name /etc/httpd/vhost.conf
	content 'fake vhost file'				# Contents of the file
	notifies :restart, 'service[httpd]'		# Notify property - sends a restart to the service resource that has a name httpd
end
```

### Not_If and Only_If
These are safeguards to prevent unwanted or unnecessary actions from running. It is a way to implement conditions that must be met in order for actions to be called.

#### not_if
In this example, not_if is used to tell the command to run only if the not_if condition is met.

```
execute 'not-if-example' do
	command '/usr/bin/echo "10.10.10.10 web webby.example.com" >> /etc/hosts'
	not_if 'test -z `grep "10.10.10.10 web webby.example.com" /etc/hosts`'
end

only_if
In this example, the only_if property is used to tell the command to run when the only_if condition is met.
execute 'not-if-example' do
	command '/usr/bin/echo "10.10.10.10 web webby.example.com" >> /etc/hosts'
	only_if 'test -z `grep "10.10.10.10 web webby.example.com" /etc/hosts`'
end
```
