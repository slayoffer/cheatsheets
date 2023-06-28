### Check connection

```bash
curl https://localhost:9200 -ku 'user:password'
```

### Get cluster info

```bash
curl -ku 'user:password' -XGET https://localhost:9200/_cluster/health?pretty
```

### Check indices

```bash
curl -ku 'user:password' -XGET "https://localhost:9200/_cat/indices?v" 
# ze3JN71hyhhCYDQB is password on docker-2 server
```

### Set shards amount

```bash
curl -ku 'user:password' -XPUT 'https://localhost:9200/_settings' -H 'Content-Type: application/json' -d '{ "persistent": { "cluster.max_shards_per_node": "3000" } }'
```

### Get shards count for indices

```bash
# for all
curl -ku 'user:password' -XGET '/_cluster/state/metadata?filter_path=metadata.indices.*.settings.index.number_of_shards'

# for one
curl -ku 'user:password' -XGET '/_cluster/state/metadata?filter_path=metadata.indices.test-index.settings.index.number_of_shards'
```

### Delete indices

```bash
# via curl
curl -ku 'user:password' -XDELETE 'https://localhost:9200/docker-2-filebeat-*'

# or get into Kibana Dev tools and run
POST /docker-%25%7Bapplication_name%7D-*/_delete_by_query
{
  "query": {
    "match_all": {}
  }
}
```

### Clear cache

```bash
curl -ku 'admin:admin' -XPOST 'https://localhost:9200/_cache/clear'
```

### Apply policy

```bash
curl -X PUT "<your_opensearch_host>:<your_opensearch_port>/your_index_name/_settings" -H 'Content-Type: application/json' -d' {   "index.opendistro.index_state_management.policy_id": "your_policy_name" } '
```