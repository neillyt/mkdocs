### Node Objects

Node object is made up of the run lists which define what recipes to run during a chef-client as well as the attributes that define information about the node.  

Attributes are build during the chef-client run process:  
- Data about the node is collected by Ohai  
- THe node object previously saved during the last chef-client run  
- THe rebuilt node object from the current chef-client run  

Once the node object is rebuilt, all attributes are compared and then updated based on attribute precendence. At the end of every chef-client run, the node object that defines the current state of the node is uplaoded to the chef server to be searched.  
