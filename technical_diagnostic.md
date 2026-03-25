# Dossiê de Diagnóstico Técnico e Funcional: SFJ Backend (Lemon Alerts)

Este documento fornece uma análise profunda e investigativa do repositório `sfj-backend-python`, preparado para a sustentação e transição de conhecimento.

---

## 1. Resumo Executivo
- **Projeto**: Backend da plataforma "Lemon Alerts" (SFJ Technologies).
- **Objetivo**: Sistema de análise financeira e auditoria que monitora dados de empresas (tickers) para identificar riscos de manipulação contábil, falência e riscos de crédito.
- **Tipo de Sistema**: Backend API (REST) baseado em Django, com processamento em segundo plano (Celery) e integração com serviços AWS.
- **Tecnologias Core**: Python 3.12, Django 5.2, Celery, Redis, MySQL, Docker, AWS (ECS, S3, CloudWatch, Secrets Manager).
- **Principais Riscos**: Grande volume de lógica em arquivos monolíticos ([views.py](file:///c:/Users/myPC/Desktop/dev/job/sfj/sfj-backend-python/sfj_backend_python/tests/test_views.py) e [admin.py](file:///c:/Users/myPC/Desktop/dev/job/sfj/sfj-backend-python/sfj_backend_python/sfj_backend_python/v1/admin.py)), pipeline de CI/CD não totalmente versionado no repositório principal (indícios de CodePipeline/GitLab híbrido) e ausência de testes unitários abrangentes.

---

## 2. Visão Geral da Arquitetura
A solução segue um padrão de arquitetura distribuída baseada em containers:

- **Componentes Principais**:
    - **API (Web)**: Gunicorn + Nginx servindo a aplicação Django.
    - **Workers (Celery)**: Processamento de tarefas assíncronas (fetch de dados, geração de PDFs).
    - **Beat (Celery)**: Agendamento de tarefas periódicas.
    - **Cache/Broker**: Redis (rodando como sidecar ou serviço isolado).
    - **Banco de Dados**: MySQL (AWS RDS).
    - **Storage**: AWS S3 para documentos, relatórios e arquivos estáticos.

- **Fluxo Macro**:
    `Usuário -> ALB (AWS) -> ECS Service (Web) -> Django -> RDS/S3`
    `Cron/Beat -> Redis -> ECS Service (Worker) -> SFJ External API -> RDS`

- **Diagrama Textual**:
    ```text
    [Client] ---HTTPS---> [Application Load Balancer]
                                |
                [ECS Cluster: Service (Web Container)]
                    |          |          |
                    |    [RDS MySQL] [AWS S3]
                    |
                [Redis (Broker)]
                    |
    [ECS Service: Worker] <---> [ECS Service: Beat]
            |
    [External APIs: SFJ API, Perplexity, Anthropic]
    ```

---

## 3. Mapeamento Estrutural do Repositório
- `sfj_backend_python/`: Diretório raiz do código Django.
    - `sfj_backend_python/v1/`: Core da aplicação (Models, Views, Tasks, Utils).
    - [sfj_backend_python/settings.py](file:///c:/Users/myPC/Desktop/dev/job/sfj/sfj-backend-python/sfj_backend_python/sfj_backend_python/settings.py): Configurações globais (Sentry, AWS, MFA).
- `devops/`: Infraestrutura e Docker.
    - `cloudformation/`: Templates de infra AWS (ECS, Worker, Beat).
    - [docker/](file:///c:/Users/myPC/Desktop/dev/job/sfj/sfj-backend-python/devops/docker/base-image.docker): Dockerfiles e scripts de startup.
- [python-phil-local-notes.txt](file:///c:/Users/myPC/Desktop/dev/job/sfj/sfj-backend-python/python-phil-local-notes.txt): Documentação técnica valiosa sobre o processo de build manual e sincronização de DB.
- [pyproject.toml](file:///c:/Users/myPC/Desktop/dev/job/sfj/sfj-backend-python/pyproject.toml): Gerenciamento de dependências via Poetry.
- [Makefile](file:///c:/Users/myPC/Desktop/dev/job/sfj/sfj-backend-python/Makefile): Automação de linting, formatação e testes locais.

---

## 4. Como a Aplicação Funciona
- **Inicialização**: O container utiliza o [devops/docker/entrypoint.sh](file:///c:/Users/myPC/Desktop/dev/job/sfj/sfj-backend-python/devops/docker/entrypoint.sh), que ativa um venv, instala dependências com `poetry`, roda migrações e inicia o `supervisord`.
- **Pontos de Entrada**:
    - **Web**: Porta 8000 via Gunicorn.
    - **Jobs**: Celery Beat dispara tarefas agendadas em [v1/tasks.py](file:///c:/Users/myPC/Desktop/dev/job/sfj/sfj-backend-python/sfj_backend_python/sfj_backend_python/v1/tasks.py).
- **Fluxos Principais**:
    - **Alert Data Fetch**: A tarefa [fetch_alert_data_v2](file:///c:/Users/myPC/Desktop/dev/job/sfj/sfj-backend-python/sfj_backend_python/sfj_backend_python/v1/tasks.py#78-130) consome a API externa da SFJ, formata e salva dados de alerta no MySQL.
    - **Geração de Relatórios**: O [ReportViewSet](file:///c:/Users/myPC/Desktop/dev/job/sfj/sfj-backend-python/sfj_backend_python/sfj_backend_python/v1/views.py#534-604) dispara `generate_report_async` que utiliza LLMs (Anthropic/Perplexity) para gerar análises em Markdown e depois converte para PDF.
    - **MFA (Multi-Factor Auth)**: Implementado via `trench`, exigindo e-mail ou TOTP após o login JWT.

---

## 5. Stack Tecnológica Detalhada
- **Linguagem**: Python >= 3.12 (confirmado em [pyproject.toml](file:///c:/Users/myPC/Desktop/dev/job/sfj/sfj-backend-python/pyproject.toml)).
- **Framework**: Django 5.2 + Django REST Framework 3.16.
- **Segurança**: JWT (simplejwt), Trench (MFA), Django OTP.
- **AI/LLM**: Langchain, Anthropic, OpenAI, Perplexity (usados para geração de relatórios).
- **Infra/Cloud**: AWS ECS (EC2 launch type), S3, RDS MySQL, Route53, Secrets Manager.
- **Observabilidade**: Sentry (erros), CloudWatch (logs via `watchtower`).
- **Compliance**: Integrado com processos Vanta/SOC 2 (evidenciado por MFA, logging de auditoria e scans de imagem).

---

## 6. Ambientes Disponíveis
- **Local**: Configurado via [docker-compose-dev.yml](file:///c:/Users/myPC/Desktop/dev/job/sfj/sfj-backend-python/docker-compose-dev.yml) ou executado manualmente com `poetry`.
- **Staging**: `backend.staging.sfjtechnologies.com`.
- **Preprod**: `backend.preproduction.sfjtechnologies.com`.
- **Production**: `eu.sfjtechnologies.com`, `na.sfjtechnologies.com` (cluster geográfico).
- **Evidências**: Encontradas em [settings.py](file:///c:/Users/myPC/Desktop/dev/job/sfj/sfj-backend-python/sfj_backend_python/sfj_backend_python/settings.py) (ALLOWED_HOSTS) e nos templates de CloudFormation.

---

## 7. Processo de Build
1. **Base Image**: Construída a partir de [devops/docker/base-image.docker](file:///c:/Users/myPC/Desktop/dev/job/sfj/sfj-backend-python/devops/docker/base-image.docker) (Alpine + Redis + Supervisor).
2. **App Image**: [devops/docker/Dockerfile](file:///c:/Users/myPC/Desktop/dev/job/sfj/sfj-backend-python/devops/docker/Dockerfile) instala o código e dependências do Poetry.
3. **Comando**: `docker build -f devops/docker/Dockerfile -t sfj-backend .`.
4. **Artefatos**: Imagens Docker enviadas para o AWS ECR.

---

## 8. Processo de Deploy
- **Mecanismo**: AWS CodePipeline (referenciado em [codepipeline.yml](file:///c:/Users/myPC/Desktop/dev/job/sfj/sfj-backend-python/devops/cloudformation/legacy/codepipeline.yml) legado e notas).
- **Fluxo**:
    - Build no CodeBuild (Dockerfile).
    - Scan de vulnerabilidades (Trivy).
    - Update de CloudFormation stacks ([backend-app.yml](file:///c:/Users/myPC/Desktop/dev/job/sfj/sfj-backend-python/devops/cloudformation/backend-app.yml), etc.).
    - ECS Rolling Update (DeploymentCircuitBreaker: true).
- **Secrets**: Resolvidos via Secrets Manager e SSM direto no CloudFormation.

---

## 9. Segurança, Compliance e Governança (SOC 2 / Vanta)
- **MFA**: Obrigatório para login (evidência em [v1/urls.py](file:///c:/Users/myPC/Desktop/dev/job/sfj/sfj-backend-python/sfj_backend_python/sfj_backend_python/v1/urls.py) e [settings.py](file:///c:/Users/myPC/Desktop/dev/job/sfj/sfj-backend-python/sfj_backend_python/sfj_backend_python/settings.py)).
- **Audit Logs**: [APIRequestLog](file:///c:/Users/myPC/Desktop/dev/job/sfj/sfj-backend-python/sfj_backend_python/sfj_backend_python/v1/models.py#23-28) mapeia cada requisição, salvando usuário, IP e endpoint no banco (evidência: [v1/views.py](file:///c:/Users/myPC/Desktop/dev/job/sfj/sfj-backend-python/sfj_backend_python/sfj_backend_python/v1/views.py)).
- **Encryption**: Uso de TLS no e-mail e S3 Boto3 Storage com buckets privados.
- **Vulnerability Scanning**: Pipeline configurado para rodar `trivy` (imagem) e `bandit` (código).
- **Gaps**: `sent_default_pii=True` no Sentry pode capturar dados sensíveis involuntariamente.

---

## 10. Riscos e Débitos Técnicos
1. **Monolitos de Código (Severidade: Alta)**: [views.py](file:///c:/Users/myPC/Desktop/dev/job/sfj/sfj-backend-python/sfj_backend_python/tests/test_views.py) (>2700 linhas) e [admin.py](file:///c:/Users/myPC/Desktop/dev/job/sfj/sfj-backend-python/sfj_backend_python/sfj_backend_python/v1/admin.py) (>100k) dificultam manutenção e aumentam risco de bugs.
2. **Dependência de API Externa (Severidade: Média)**: A saúde dos dados depende da estabilidade da "SFJ External API".
3. **Logs em S3 (Severidade: Baixa)**: O processo de fetch de logs via ZIP ([s3FetchView](file:///c:/Users/myPC/Desktop/dev/job/sfj/sfj-backend-python/sfj_backend_python/sfj_backend_python/v1/views.py#619-663)) pode ser lento para grandes volumes.
4. **Testes (Severidade: Alta)**: Baixa cobertura de testes para lógica de negócio crítica em [v1/utils.py](file:///c:/Users/myPC/Desktop/dev/job/sfj/sfj-backend-python/sfj_backend_python/sfj_backend_python/v1/utils.py).

---

## 11. Runbook Operacional Inicial
- **Subir Local**: `docker-compose -f docker-compose-dev.yml up`.
- **Troubleshooting**: Verificar logs no CloudWatch Group `/ecs-cluster/${Environment}/backend`.
- **Migrações**: Executadas no startup via [entrypoint.sh](file:///c:/Users/myPC/Desktop/dev/job/sfj/sfj-backend-python/devops/docker/entrypoint.sh). Ver notas de Phil para casos de "Already Exists" (fake migrations).
- **Tasks**: Monitorar via Django Admin (django-celery-beat).

---

## 12. Recomendações Prioritárias
1. **Refatoração**: Quebrar [views.py](file:///c:/Users/myPC/Desktop/dev/job/sfj/sfj-backend-python/sfj_backend_python/tests/test_views.py) e [utils.py](file:///c:/Users/myPC/Desktop/dev/job/sfj/sfj-backend-python/sfj_backend_python/tests/test_utils.py) em módulos menores (ex: `views/reports.py`, `views/alerts.py`).
2. **Testes de Integração**: Implementar testes que validem o fluxo `Fetch Data -> Create Alert -> Send Email`.
3. **Revisão de PII**: Ajustar Sentry para filtrar tokens JWT e dados sensíveis de usuários.
4. **Documentação de API**: Ativar e revisar o Swagger ([MySwaggerView](file:///c:/Users/myPC/Desktop/dev/job/sfj/sfj-backend-python/sfj_backend_python/sfj_backend_python/v1/views.py#147-159)).

---
**Análise concluída com base nas evidências do repositório local.**
