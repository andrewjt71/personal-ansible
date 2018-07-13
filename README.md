# Ansible Playbook

A basic Ansible Playbook project to provision VMs / servers for PHP projects

## VM Provisioning

``` bash
vagrant up
ssh-add ~/.vagrant.d/insecure_private_key
ansible-playbook playbook-vm.yml --private-key=/Users/.../.vagrant.d/insecure_private_key --become --become-user root --user vagrant
Vagrant ssh
sudo su - personal-site-app
cd /opt/sites/personal-website/
./bin/deploy
```

## Live Provisioning: 

``` bash
ansible-playbook playbook-live.yml --private-key=/Users/.../.ssh/deploy_key --become --become-user root
```

## Permissions
runtime:
nginx runs as nginx
php-fpm runs as a app fpm user (mywebfpm)

connections/socks:
php-fpm socket file is owned by nginx so nginx has access to it, or: if not using sock file and instead using a port (e.g. 127.0.0.1:9000) then nginx needs access to this, easy since its localhost

directory permissions:
php-fpm user (mywebfpm) needs read/write access to app cache and app log directories
the entire app folder which is "immutable" (e.g. deploy, nothing will ever change it) should be owned by root. lock everyone out
if we need to run stuff like assetic dump (nooo) then either 1) change ownership of the entire app folder to 'mywebapp' and run the commands as that, or 2) change ownership of individual folders, e.g. mywebapp has write access to ./web/assets
fpm only needs read access to code dir
