# docker-notes

## v2 registry curls for image list and tag list
```
$ curl -ulogin  -X GET https://host/v2/_catalog
{ "repositories": [ "one-repo", "username/repo" ] }

$ curl -ulogin -X GET https://host/v2/imagename/tags/list > l2
{ "name": "nnnn", "tags": [ "tags...", "latest" ] }
```
