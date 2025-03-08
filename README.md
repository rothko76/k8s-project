# k8s-project

Cloud Architecture course first project

# Kubernetes Project with Jenkins and Helm

## Overview
This project is an example implementation of a Kubernetes-based deployment pipeline using Jenkins and Helm. It is part of a cloud architecture course and demonstrates how to automate deployments using CI/CD best practices.

## Implementation choices
For convenience, all the artifacts (Jenkins CI/CD scripts, Helm charts, and Python programs) are kept within the same repository.

- Jenkins Setup: Jenkins was installed locally, and the agent was set to my local host (which is not ideal in a production environment). This setup allowed me to leverage my local machine to satisfy all dependencies and prerequisites. In a more robust setup, a dedicated CI/CD server or cloud-hosted Jenkins instance would be preferred.

- Helm Charts: The focus in the consumer and producer Helm charts was primarily on the deployment itself. While important components like the deployments, services, and ingress configurations were addressed, other elements such as ConfigMaps, Secrets, and persistent storage were not given as much attention in this initial setup. These areas can be further enhanced in future iterations.

- Folder Structure:

    - The consumer-charts and producer-charts directories contain the respective Helm charts for deploying the consumer and producer services on Kubernetes.
    - The consumer and producer directories contain the application code and Dockerfiles necessary for building the respective Docker images. Some slight modifications were made in the application code during development.

## Components
- **Kubernetes**: Orchestrates containerized applications.
- **Jenkins**: Automates the CI/CD pipeline.
- **Helm**: Manages Kubernetes deployments via Helm charts.
- **RabbitMQ**: Message broker for inter-service communication.

## Prerequisites
- Kubernetes cluster (e.g., Minikube, Kind, or a managed Kubernetes service)
- Helm installed (`helm version`)
- Jenkins installed and configured with required plugins
- Docker installed (`docker --version`)

## Installation and Setup

### Deploy RabbitMQ
```sh
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install my-rabbitmq bitnami/rabbitmq --set auth.username=guest --set auth.password=guest
```

### Deploy RabbitMQ prom exporter
```sh
helm install rabbitmq-exporter prometheus-community/prometheus-rabbitmq-exporter \
  --set rabbitmq.url="http://localhost:15672" \
  --set rabbitmq.user="guest" \
  --set rabbitmq.password="guest"
```

