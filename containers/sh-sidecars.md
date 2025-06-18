# Container sidecar shells

> Having tools in a container might sometimes be useful for different debugging. Just exec into it and start the diagnostics. With a distroless/minimized image, it's not that easy. What we can do instead is attach a sidecar container:
> 
```sh
docker run \
  --rm \
  -it \
  --pid=container:<container id> \
  --net=container:<container id> \
  --cap-add sys_admin \
  alpine \
  sh
```


from:  [Minimal containers using Nix](https://tmp.bearblog.dev/minimal-containers-using-nix/)



This starts an Alpine container that attaches to the same PID and network namespaces, giving you visibility into what's running inside the original container.


To make this easier, you can define a Bash function:

```bash
sidecar-sh() {
  local target_container="$1"
  local container_id
  container_id=$(docker inspect --format '{{.Id}}' "$target_container")
  
  docker run --rm -it \
    --pid=container:"$container_id" \
    --net=container:"$container_id" \
    --cap-add sys_admin \
    alpine sh
}

```

**Usage:** `sidecar-sh <container-name>` and you will be alongside your process  