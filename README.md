# docker-volumes

## Description

Creating Volume Mount from docker run command & sharing same Volume Mounts among multiple containers

## Getting started

Display existing volumes:

```
$ docker volume ls
```

Create a new volume:

```
$ docker volume create my-volume
```

Inspect the volume to find the mountpoint (volume location):

```
$ docker volume inspect my-volume

[
    {
        "CreatedAt": "2020-01-18T04:09:03Z",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/my-volume/_data",        
        "Name": "my-volume",
        "Options": null,
        "Scope": "local"
    }
]
```

Run a container and mount the created volume to the root

```
$ docker run --rm -it --name c1 -v my-volume:/data alpine
```

Create a new file under `/data`:

```
$ cd data
$ echo "hello from c1" > hello.txt
$ cat hello.txt
```

Open other terminal window and run other container with the same volume:

```
$ docker run --rm -it --name c2 -v my-volume:/data alpine
```

Inspect the `/data` folder (the created file will be there):

```
$ cd data
$ ls
$ cat hello.txt
```

Exit from both containers (they will also be deleted automatically as they were created with the `--rm` flag). Ensure they are deleted:

```
$ docker ps -a
```

Run a new container attaching the existing volume:

```
$ docker run --rm -it --name c3 -v my-volume:/data alpine
```

Inspect the `/data` folder (the created file will still be there):

```
$ cd data
$ ls
$ cat hello.txt
```

Exit from the new container (it will also be deleted automatically as it was created with the `--rm` flag).

## Clean up
 
Delete the volume:

```
$ docker volume rm my-volume
```