# CouchDB Exporter

[![Build Status](https://travis-ci.org/gesellix/couchdb-exporter.svg?branch=master)](https://travis-ci.org/gesellix/couchdb-exporter)

[CouchDB](http://couchdb.apache.org/) exporter for [Prometheus](http://prometheus.io/)

The CouchDB Exporter requests the CouchDB stats from the `/_stats` and `_active_tasks` endpoints and 
exposes them for Prometheus consumption. You can optionally monitor detailed database stats like
disk size or fragmentation.

## Run it as container

```
docker run -p 9984:9984 gesellix/couchdb-exporter -couchdb.uri=http://couchdb:5984
```

The couchdb-exporter uses the [glog](https://godoc.org/github.com/golang/glog) library for logging.
With the default parameters nothing will be logged.
Use `-logtostderr` to enable logging to stderr and `--help` to see all options.

For CouchDB 2.x, you should configure the exporter to fetch the stats from one node, to get
a complete cluster overview. In contrast to CouchDB 1.x you'll need to configure the admin
credentials, e.g. like this:

```
docker run -p 9984:9984 gesellix/couchdb-exporter -couchdb.uri=http://couchdb:5984 -couchdb.username=root -couchdb.password=a-secret
```

## Metrics Overview
The following list gives you an overview on the currently exposed metrics.
Please note that beyond the complete CouchDB stats a metric `couchdb_up` has been
added as minimal connection health check.

The `node_name` label defaults to `master` for CouchDB 1.x and `nonode@nohost` for a single CouchDB 2.x setup.
The node name makes more sense in a clustered CouchDB 2.x environment with several nodes to provide separated stats for each node. 

```
# HELP couchdb_database_data_size data size
# TYPE couchdb_database_data_size gauge
couchdb_database_data_size{db_name="db-1",node_name="nonode@nohost"} 1.764941e+06
couchdb_database_data_size{db_name="db-2",node_name="nonode@nohost"} 4.93011369e+08
# HELP couchdb_database_disk_size disk size
# TYPE couchdb_database_disk_size gauge
couchdb_database_disk_size{db_name="db-1",node_name="nonode@nohost"} 2.098663e+06
couchdb_database_disk_size{db_name="db-2",node_name="nonode@nohost"} 6.60108847e+08
# HELP couchdb_database_overhead disk size overhead
# TYPE couchdb_database_overhead gauge
couchdb_database_overhead{db_name="db-1",node_name="nonode@nohost"} 333722
couchdb_database_overhead{db_name="db-2",node_name="nonode@nohost"} 1.67097478e+08
# HELP couchdb_httpd_auth_cache_hits number of authentication cache hits
# TYPE couchdb_httpd_auth_cache_hits gauge
couchdb_httpd_auth_cache_hits{node_name="nonode@nohost"} 0
# HELP couchdb_httpd_auth_cache_misses number of authentication cache misses
# TYPE couchdb_httpd_auth_cache_misses gauge
couchdb_httpd_auth_cache_misses{node_name="nonode@nohost"} 0
# HELP couchdb_httpd_bulk_requests number of bulk requests
# TYPE couchdb_httpd_bulk_requests gauge
couchdb_httpd_bulk_requests{node_name="nonode@nohost"} 1871
# HELP couchdb_httpd_clients_requesting_changes number of clients for continuous _changes
# TYPE couchdb_httpd_clients_requesting_changes gauge
couchdb_httpd_clients_requesting_changes{node_name="nonode@nohost"} 0
# HELP couchdb_httpd_database_reads number of times a document was read from a database
# TYPE couchdb_httpd_database_reads gauge
couchdb_httpd_database_reads{node_name="nonode@nohost"} 8
# HELP couchdb_httpd_database_writes number of times a database was changed
# TYPE couchdb_httpd_database_writes gauge
couchdb_httpd_database_writes{node_name="nonode@nohost"} 14955
# HELP couchdb_httpd_open_databases number of open databases
# TYPE couchdb_httpd_open_databases gauge
couchdb_httpd_open_databases{node_name="nonode@nohost"} 16
# HELP couchdb_httpd_open_os_files number of file descriptors CouchDB has open
# TYPE couchdb_httpd_open_os_files gauge
couchdb_httpd_open_os_files{node_name="nonode@nohost"} 16
# HELP couchdb_httpd_request_methods number of HTTP requests by method
# TYPE couchdb_httpd_request_methods gauge
couchdb_httpd_request_methods{method="COPY",node_name="nonode@nohost"} 0
couchdb_httpd_request_methods{method="DELETE",node_name="nonode@nohost"} 0
couchdb_httpd_request_methods{method="GET",node_name="nonode@nohost"} 1465
couchdb_httpd_request_methods{method="HEAD",node_name="nonode@nohost"} 0
couchdb_httpd_request_methods{method="POST",node_name="nonode@nohost"} 2995
couchdb_httpd_request_methods{method="PUT",node_name="nonode@nohost"} 152
# HELP couchdb_httpd_request_time length of a request inside CouchDB without MochiWeb
# TYPE couchdb_httpd_request_time gauge
couchdb_httpd_request_time{node_name="nonode@nohost"} 0
# HELP couchdb_httpd_requests number of HTTP requests
# TYPE couchdb_httpd_requests gauge
couchdb_httpd_requests{node_name="nonode@nohost"} 4611
# HELP couchdb_httpd_status_codes number of HTTP responses by status code
# TYPE couchdb_httpd_status_codes gauge
couchdb_httpd_status_codes{code="200",node_name="nonode@nohost"} 2422
couchdb_httpd_status_codes{code="201",node_name="nonode@nohost"} 2171
couchdb_httpd_status_codes{code="202",node_name="nonode@nohost"} 0
couchdb_httpd_status_codes{code="301",node_name="nonode@nohost"} 0
couchdb_httpd_status_codes{code="304",node_name="nonode@nohost"} 4
couchdb_httpd_status_codes{code="400",node_name="nonode@nohost"} 0
couchdb_httpd_status_codes{code="401",node_name="nonode@nohost"} 2
couchdb_httpd_status_codes{code="403",node_name="nonode@nohost"} 0
couchdb_httpd_status_codes{code="404",node_name="nonode@nohost"} 9
couchdb_httpd_status_codes{code="405",node_name="nonode@nohost"} 0
couchdb_httpd_status_codes{code="409",node_name="nonode@nohost"} 0
couchdb_httpd_status_codes{code="412",node_name="nonode@nohost"} 1
couchdb_httpd_status_codes{code="500",node_name="nonode@nohost"} 0
# HELP couchdb_httpd_temporary_view_reads number of temporary view reads
# TYPE couchdb_httpd_temporary_view_reads gauge
couchdb_httpd_temporary_view_reads{node_name="nonode@nohost"} 0
# HELP couchdb_httpd_up Was the last query of CouchDB stats successful.
# TYPE couchdb_httpd_up gauge
couchdb_httpd_up 1
# HELP couchdb_httpd_view_reads number of view reads
# TYPE couchdb_httpd_view_reads gauge
couchdb_httpd_view_reads{node_name="nonode@nohost"} 0
# HELP couchdb_server_active_tasks active tasks
# TYPE couchdb_server_active_tasks gauge
couchdb_server_active_tasks{database_compaction="0",indexer="0",node_name="nonode@nohost",replication="1",view_compaction="0"} 1
# HELP go_gc_duration_seconds A summary of the GC invocation durations.
# TYPE go_gc_duration_seconds summary
go_gc_duration_seconds{quantile="0"} 0
go_gc_duration_seconds{quantile="0.25"} 0
go_gc_duration_seconds{quantile="0.5"} 0
go_gc_duration_seconds{quantile="0.75"} 0
go_gc_duration_seconds{quantile="1"} 0
go_gc_duration_seconds_sum 0
go_gc_duration_seconds_count 0
# HELP go_goroutines Number of goroutines that currently exist.
# TYPE go_goroutines gauge
go_goroutines 9
# HELP go_memstats_alloc_bytes Number of bytes allocated and still in use.
# TYPE go_memstats_alloc_bytes gauge
go_memstats_alloc_bytes 791456
# HELP go_memstats_alloc_bytes_total Total number of bytes allocated, even if freed.
# TYPE go_memstats_alloc_bytes_total counter
go_memstats_alloc_bytes_total 791456
# HELP go_memstats_buck_hash_sys_bytes Number of bytes used by the profiling bucket hash table.
# TYPE go_memstats_buck_hash_sys_bytes gauge
go_memstats_buck_hash_sys_bytes 2482
# HELP go_memstats_frees_total Total number of frees.
# TYPE go_memstats_frees_total counter
go_memstats_frees_total 168
# HELP go_memstats_gc_cpu_fraction The fraction of this program's available CPU time used by the GC since the program started.
# TYPE go_memstats_gc_cpu_fraction gauge
go_memstats_gc_cpu_fraction 0
# HELP go_memstats_gc_sys_bytes Number of bytes used for garbage collection system metadata.
# TYPE go_memstats_gc_sys_bytes gauge
go_memstats_gc_sys_bytes 137216
# HELP go_memstats_heap_alloc_bytes Number of heap bytes allocated and still in use.
# TYPE go_memstats_heap_alloc_bytes gauge
go_memstats_heap_alloc_bytes 791456
# HELP go_memstats_heap_idle_bytes Number of heap bytes waiting to be used.
# TYPE go_memstats_heap_idle_bytes gauge
go_memstats_heap_idle_bytes 573440
# HELP go_memstats_heap_inuse_bytes Number of heap bytes that are in use.
# TYPE go_memstats_heap_inuse_bytes gauge
go_memstats_heap_inuse_bytes 1.2288e+06
# HELP go_memstats_heap_objects Number of allocated objects.
# TYPE go_memstats_heap_objects gauge
go_memstats_heap_objects 5922
# HELP go_memstats_heap_released_bytes Number of heap bytes released to OS.
# TYPE go_memstats_heap_released_bytes gauge
go_memstats_heap_released_bytes 0
# HELP go_memstats_heap_sys_bytes Number of heap bytes obtained from system.
# TYPE go_memstats_heap_sys_bytes gauge
go_memstats_heap_sys_bytes 1.80224e+06
# HELP go_memstats_last_gc_time_seconds Number of seconds since 1970 of last garbage collection.
# TYPE go_memstats_last_gc_time_seconds gauge
go_memstats_last_gc_time_seconds 0
# HELP go_memstats_lookups_total Total number of pointer lookups.
# TYPE go_memstats_lookups_total counter
go_memstats_lookups_total 7
# HELP go_memstats_mallocs_total Total number of mallocs.
# TYPE go_memstats_mallocs_total counter
go_memstats_mallocs_total 6090
# HELP go_memstats_mcache_inuse_bytes Number of bytes in use by mcache structures.
# TYPE go_memstats_mcache_inuse_bytes gauge
go_memstats_mcache_inuse_bytes 4800
# HELP go_memstats_mcache_sys_bytes Number of bytes used for mcache structures obtained from system.
# TYPE go_memstats_mcache_sys_bytes gauge
go_memstats_mcache_sys_bytes 16384
# HELP go_memstats_mspan_inuse_bytes Number of bytes in use by mspan structures.
# TYPE go_memstats_mspan_inuse_bytes gauge
go_memstats_mspan_inuse_bytes 16264
# HELP go_memstats_mspan_sys_bytes Number of bytes used for mspan structures obtained from system.
# TYPE go_memstats_mspan_sys_bytes gauge
go_memstats_mspan_sys_bytes 16384
# HELP go_memstats_next_gc_bytes Number of heap bytes when next garbage collection will take place.
# TYPE go_memstats_next_gc_bytes gauge
go_memstats_next_gc_bytes 4.473924e+06
# HELP go_memstats_other_sys_bytes Number of bytes used for other system allocations.
# TYPE go_memstats_other_sys_bytes gauge
go_memstats_other_sys_bytes 552526
# HELP go_memstats_stack_inuse_bytes Number of bytes in use by the stack allocator.
# TYPE go_memstats_stack_inuse_bytes gauge
go_memstats_stack_inuse_bytes 294912
# HELP go_memstats_stack_sys_bytes Number of bytes obtained from system for stack allocator.
# TYPE go_memstats_stack_sys_bytes gauge
go_memstats_stack_sys_bytes 294912
# HELP go_memstats_sys_bytes Number of bytes obtained from system.
# TYPE go_memstats_sys_bytes gauge
go_memstats_sys_bytes 2.822144e+06
# HELP go_threads Number of OS threads created
# TYPE go_threads gauge
go_threads 6
```

## Thanks

Thanks go to the Prometheus team, which is very active and responsive!

I also have to admit that the couchdb-exporter code is heavily inspired by 
the other [available exporters](http://prometheus.io/docs/instrumenting/exporters/), 
and that some ideas have just been copied from them.
