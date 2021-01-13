# inbox

## docker

* get oci labels
  local oci_labels=$(docker inspect --format='{{json .Config.Labels}}' $img \
	  | jq -c -r 'to_entries[]|select(.key|test("opencontainers"))|[.key, .value]')
* oci label [link](https://github.com/opencontainers/image-spec/blob/master/annotations.md#pre-defined-annotation-keys) 
