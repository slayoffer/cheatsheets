### Check CPUs

```bash
# check CT CPUs
nproc
```

### CPUs

```bash
# to set that CT wont ever take more than 50% of CPU power of 1 CPU core from the docker host
docker run --cpus=.5 ubuntu

# or update existing CT
docker update --cpus=2.5 ubuntu # give it 2,5 cores

# set CPU share (default share is 1024)
docker container run --cpu-shares=512 webapp4 # will get twice less core usage time than default CTs

# to use only #0 and #1 core (2 cores)
docker run —it ——cpuset—cpus=O,2 busybox:1.36.O # only 0 and 2
# or
docker run —it ——cpuset—cpus=O-2 busybox:1.36.O # 0-2 (3 cores total)
```

### RAM

```bash
# check default CT ram settings
cat /sys/fs/cgroup/memory.max

# limit RAM
docker run --memory=512m ubuntu
# or
docker run -m 100m ubuntu

# set RAM and SWAP
docker container run --memory=512m --memory-swap=768m webapp # 256 mb forswap. It cant be set without RAM!

# set unlimited swap
docker container run --memory=512m --memory-swap=-1 webapp

# soft limit
docker container run --memory=512m --memory-swap=-1 --memory-reservation=100 webapp # set 100-200 mb limit
```

### Compose

```bash
    deploy:
      resources:
        limits:
          cpus: "4"
          memory: "512M"
	    reservations:
	      cpus: '0.25'
	      memory: 20M
```
```

### Stress test CT

```bash
docker run --rm -it progrium/stress --cpu 2 --io 1 --vm 2 --vm-bytes 128M --timeout 10s # args after image name are for Entrypoint

# check stress test logs with kernel logs (dmesg)
dmesg
```

### Check restrictions

```bash
docker inspect dkc-hercules-frontend-stage | jq '.[0].HostConfig | {CpuShares: .CpuShares, CpusetCpus: .CpusetCpus, Memory: .Memory, NanoCpus: .NanoCpus}'

# or
docker stats | grep ct_name
```