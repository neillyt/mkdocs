### Attribute Precedence

Attributes are specific details about a node. Attributes describe:  
- The current state of the node  
- What the state of the node was at the end of the previous chef-client run  
- What the state of the node should be at the of the current chef-client run  

Attributes can be defined by:  
- The state of the node (using Ohai which runs at the beginning of a chef-client run)  
- Cookbooks (our attribute files)  
- Roles  
- Environments  

After a node object is rebuilt in the chef run, all attributes loaded in the chef-client are then compared. The node is updated based on attribute precendence and at the very end of the convergence (chef-client) the node object is uploaded to the chef server.  

- Node object defines the current state of the node (made up of attributes)  
- Node object is stored on the chef server so that it can be searched  
- Node object is updated at each convergence  

If there are attributes with the same names then attribute precedence determines which attribute is applied to the node and the node object.

### Attributes Precedence: Levels of Precedence

- default: Automatically reset at the start of every chef-client run is the lowest level of precedence.
- force_default: Used in a cookbook or recipe to override an existing "default" attribute  
- normal: A setting that persists in the node object.  
- override: Automatically reset at the start of every chef-client, most often should be used only when required.  
- force_override: Used to ensure that an attributed defined in a cookbook (by an attribute file or by a recipe) takes precendence over an override attribute set by a role or an environment.  
- automatic: Contains data populated by Ohai at the beginning of every chef-client run and cannot be modified and always has the highest attribute precedence.
