# victoriametrics
This is just a few step for installing victoria metrics

Step:

1. Download victoria from the official site

```bash
wget https://github.com/VictoriaMetrics/VictoriaMetrics/releases/download/v1.81.2/victoria-metrics-linux-amd64-v1.81.2.tar.gz
```

2. Extract and adduser,group
```bash
sudo useradd -rs /bin/false victoriametrics
tar -xvf victoria-metrics-linux-amd64-v1.81.2.tar.gz
chown victoriametrics:victoriametrics victoria-metrics-linux-amd64-v1.81.2/victoria-metrics-prod
mv victoria-metrics-linux-amd64-v1.81.2/victoria-metrics-prod /usr/local/bin/
```

3. Prepare storage path
```bash
mkdir /data
chown -R victoriametrics:victoriametrics /data
```

4. Create service
```bash
We already prepare it
```

5. Start
```bash
systemctl daemon-reload
systemctl enable --now victoriametrics.service
systemctl start victoriametrics.service
systemctl status victoriametrics.service
```

### Additional segmen for prometheus that used to push it's metrics into victoriametrics

1. Edit prometheus.yml

```yaml
# just add this at end of line

...

remote_write:
  - url: http://victoriametrics-ip:8428/api/v1/write
    queue_config:
      max_samples_per_send: 10000
      capacity: 20000
      max_shards: 30

...
```

2. Then restart

```bash
systemctl restart prometheus
```
