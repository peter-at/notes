# cloud-init

## apt for hashicorp

* official [linux repo](https://www.hashicorp.com/blog/announcing-the-hashicorp-linux-repository)
* key in `curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -`
```
write_files:
  - path: /etc/apt/apt.conf.d/02-hashicorp
    content: |
      # proxy for HashiCorp
      Acquire::https::Proxy::apt.releases.hashicorp.com "http://proxy";
apt:
  sources:
    hashicorp:
      source: deb https://apt.releases.hashicorp.com $RELEASE main
      key: |
        -----BEGIN PGP PUBLIC KEY BLOCK-----
        
        mQINBF60TuYBEADLS1MP7XrMlRkn1Y54cb2UclUMH8HkIRfBrhk5Leo9kNZc/2QD
        LmdQbi3UbZkz0uVkHqbFDgV5lAnukCnxgr9BqnL0GJpO78le7gCCbM5bR4rTJ6Ar

```


## Cloud-Init Debugging [from gist](https://gist.github.com/RagedUnicorn/a70f8540c68e0a41e3e097a2e29130f1)

Cloud-init combined with terraform can be a powerful tool to provision instances on startup. Debugging scripts that are run by cloud-init however are not the easiest to debug.


### Logs

Usually on an Ubuntu machine a lot of what is happening can be found in the syslog

```
cat /var/log/syslog
```

By default cloud-init writes its logs to the following place

```
cat /var/log/cloud-init-output.log
```

Using a cloud-init config file this can be changed in the `output` key

```
output:
  all: '| tee -a /var/log/cloud-init-output.log'
```

The generated full cloud-config can be found in the following place:

```
cat /var/lib/cloud/instance/cloud-config.txt
```


### Writing Files

Writing files can be really helpful when additional scripts need to be pushed onto the instance and subsequently run. Terraforms provides `template_cloudinit_config` provider for creating a cloud config.

```hcl
data "template_cloudinit_config" "config" {
  gzip          = false
  base64_encode = false

  part {
    content_type = "text/cloud-config"
    content      = "${data.template_file.cloud-init.rendered}"
  }
}
```

The `content_type` is very important in this case. `template_cloudinit_config` can work with rendered templates allowing for cloud-init configs templates with dynamic parts.

**Note:** gzip and base64_encode settings can be set to false for easier debugging of the content on the created instance. Otherwise content on the server is usually both gzip and base64 encoded.

```
write_files:
- path: /tmp/instance-entrypoint.sh
  content: |
    ${instance_entrypoint}
  owner: root:root
  encoding: b64
  permissions: '0744'
```

Respective template example

```hcl
data "template_file" "cloud-init" {
  template = "${file("${path.module}/templates/cloud-init.tpl")}"

  vars {
    instance_entrypoint = "${base64encode(var.instance_entrypoint)}"
  }
}
```

**Note:** It is required in a lot of cases to base64 encode the content of the script to write. Otherwise its content interferes with the cloud-init structure.


### Execution Order

Often times scripts should be executed in a certain order and only after another is done. Terraforms `remote-exec` provider as an example has no knowledge about when the cloud-init script is finished.
This can be a problem if the script should only run after initialization is done. Cloud-init however executes commands in the order of how they are put into the cloud-config. This gives us good control over execution order.

```
write_files:
  - path: /tmp/ansible-playbook.yml
    content: |
      ${ansible_playbook_docker}
    owner: root:root
    encoding: b64
  - path: /tmp/instance-entrypoint.sh
    content: |
      ${instance_entrypoint}
    owner: root:root
    encoding: b64
    permissions: '0744'
runcmd:
  - [ ansible-playbook, /tmp/ansible-playbook.yml]
  - [ /tmp/instance-entrypoint.sh]    
```
