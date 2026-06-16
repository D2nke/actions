# Actions

Composite actions reutilizáveis para pipelines CI/CD — build, segurança, deploy e release em módulos independentes prontos para qualquer repositório.

## Problema

Cada time mantinha seu próprio pipeline do zero: Dockerfiles diferentes, scans de segurança inconsistentes, deploys feitos à mão. Qualquer mudança de padrão exigia atualizar dezenas de repositórios individualmente.

## Solução

Actions compostas versionadas em um repositório central. Um time atualiza aqui; todos os projetos que apontam para `@main` recebem a melhoria automaticamente.

## Impacto

- Padronização de pipelines em múltiplos repositórios a partir de uma única fonte
- Redução do tempo de setup de CI/CD de horas para minutos
- Scans de segurança aplicados uniformemente sem configuração por projeto

---

## Actions disponíveis

| Action | Descrição |
|--------|-----------|
| [build-docker](./build-docker/) | Build e push de imagem Docker para GHCR e Docker Hub com layer caching |
| [deploy](./deploy/) | Deploy de imagem Docker no Fly.io, criando o app automaticamente |
| [files-update](./files-update/) | Sincroniza arquivos de um repositório fonte para o repositório chamador |
| [pre-build](./pre-build/) | Valida variáveis de ambiente e estrutura obrigatória do projeto |
| [release-tag](./release-tag/) | Gera e publica uma git tag semver baseada no número do run |
| [security-scan](./security-scan/) | Roda Snyk (dependências) e SonarQube (qualidade de código) |

---

## Como usar

Referencie qualquer action diretamente em um step do seu workflow:

```yaml
jobs:
  ci:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: actions/checkout@v4

      - uses: D2nke/my_workflows/actions/pre-build@main

      - uses: D2nke/my_workflows/actions/build-docker@main
        with:
          image_name: ghcr.io/${{ github.repository_owner }}/meu-app

      - uses: D2nke/my_workflows/actions/security-scan@main

      - uses: D2nke/my_workflows/actions/deploy@main
        with:
          environment: dev
          image_name: ghcr.io/${{ github.repository_owner }}/meu-app

      - uses: D2nke/my_workflows/actions/release-tag@main
```

## Secrets e vars

As actions leem secrets e vars diretamente do repositório chamador — não é necessário passá-los como inputs. Configure-os uma vez no repositório e todas as actions os encontrarão automaticamente.

| Secret / Var | Usada em |
|--------------|----------|
| `secrets.DOCKER_PASSWORD` | build-docker |
| `secrets.GHCR_PASSWORD` | build-docker |
| `secrets.FLY_API_TOKEN` | deploy |
| `secrets.GH_PAT` | files-update |
| `secrets.SNYK_TOKEN` | security-scan |
| `secrets.SONAR_TOKEN` | security-scan |
| `vars.DOCKER_USERNAME` | build-docker |
| `vars.GHCR_USERNAME` | build-docker |
| `vars.FLY_ORG` | deploy |
| `vars.SONAR_ORG_KEY` | security-scan |

## Versionamento

Use `@master` para sempre acompanhar a versão mais recente, ou fixe em uma tag específica para mais controle:

```yaml
uses: D2nke/my_workflows/actions/build-docker@latest
```
