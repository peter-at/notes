# cloud

# cloud-init
* debugging and find logs - [link](./cloud/cloud-init.md#logs)
* render in terraform - [link](./cloud/cloud-init.md#writing-files)
* apt for HashiCorp - [link](./cloud/cloud-init.md#apt-for-hashicorp)

# vagrant
* scp files - [link](./cloud/vagrant.md#scp-files)

# consul

## start / stop agent
`consul agent -dev` to start agent
`consul leave` gracefully stop the agent on the same machine
force kill will result in node still in catalog but marked as failed -- results in health critical
`consul agent -dev -enable-script-checks -config-dir=./consul.d` starts consul server grab service defs in ./consul.d/*.json

## discover nodes
* gossip protocol - eventual consistent
`consul members`
* direct HTTP API - forwards to server
`curl localhost:8500/v1/catalog/nodes`
* via DNS - on port 8600 where agent is running
`dig @127.0.0.1 -p 8600 consul.node.consul`

## discover services
`dig @127.0.0.1 -p 8600 web.service.consul`
`dig @127.0.0.1 -p 8600 web.service.consul SRV`
`dig @127.0.0.1 -p 8600 rails.web.service.consul` - query by TAG.NAME.service.consul
DNS will not return answers if failed health check

__using HTTP API__
`curl http://localhost:8500/v1/catalog/service/web`
`curl 'http://localhost:8500/v1/health/service/web?passing'` - filter only healthy services

## addr
server - `consul agent -dev -enable-script-checks -client=192.168.64.15`
client - `consul leave -http-addr=192.168.64.15:8500`

