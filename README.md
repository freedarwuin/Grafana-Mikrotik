# Grafana-Mikrotik

![logo](https://repository-images.githubusercontent.com/366494855/c62052b8-17c2-47f2-a3ae-0e397a3ef074)

![visitors](https://visitor-badge.laobi.icu/badge?page_id=IgorKha.Grafana-Mikrotik)
![example branch parameter](https://github.com/IgorKha/Grafana-Mikrotik/actions/workflows/action.yml/badge.svg?branch=master)
![mikrotikOS](https://img.shields.io/badge/Mikrotik_ROS-v7.4-blue)
![Grafana](https://img.shields.io/badge/Grafana-v9.0.5-orange?logo=grafana)
![Prometheus](https://img.shields.io/badge/Prometheus-v2.37.0-red?logo=prometheus)
![snmp_exporter](https://img.shields.io/badge/snmp__exporter-v0.20.0-red?logo=prometheus)

[![Donate using Liberapay](https://liberapay.com/assets/widgets/donate.svg)](https://liberapay.com/~1772367/donate)

---

## 🐳 Deploy with docker-compose

### Deploy with bash script

```console
curl -fsSL https://raw.githubusercontent.com/freedarwuin/Grafana-Mikrotik/master/run.sh | bash -s -- --config
```

```console
  También puede pasar algunos argumentos al script para establecer algunas de estas opciones:

    --config: cambie el usuario y la contraseña a grafana y especifique la dirección IP de mikrotik

    --stop: detener los contenedores docker

    --help
```

Por ejemplo:

```console
    bash run.sh --config
```

[![asciicast](https://asciinema.org/a/nOhuc7LvI6bRWbg7dcvqFQ4Kc.png)](https://asciinema.org/a/nOhuc7LvI6bRWbg7dcvqFQ4Kc)

### implementar con docker-compose manual

1.Cambie la IP de destino (192.168.88.1) al archivo prometheus/prometheus.yml

2.Correr

```console
docker-compose up -d
```

3.Abra [localhost:3000](http://localhost:3000)

* Inicio de sesión de Grafana: `admin`

* Contraseña: `mikrotik`

Si desea cambiar las credenciales, edite el archivo ".env"

---

## Despliegue manual

1.añadir en prometheus.yml

```yml
  - job_name: Mikrotik
    static_configs:
      - targets:
        - 192.168.88.1  # SNMP device IP.
    metrics_path: /snmp
    params:
      module: [mikrotik]
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: localhost:9116  # The SNMP exporter's real hostname:port.
```

2.Configure Prometheus y ejecute /snmp/snmp_exporter

3.Agregar dashboard <https://grafana.com/grafana/dashboards/14420>

---

### Acoplador snmp_exportador

[![Docker Pulls](https://img.shields.io/docker/pulls/mashinkopochinko/snmp_exporter_mikrotik?logo=docker)](https://hub.docker.com/repository/docker/mashinkopochinko/snmp_exporter_mikrotik)

> amd64-linux container

```console
sudo docker run -d -p 9116:9116 mashinkopochinko/snmp_exporter_mikrotik:latest
```

---
![img1](/readme/screen.png)
