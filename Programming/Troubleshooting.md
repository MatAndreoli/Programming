This document has some solutions for troubleshooting.

## Docker Troubleshooting

If docker doesn't connect:
```shell
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
```

Just run it, it'll start the docker service:
```shell
$ sudo service docker start
```

## WSL
If the service ``VmmemWSL`` is consuming high amounts of memory do it:
Go to %UserProfile%/.wslconfig (If the file doesn't exist, just create it).

Write in the file:
```
[wsl2]
memory=2GB
```
