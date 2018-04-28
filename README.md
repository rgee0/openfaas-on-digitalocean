## OpenFaaS-DO

Ansible Playbook to get you up and running with OpenFaas on Digital Ocean.  This is intended for evaluation / testing where quick and repeatable access to a clean instance is desired.

Please be aware this playbook is not idempotent, specifically in droplet creation; if you run it _n_ times you will get _n_ droplets.

### Pre-requisites

* [Install Ansible](http://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
* [PIP](https://pip.pypa.io/en/stable/installing/) and [DOPY](https://pypi.org/project/dopy)
* [Digital Ocean](https://www.digitalocean.com/) account

### Configure Variables

Configure the [create_droplet role variables](create_droplet/vars/main.yml) according to your needs.

```yml

  token: <your DO API token>
  sshid: <your ssh key ids>
  size_slug: <your required vm size>
  region: <your desired region>
  image: ubuntu-17-10-x64

```

* token

Generated under the API page on the DO portal

* sshid

The ssh key IDs can be found but running:
```sh
$ export DOTOKEN=<API token>
$ curl -X GET -H 'Content-Type: application/json' -H 'Authorization: Bearer '$DOTOKEN "https://api.digitalocean.com/v2/account/keys" 2>/dev/null | python -m json.tool
```

* size_slug

These can be found on [Digital Ocean](https://developers.digitalocean.com/documentation/changelog/api-v2/new-size-slugs-for-droplet-plan-changes/)

or through the API:
```sh
$ curl -X GET -H 'Content-Type: application/json' -H 'Authorization: Bearer '$DOTOKEN "https://api.digitalocean.com/v2/sizes" 2>/dev/null | python -m json.tool
```

* region

```
$ curl -X GET -H 'Content-Type: application/json' -H 'Authorization: Bearer '$DOTOKEN "https://api.digitalocean.com/v2/regions" 2>/dev/null | python -m json.tool
```

### Run the playbook

```sh
$ ansible-playbook site.yml
```

### Head over to the UI

Head to http://<droplet_ip>:8080 to find the UI of your new OpenFaaS instance.  
There may be a slight delay as the images are downloaded and initialised.
