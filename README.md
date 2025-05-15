# fire-and-forget-linux

This is my attempt at making a relatively sane configuration for a linux server that will run containers for you. This repository is a companion piece to a big blog post / tutorial. You can read it [here](https://arktekk.no/blogs/2025_fire_and_forget_linux_p1). It's in three parts because of its length, but they're all published and ready to read. No useless waiting for sequels that are never finished!

## Playing and testing

Install [Vagrant](https://developer.hashicorp.com/vagrant) and [VirtualBox](https://www.virtualbox.org/) to get started. You'll also need [Ansible](https://docs.ansible.com/ansible/latest/index.html). I installed it with `homebrew`. I have mixed experiences with installing anything that uses Python with homebrew. Better suggestions are welcome in the issue tracker.

Here are some commands to get you started:

### Vagrant

Create a VM in virtualbox with ubuntu 24.04 and most of the setup:
```shell
vagrant up
```
Destroy the VM:
```
vagrant destroy -f 
```

Reapply `initialize.yml`:
```shell
vagrant provision
```

### Ansible

The `secrets.yml` in this repository contains correctly formatted secrets when decrypted. The key is `hugabuga`. I don't know why. These secrets are _terrible_ and don't work with anything. You should make better ones.

Make a new `secrets.yml` with `ansible-vault`:

```shell
# Opens $EDITOR
ansible-vault create secrets.yml
```

Edit your `secrets.yml`:
```shell
ansible-vault edit secrets.yml
```

View your secrets:
```shell
ansible-vault view secrets.yml
```

Run a playbook (this playbook requires that `initialize.yml` has run first):

```shell
ansible-playbook -i hosts.ini \
  --limit vagrant \
  --ask-vault-pass \
  apps.yml
```

Due to the poor quality of the secrets in the default `secrets.yml`, this will get the containers stuck in a crash loop that uses a lot of CPU. Find better containers!

### Caddy

Forward Caddy to your localhost interface, so you can interact with it:

```shell
vagrant ssh -L8443:localhost:443
```

Note that caddy is going to have a card time making certificates inside the Vagrant box. You can [use the tls directive](https://caddyserver.com/docs/caddyfile/directives/tls) to have it fake a certificate instead.

## Contributions

I want this repository to be a safe torch, a good starting place to give to someone who wants to self-host on the dark and scary internet.

I don't know everything, I might've missed something that you know is important. In that case, I'd really appreciate if you file a ticket or make a pull request! It's also possible there are bugs in here. 

## Using & License

This code is available under the [MIT License](./LICENSE.md). Feel free to use it anything, as long as you don't blame me. Happy hacking! 🥳