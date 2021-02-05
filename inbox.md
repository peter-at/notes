# inbox

## docker

* get oci labels
  local oci_labels=$(docker inspect --format='{{json .Config.Labels}}' $img \
	  | jq -c -r 'to_entries[]|select(.key|test("opencontainers"))|[.key, .value]')
* oci label [link](https://github.com/opencontainers/image-spec/blob/master/annotations.md#pre-defined-annotation-keys) 

## cloud-init for hashicorp

* official [linux repo](https://www.hashicorp.com/blog/announcing-the-hashicorp-linux-repository)
key in `curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -`
```
apt:
  sources:
    hashicorp:
      source: deb https://apt.releases.hashicorp.com $RELEASE main
      key: |
        -----BEGIN PGP PUBLIC KEY BLOCK-----
        
        mQINBF60TuYBEADLS1MP7XrMlRkn1Y54cb2UclUMH8HkIRfBrhk5Leo9kNZc/2QD
        LmdQbi3UbZkz0uVkHqbFDgV5lAnukCnxgr9BqnL0GJpO78le7gCCbM5bR4rTJ6Ar

```