#### output example
```
# HELP go_gc_duration_seconds A summary of the pause duration of garbage collection cycles.
# TYPE go_gc_duration_seconds summary
go_gc_duration_seconds{quantile="0"} 3.6399e-05
go_gc_duration_seconds{quantile="0.25"} 3.6399e-05
go_gc_duration_seconds{quantile="0.5"} 3.6399e-05
go_gc_duration_seconds{quantile="0.75"} 3.6399e-05
go_gc_duration_seconds{quantile="1"} 3.6399e-05
go_gc_duration_seconds_sum 3.6399e-05
go_gc_duration_seconds_count 1
# HELP go_goroutines Number of goroutines that currently exist.
# TYPE go_goroutines gauge
go_goroutines 9
# HELP go_info Information about the Go environment.
# TYPE go_info gauge
go_info{version="go1.21.8"} 1
# HELP go_memstats_alloc_bytes Number of bytes allocated and still in use.
# TYPE go_memstats_alloc_bytes gauge
go_memstats_alloc_bytes 704728
# HELP go_memstats_alloc_bytes_total Total number of bytes allocated, even if freed.
# TYPE go_memstats_alloc_bytes_total counter
go_memstats_alloc_bytes_total 3.032136e+06
# HELP go_memstats_buck_hash_sys_bytes Number of bytes used by the profiling bucket hash table.
# TYPE go_memstats_buck_hash_sys_bytes gauge
go_memstats_buck_hash_sys_bytes 69666
# HELP go_memstats_frees_total Total number of frees.
# TYPE go_memstats_frees_total counter
go_memstats_frees_total 24650
# HELP go_memstats_gc_sys_bytes Number of bytes used for garbage collection system metadata.
# TYPE go_memstats_gc_sys_bytes gauge
go_memstats_gc_sys_bytes 3.28948e+06
# HELP go_memstats_heap_alloc_bytes Number of heap bytes allocated and still in use.
# TYPE go_memstats_heap_alloc_bytes gauge
go_memstats_heap_alloc_bytes 704728
# HELP go_memstats_heap_idle_bytes Number of heap bytes waiting to be used.
# TYPE go_memstats_heap_idle_bytes gauge
go_memstats_heap_idle_bytes 5.414912e+06
# HELP go_memstats_heap_inuse_bytes Number of heap bytes that are in use.
# TYPE go_memstats_heap_inuse_bytes gauge
go_memstats_heap_inuse_bytes 2.2528e+06
# HELP go_memstats_heap_objects Number of allocated objects.
# TYPE go_memstats_heap_objects gauge
go_memstats_heap_objects 3150
# HELP go_memstats_heap_released_bytes Number of heap bytes released to OS.
# TYPE go_memstats_heap_released_bytes gauge
go_memstats_heap_released_bytes 3.104768e+06
# HELP go_memstats_heap_sys_bytes Number of heap bytes obtained from system.
# TYPE go_memstats_heap_sys_bytes gauge
go_memstats_heap_sys_bytes 7.667712e+06
# HELP go_memstats_last_gc_time_seconds Number of seconds since 1970 of last garbage collection.
# TYPE go_memstats_last_gc_time_seconds gauge
go_memstats_last_gc_time_seconds 1.7414255902048545e+09
# HELP go_memstats_lookups_total Total number of pointer lookups.
# TYPE go_memstats_lookups_total counter
go_memstats_lookups_total 0
# HELP go_memstats_mallocs_total Total number of mallocs.
# TYPE go_memstats_mallocs_total counter
go_memstats_mallocs_total 27800
# HELP go_memstats_mcache_inuse_bytes Number of bytes in use by mcache structures.
# TYPE go_memstats_mcache_inuse_bytes gauge
go_memstats_mcache_inuse_bytes 9600
# HELP go_memstats_mcache_sys_bytes Number of bytes used for mcache structures obtained from system.
# TYPE go_memstats_mcache_sys_bytes gauge
go_memstats_mcache_sys_bytes 15600
# HELP go_memstats_mspan_inuse_bytes Number of bytes in use by mspan structures.
# TYPE go_memstats_mspan_inuse_bytes gauge
go_memstats_mspan_inuse_bytes 105840
# HELP go_memstats_mspan_sys_bytes Number of bytes used for mspan structures obtained from system.
# TYPE go_memstats_mspan_sys_bytes gauge
go_memstats_mspan_sys_bytes 130368
# HELP go_memstats_next_gc_bytes Number of heap bytes when next garbage collection will take place.
# TYPE go_memstats_next_gc_bytes gauge
go_memstats_next_gc_bytes 5.814576e+06
# HELP go_memstats_other_sys_bytes Number of bytes used for other system allocations.
# TYPE go_memstats_other_sys_bytes gauge
go_memstats_other_sys_bytes 1.190812e+06
# HELP go_memstats_stack_inuse_bytes Number of bytes in use by the stack allocator.
# TYPE go_memstats_stack_inuse_bytes gauge
go_memstats_stack_inuse_bytes 720896
# HELP go_memstats_stack_sys_bytes Number of bytes obtained from system for stack allocator.
# TYPE go_memstats_stack_sys_bytes gauge
go_memstats_stack_sys_bytes 720896
# HELP go_memstats_sys_bytes Number of bytes obtained from system.
# TYPE go_memstats_sys_bytes gauge
go_memstats_sys_bytes 1.3084534e+07
# HELP go_threads Number of OS threads created.
# TYPE go_threads gauge
go_threads 11
# HELP process_cpu_seconds_total Total user and system CPU time spent in seconds.
# TYPE process_cpu_seconds_total counter
process_cpu_seconds_total 0.01
# HELP process_max_fds Maximum number of open file descriptors.
# TYPE process_max_fds gauge
process_max_fds 1.048576e+06
# HELP process_open_fds Number of open file descriptors.
# TYPE process_open_fds gauge
process_open_fds 8
# HELP process_resident_memory_bytes Resident memory size in bytes.
# TYPE process_resident_memory_bytes gauge
process_resident_memory_bytes 1.2713984e+07
# HELP process_start_time_seconds Start time of the process since unix epoch in seconds.
# TYPE process_start_time_seconds gauge
process_start_time_seconds 1.74142558905e+09
# HELP process_virtual_memory_bytes Virtual memory size in bytes.
# TYPE process_virtual_memory_bytes gauge
process_virtual_memory_bytes 1.270239232e+09
# HELP process_virtual_memory_max_bytes Maximum amount of virtual memory available in bytes.
# TYPE process_virtual_memory_max_bytes gauge
process_virtual_memory_max_bytes 1.8446744073709552e+19
# HELP rabbitmq_exporter_build_info A metric with a constant '1' value labeled by version, revision, branch and build date on which the rabbitmq_exporter was built.
# TYPE rabbitmq_exporter_build_info gauge
rabbitmq_exporter_build_info{branch="HEAD",builddate="2024-03-18T06:03:43Z",revision="1859734",version="1.0.0"} 1
# HELP rabbitmq_module_up Was the last scrape of rabbitmq successful per module.
# TYPE rabbitmq_module_up gauge
rabbitmq_module_up{cluster="",module="exchange",node=""} 0
rabbitmq_module_up{cluster="",module="node",node=""} 0
rabbitmq_module_up{cluster="",module="overview",node=""} 0
rabbitmq_module_up{cluster="",module="queue",node=""} 0
# HELP rabbitmq_up Was the last scrape of rabbitmq successful.
# TYPE rabbitmq_up gauge
rabbitmq_up{cluster="",node=""} 0
```









