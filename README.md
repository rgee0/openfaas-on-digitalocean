## OpenFaaS-DO

Ansible Playbook to get you up and running with OpenFaas on Digital Ocean.  This is intended for evaluation / testing where quick and repeatable access to a clean instance is desired.

Please be aware this playbook is not idempotent, specifically in droplet creation; if you run it _n_ times you will get _n_ droplets.

Both swarm and kubernetes deployments are possible but passing `-e "orchestrator=swarm"` or `-e "orchestrator=k8s"`. 

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

* Using Swarm
```sh
$ ansible-playbook site.yml -e "orchestrator=swarm"
```

* Using Kubernetes
```sh
$ ansible-playbook site.yml -e "orchestrator=k8s"
```

### Head over to the UI

Check the output of the final task to find the location of the OpenFaaS UI.  For example:
```
ok: [209.97.188.227] => {
    "msg": [
        "OpenFaaS Gateway: http://209.97.188.227:8080",
        "Gateway User: admin ",
        "Gateway Password: c2db2b6d59df518392a81eff57c52486f9f2f46e2480d9de150904bbce0560bc",
        "CLI Gateway Login: echo -n c2db2b6d59df518392a81eff57c52486f9f2f46e2480d9de150904bbce0560bc | faas-cli login --username=admi
n --password-stdin -g http://209.97.188.227:8080"
    ]
}

PLAY RECAP **************************************************************************************************************************
209.97.188.227             : ok=12   changed=6    unreachable=0    failed=0
localhost                  : ok=6    changed=2    unreachable=0    failed=0
```

As this deploys to public cloud basic auth is enabled by default for both orchestrators.