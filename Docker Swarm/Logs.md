### Find

```bash
docker service logs vote_worker 2>&1 | grep 7bOcbO0cd49ba7499

# for windows
docker service logs vote_worker 2>&1 | findstr 7bOcbO0cd49ba7499


```