Prometheus
==========

Prometheus monitoring server, includes prometheus, alertmanager, blackbox-exporter and some bundled exporters.
This requires docker to be running (not handled by this role).


Parameters
----------

- `prometheus_targets`: Optional list of dictionaries of targets:
  - `group`: A list of Ansible groups (optional)
  - `hosts`: A list of hosts (optional)
  - `port`: The port to be monitored
  - `jobname`: The prometheus job-name
  - `scrape_interval`: Prometheus scrape interval (optional)
  - `metrics_path`: Path to metrics (optional)
- `prometheus_sd_targets`: Optional list of dictionaries of additional targets:
  - `groupname`: The name of the configuration target file, used to name a configuration file so must be unique
  - `group`: A list of Ansible groups (optional)
  - `hosts`: A list of hosts (optional)
  - `port`: The port to be monitored
  - `jobname`: The prometheus job-name

  This is intended to be an example.
  In practice these configuration files could be dynamically generated outside this role, prometheus will automatically reload them.
- `prometheus_custom_targets`: Optional list of dictionaries of additional targets with custom arguments, use this if you need to pass custom arguments that aren't supported by the previous two parameters.
  Each item is a [`<scrape_config>`](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#scrape_config) that will be copied unchanged into the configuration.
- `prometheus_port`: External Prometheus port, set to `0` to disable, default `9090`
- `prometheus_alertmanager_port`: External Alertmanager port, set to `0` to disable, default `9093`
- `prometheus_blackboxexporter_port`: External Blackbox-exporter port, set to `0` to disable, default `9115`
- `prometheus_additional_command_args`: Additional command line arguments for prometheus
- `prometheus_alertmanager_additional_command_args`: Additional command line arguments for alertmanager
- `prometheus_additional_rules_template`: Template with additional alert rules.
  See https://awesome-prometheus-alerts.grep.to/rules for some ideas but note the labels may be different.


- `prometheus_docker_network`: Docker network for prometheus Docker applications, default `prometheus`
- `prometheus_docker_user`: User ID that prometheus should run as, default is the container default
- `prometheus_docker_data_volume`: Docker volume or host path for Prometheus data, default is a docker volume called `prometheus-data`. If this is a host path it must be writeable by `prometheus_docker_user`.


Outputs
-------
This role sets the following variables which can be used in other tasks:
- `prometheus_internal_ip`: Internal IP of the Prometheus container
- `prometheus_blackboxexporter_internal_ip`: Internal IP of the Blackbox exporter container
- `prometheus_alertmanager_internal_ip`: Internal IP of the AlertManager container

These are intended for use when you don't want to expose the container ports using standard Docker port-forwarding (set `prometheus_*port: 0`).


Author Information
------------------

This role was originally forked from https://github.com/ome/ansible-role-prometheus
