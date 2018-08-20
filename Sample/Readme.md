Run

```
docker pull alpine
```

- Delete image by id

```
docker rmi -f id
```

The -f argument is used to remove images even when they are being
used by containers

- Deleting All Images

```
docker rmi -f $(docker images -q)
```

The -q argument specifies the oddly named quiet mode, which returns only the IMAGE ID values from
the docker images command, which are processed by the docker rmi command, removing all the images
in the list.
