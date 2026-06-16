# build-docker

Build e push de imagem Docker para GHCR e Docker Hub com layer caching via GitHub Actions Cache.

## Uso

```yaml
- uses: D2nke/my_workflows/actions/build-docker@main
  with:
    image_name: ghcr.io/${{ github.repository_owner }}/meu-app
```

## Inputs

| Input | Obrigatório | Padrão | Descrição |
|-------|-------------|--------|-----------|
| `image_name` | sim | — | Nome completo da imagem (ex: `ghcr.io/org/app`) |
| `dockerfile` | não | `Dockerfile` | Caminho para o Dockerfile |
| `context` | não | `.` | Contexto do build Docker |
| `tag` | não | `github.sha` | Tag da imagem; usa o SHA do commit se omitido |

## Outputs

| Output | Descrição |
|--------|-----------|
| `image-uri` | URI completa da imagem com tag |

## Secrets necessários no repositório

| Secret | Descrição |
|--------|-----------|
| `DOCKER_PASSWORD` | Token para autenticação no Docker Hub |
| `GHCR_PASSWORD` | Token para autenticação no GHCR |

## Vars necessárias no repositório

| Var | Descrição |
|-----|-----------|
| `DOCKER_USERNAME` | Usuário para login no Docker Hub |
| `GHCR_USERNAME` | Usuário para login no GHCR |

## O que faz

1. Calcula a tag final (input ou SHA do commit)
2. Configura QEMU e Docker Buildx para build multi-plataforma
3. Autentica no Docker Hub e no GHCR
4. Extrai metadados da imagem
5. Realiza build e push com cache GHA (`linux/amd64` + `linux/arm64`)
