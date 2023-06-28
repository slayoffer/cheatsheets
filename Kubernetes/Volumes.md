### Read modes

```yml
# single pod access to volume

kind: PersistentVolumeClaim
apiVersion: v1
metadata:
	name: single-writer-only
spec:
	accessModes:
		ReadWriteOncePod # Allow only a single pod to access single-writer-only.
resources:
	requests:
		storage: 16i

```