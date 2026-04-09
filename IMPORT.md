## Galaxy Import

To import the role into Ansible Galaxy for the first time, you can use the following command:

```bash
export AG_TOKEN=...
export AG_USER=....
ansible-galaxy role import --token $AG_TOKEN --branch main --role-name molecule_openstack $AG_USER molecule_openstack
```

## Galaxy Delete

### Role

````shell
export AG_TOKEN=...
export AG_USER=....
ansible-galaxy role delete --token $AG_TOKEN $AG_USER molecule_openstack
````

## Galaxy Token
To get your Galaxy token, follow this url: https://galaxy.ansible.com/ui/token/