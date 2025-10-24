atlas/
  README.md                ← Ponto de partida (como rodar, contribuir)
  pyproject.toml           ← Dependências e ferramentas (ruff, black, mypy, pytest)
  Dockerfile               ← Empacotamento para rodar igual em qualquer máquina
  docker-compose.yml       ← (Opcional) Sobe serviços de apoio (ex.: Redis, DB)
  Makefile                 ← Atalhos: fmt, lint, test, run, build
  .env.example             ← Variáveis de ambiente (sem segredos)
  .github/workflows/ci.yml ← CI (lint + typecheck + tests + build)

  atlas/                   ← Código-fonte do produto (pacote Python)
    __init__.py
    configs/               ← Configurações por ambiente (validadas)
      base.yml
      dev.yml
      prod.yml
      schemas/             ← Esquemas Pydantic garantindo config correta
        config.py
    utils/                 ← Utilidades transversais
      logger.py            ← Logging estruturado (JSON) com correlação
      helpers.py
      time.py              ← Funções de tempo seguras (timezone-aware)
    io/                    ← Formatos de entrada/saída padronizados
      models.py            ← Pydantic: eventos, alertas, resultados
      adapters/            ← Conectores (arquivos, S3, Kafka, etc.)
        files.py
        s3.py
        kafka.py
    core/                  ← “Motor” do sistema
      pipeline.py          ← Orquestra N analisadores; agrega resultados
      scheduler.py         ← Agendador/assíncrono (opcional)
      repositories/        ← Persistência (memória, FS, SQL) isolada do core
        memory.py
        filesystem.py
        sql.py
      services/            ← Regras/Enriquecimento (lógica de negócio)
        rules_engine.py
        enrichment.py
    analyzers/             ← “Cérebros” do Atlas (plugins)
      base_analyzer.py     ← Contrato único: analyze(Input)->Output + metadata()
      behavior_analyzer.py
      log_analyzer.py
      network_analyzer.py
      threat_intel_analyzer.py
      plugins/             ← (Opcional) Descoberta dinâmica
    interfaces/            ← Ponto de entrada para usuários/automação
      cli.py               ← CLI (Typer): run, validate-config, list-analyzers
      api/                 ← (Opcional) FastAPI p/ servir/monitorar
        app.py
        routers/
          health.py
          runs.py
          analyzers.py

  tests/                   ← Testes (unit → integration → e2e)
    unit/
    integration/
    e2e/
    data/fixtures/         ← Dados sintéticos base
  docs/                    ← Documentação do time/produto
    architecture.md        ← Diagramas + fluxo (mermaid/C4)
    analyzers.md           ← Como criar/estender analisadores
    ops.md                 ← Observabilidade, deploy, troubleshooting
    configs.md             ← Esquemas, exemplos e overrides
