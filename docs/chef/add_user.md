### Adding Users to Chef Users Recipe

Create a new data bag item for the user. On the Chef workstation, create a ~/chef/chef-repo/data_bags/users/<username>.json file. Example:
`$ vim billybob.json`

The password should be the hashed password from */etc/shadow* (`cat /etc/shadow | grep -i billybob`):
```
{
  "id": "billybob",
  "comment": "Billy Bobberino",
  "home": "/home/billybob",
  "shell": "/bin/bash",
  "password": "iadfoi234io234ioasdfiasdf3323i4oi234"
}
```

Upload the data_bag item to the Chef server:
`$ knife data bag from file users <data bag item>`

Update the cookbook:
`$ knife cookbook upload system_users`
