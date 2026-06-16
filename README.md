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
| [release-tag](./release-tag/) | Gera e publica uma git tag semver calculada a partir do tipo do último commit (Conventional Commits) |
| [security-scan](./security-scan/) | Roda Snyk (dependências) e SonarQube (qualidade de código) |

---

## Como usar

Referencie qualquer action diretamente em um step do seu workflow passando credenciais como inputs:

```yaml
jobs:
  ci:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: actions/checkout@v4

      - uses: D2nke/actions/pre-build@main
        with:
          docker_registry: ${{ vars.DOCKER_REGISTRY }}
          fly_app_name: ${{ vars.FLY_APP_NAME }}

      - uses: D2nke/actions/build-docker@main
        with:
          image_name: ghcr.io/${{ github.repository_owner }}/meu-app
          ghcr_username: ${{ vars.GHCR_USERNAME }}
          ghcr_password: ${{ secrets.GHCR_PASSWORD }}
          docker_username: ${{ vars.DOCKER_USERNAME }}
          docker_password: ${{ secrets.DOCKER_PASSWORD }}

      - uses: D2nke/actions/security-scan@main
        with:
          snyk_token: ${{ secrets.SNYK_TOKEN }}
          sonar_token: ${{ secrets.SONAR_TOKEN }}
          sonar_org_key: ${{ vars.SONAR_ORG_KEY }}

      - uses: D2nke/actions/deploy@main
        with:
          environment: dev
          image_name: ghcr.io/${{ github.repository_owner }}/meu-app
          fly_api_token: ${{ secrets.FLY_API_TOKEN }}
          fly_org: ${{ vars.FLY_ORG }}

      - uses: D2nke/actions/release-tag@main
```

## Inputs de credenciais por action

Cada action recebe suas credenciais explicitamente via `with:`. Os secrets e vars continuam configurados no repositório chamador — você só os passa como inputs.

| Input | Action | Descrição |
|-------|--------|-----------|
| `ghcr_username` | build-docker | Usuário para login no GHCR |
| `ghcr_password` | build-docker | Token para autenticação no GHCR |
| `docker_username` | build-docker | Usuário para login no Docker Hub |
| `docker_password` | build-docker | Token para autenticação no Docker Hub |
| `fly_api_token` | deploy | Token de API do Fly.io |
| `fly_org` | deploy | Nome da organização no Fly.io |
| `gh_pat` | files-update | Personal Access Token do GitHub |
| `snyk_token` | security-scan | Token de autenticação do Snyk |
| `sonar_token` | security-scan | Token do SonarQube/SonarCloud |
| `sonar_org_key` | security-scan | Chave da organização no SonarCloud |

## Versionamento

Use `@master` para sempre acompanhar a versão mais recente, ou fixe em uma tag específica para mais controle:

```yaml
uses: D2nke/actions/build-docker@v1.2.0
```
