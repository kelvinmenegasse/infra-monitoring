# Infra Monitoring

Stack de monitoramento com Prometheus e Grafana.

## Quick Start

```bash
docker compose up -d
```

## Servicos

| Servico | Porta | Descricao |
|---------|-------|-----------|
| Prometheus | 9090 | Coleta de metricas |
| Grafana | 3000 | Visualizacao de metricas |

## Acessos

| Servico | URL | Credenciais |
|---------|-----|-------------|
| Prometheus | http://localhost:9090 | - |
| Grafana | http://localhost:3000 | admin / admin |

## Variaveis de Ambiente

| Variavel | Padrao | Descricao |
|----------|--------|-----------|
| `PROMETHEUS_PORT` | `9090` | Porta do Prometheus |
| `GRAFANA_PORT` | `3000` | Porta do Grafana |
| `GRAFANA_ADMIN_USER` | `admin` | Usuario admin do Grafana |
| `GRAFANA_ADMIN_PASSWORD` | `admin` | Senha admin do Grafana |
| `GRAFANA_ROOT_URL` | `http://localhost:3000` | URL raiz do Grafana |

## Estrutura

```
infra-monitoring/
├── docker-compose.yml
├── prometheus/
│   └── prometheus.yml     # Configuracao do Prometheus
└── grafana/
    ├── provisioning/
    │   ├── datasources/
    │   │   └── datasources.yml
    │   └── dashboards/
    │       └── dashboards.yml
    └── dashboards/
        └── keycloak.json   # Dashboard do Keycloak
```

## Configurando Targets

Edite `prometheus/prometheus.yml` para adicionar novos targets:

```yaml
scrape_configs:
  - job_name: 'minha-aplicacao'
    static_configs:
      - targets: ['minha-app:8080']
```

## Dashboards Inclusos

- **Keycloak Metrics**: Metricas do Keycloak (JVM, logins, sessoes)

## Adicionando Dashboards

1. Crie o arquivo JSON em `grafana/dashboards/`
2. O dashboard sera carregado automaticamente

## Metricas Coletadas

### Keycloak

- `keycloak_logins_total`: Total de logins
- `keycloak_failed_login_attempts_total`: Tentativas de login falhas
- `keycloak_sessions_active`: Sessoes ativas
- `jvm_memory_used_bytes`: Uso de memoria JVM

### Redis (opcional)

- `redis_connected_clients`: Clientes conectados
- `redis_used_memory_bytes`: Memoria usada

## Alertas

Edite `prometheus/prometheus.yml` para adicionar regras de alerta:

```yaml
rule_files:
  - /etc/prometheus/alerts.yml
```

## Retencao de Dados

Por padrao, o Prometheus mantem dados por 30 dias. Para alterar:

```yaml
command:
  - '--storage.tsdb.retention.time=90d'
```

## Network

O servico cria a rede `monitoring-network` que pode ser usada por outros containers:

```yaml
networks:
  monitoring-network:
    external: true
    name: monitoring-network
```
