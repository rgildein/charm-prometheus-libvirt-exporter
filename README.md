# Juju prometheus libvirt exporter charm

This charm provides the [Prometheus libvirt exporter](https://github.com/kumina/libvirt_exporter)

## Testing

# This directory needs to be create in the charm path prior to testing
```
mkdir -p report/lint
```

## Deployment

# A typical deployment with nova and libvirt is as follows:
# The metrics will be at http://nova-compute:9177
```
juju deploy nova-compute 
juju deploy prometheus-libvirt-exporter
juju add-relation nova-compute prometheus-libvirt-exporter
```

# To avail of the metrics in grafana the following steps can be used
```
juju deploy grafana
juju deploy prometheus2
juju add-relation prometheus-libvirt-exporter:scrape prometheus2:target
juju add-relation prometheus-libvirt-exporter:dashboards grafana:dashboards
```

# To setup reporting with nagios
```
juju deploy nrpe
juju add-relation nova-compute nrpe
juju add-relation prometheus-libvirt-exporter:nrpe-external-master nrpe:nrpe-external-master
```

# Change or update dashboards
```
# The exporter is distributed with a standard dashboard
# To provide your own dashboards, create a zip file and attach it as a resource 
zip grafana-dashboards.zip libvirt-simple.json libvirtadvanced.json
juju attach-resource prometheus-libvirt-exporter dashboards=./grafana-dashboards.zip
```

# Contact Information
- Charm bugs: https://bugs.launchpad.net/charm-prometheus-libvirt-exporter
