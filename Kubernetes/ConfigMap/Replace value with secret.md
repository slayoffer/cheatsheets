### How to replace ConfigMap value with ENV from Gitlab

```yml
--- 
apiVersion: v1 
kind: ConfigMap 
metadata:   
	name: dtr-nats-creds 
data:   
	dtr_nats.creds: |     
		replaceme

# script for gitlab ci file
- awk -v config="$CI_NATS_CREDS" '{gsub("replaceme", config)}1' ./backend/templates/configmap.yaml > ./backend/templates/temp.yaml && mv ./backend/templates/temp.yaml ./backend/templates/configmap.yaml
```