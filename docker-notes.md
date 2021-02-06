# docker-notes

## v2 registry curls for image list and tag list
```
$ curl -ulogin  -X GET https://host/v2/_catalog
{ "repositories": [ "one-repo", "username/repo" ] }

$ curl -ulogin -X GET https://host/v2/imagename/tags/list > l2
{ "name": "nnnn", "tags": [ "tags...", "latest" ] }
```

## labels

* get oci labels
  ```
  local oci_labels=$(docker inspect --format='{{json .Config.Labels}}' $img \
	  | jq -c -r 'to_entries[]|select(.key|test("opencontainers"))|[.key, .value]')
  ```	  
* oci label [link](https://github.com/opencontainers/image-spec/blob/master/annotations.md#pre-defined-annotation-keys) 
