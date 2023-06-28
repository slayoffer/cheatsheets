To check the status of all jobs, you can use the following command:

```bash
kubectl get jobs
```

This command lists all the jobs and their status. If the job is completed, it shows 1/1 or n/n (where n is the total number of tasks that job has to perform) in the COMPLETIONS column.

However, if you want to filter only those jobs that have completed, you can use a combination of `kubectl get jobs` with some commands in `jq` and `grep`:

```bash
kubectl get jobs -o json | jq -r '.items[] | "\(.status.succeeded) \(.metadata.name)"' | grep '^1' | awk '{print $2}'
```

In this command:
- `kubectl get jobs -o json` gets the list of jobs in JSON format.
- `jq -r '.items[] | "\(.status.succeeded) \(.metadata.name)"'` filters out the job names along with their completion status.
- `grep '^1'` filters out only those jobs that have status 1 (i.e., completed jobs).
- `awk '{print $2}'` prints only the job names.

Please note that you'll need to have `jq` installed on your system to use this command.

Also, if you are dealing with a specific namespace, you need to specify the namespace with `-n <namespace>` in the `kubectl get jobs` command. 

This command can be considered as a workaround as Kubernetes doesn't provide a native way to filter jobs based on their status as of my knowledge cut-off in September 2021.