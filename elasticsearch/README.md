# Elasticsearch

## Image Utilities
Following are utilities included in the image to facilitate running commonly used
administration commands.  Use of these utilities assumes you have `exec` access
to the container.  Some of these utilities make use of the administration certs which have
full access to all indices and operations of the Elasticsearch cluster.

### es_util
Run any REST command against the Elasticsearch endpoint

Example to retrieve the list of nodes
```
$ oc exec -c elasticsearch $POD -- es_util --query=_cat/nodes?pretty
```

### allocate-shard
Manually allocate a shard to a given node in the Elasticsearch cluster.  The node
by default will be the one on which the command is executed. This command
is used to explicitly [reroute and allocate](https://www.elastic.co/guide/en/elasticsearch/reference/2.4/cluster-reroute.html)
a shard

Example:
```
$ oc exec -c elasticsearch $POD -- allocate-shard .kibana.02c55f18a892b365bcd1802db9e5c9df39c04674
```

### logs
Retrieve Elasticsearch logs from the log directory. This command defaults to retrieving
the file `/elasticsearch/logging-es/logs/logging-es.log` which may not be directed to
`STDOUT` by the logging configuration.

Example to follow the default file:
```
$ oc exec -c elasticsearch $POD -- logs -f
```

### move-replica-shard
Manually move a shard to a given node in the Elasticsearch cluster.

Example:
```
$ oc exec -c elasticsearch $POD -- move-replica-shard .kibana.02c55f18a892b365bcd1802db9e5c9df39c04674 0 source-node target-node
```

### shards
List information about shards in the cluster

Example to find unassigned shards:
```
$ oc exec -c elasticsearch $POD -- shards | grep UNASSIGNED
```
