## Galaxy Import

To import the role into Ansible Galaxy for the first time, you can use the following command:

```bash
ansible-galaxy role import --token <your-galaxy-token> --branch main --role-name molecule_openstack <your-github-username> molecule_openstack
```

## Galaxy Delete

### Role

````shell
ansible-galaxy role delete --token <your-galaxy-token> <your-github-username> molecule_openstack
````

## Galaxy Token
To get your Galaxy token, follow this url: https://galaxy.ansible.com/ui/token/