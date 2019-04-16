# Ansible Playbook

A basic Ansible Playbook project to provision VMs / servers for PHP projects

## Building / Provisioning

### VM

Clone Ansible Repo

``` bash
git clone git@github.com:andrewjt71/personal-ansible.git
```

Clone site

``` bash
git clone git@github.com:andrewjt71/personal-website.git
```

Generate vagrant key

``` bash
ssh-add ~/.vagrant.d/insecure_private_key
```

Boot up the VM

```bash
vagrant up
```

Deploy

``` bash
./bin/build_vm.sh
```

## Production

Clone Ansible Repo

``` bash
git clone git@github.com:andrewjt71/personal-ansible.git
```

Generate a deployment key - call it personalsitedeploy. This will be used for communication with Github

``` bash
ssh-keygen
```

Add deployment key to website's github project with write access
https://developer.github.com/v3/guides/managing-deploy-keys/#deploy-keys

Generate a deployment key - call it digitalocean. This will be used for communication with Digital Ocean

``` bash
ssh-keygen
```

Add ssh key to Digital Ocean droplet during creation

Deploy

``` bash
./bin/build_prod.sh
```

### Test production build on VM

Generate a deployment key - call it personalsitedeploy

``` bash
ssh-keygen
```

Comment out the `config.vm.synced_folder` line in the Vagrantfile

Modify playbook-production-build.yml setting the host to `192.168.33.99`

Boot up the VM

```bash
vagrant up
```

Add deployment key to website's github project with write access
https://developer.github.com/v3/guides/managing-deploy-keys/#deploy-keys

Deploy

``` bash
./bin/deploy_prod_vm.sh
```

## Deploying

### VM
Deploy

``` bash
./bin/deploy_vm.sh
```

### Production

Deploy

``` bash
./bin/deploy_prod.sh
```

## Permissions Explained

### VM

- Nginx runs as nginx user
- FPM runs as php-fpm user (listen.owner and listen.group are set to nginx in /etc/php-fpm.d/www.conf)
- php-fpm user / group owns the cache and log dirs
- deploy user owns the code folders, and should be used for composer installs, git clones, npm installs etc
- deploy user is in the php-fpm group, and therefore can write to cache / logs where necessary e.g. composer install

### Prod

- Nginx runs as nginx user
- FPM runs as php-fpm user (listen.owner and listen.group are set to nginx in /etc/php-fpm.d/www.conf)
- php-fpm user / group owns the cache and log dirs
- root owns the code folders as they should never be written to. The deployment and runtime processes have been separated
- deploy user is used to clone the github repo and build code from within the staging folder which it is the owner for.
Once generated, the permissions on the generated code are switched to root, and the contents are rsynched into the 
directory which live points to. No manual build steps should ever be run on the code in the live folder.


### Upgrading node

https://github.com/boxuk/ansible-boxuk-roles-common/blob/master/roles/yum_repo/templates/nodesource-el-7.repo.j2
https://github.com/boxuk/ansible-boxuk-roles-common/blob/master/roles/yum_repo/templates/nodesource-el-0.12.repo.j2
