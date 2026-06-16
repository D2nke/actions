# build-docker

Build e push de imagem Docker para GHCR e Docker Hub com layer caching via GitHub Actions Cache.

## Uso

```yaml
- uses: D2nke/actions/build-docker@main
  with:
    image_name: ghcr.io/${{ github.repository_owner }}/meu-app
    ghcr_username: ${{ vars.GHCR_USERNAME }}
    ghcr_password: ${{ secrets.GHCR_PASSWORD }}
    docker_username: ${{ vars.DOCKER_USERNAME }}
    docker_password: ${{ secrets.DOCKER_PASSWORD }}
```

## Inputs

| Input | Obrigatório | Padrão | Descrição |
|-------|-------------|--------|-----------|
| `image_name` | sim | — | Nome completo da imagem (ex: `ghcr.io/org/app`) |
| `ghcr_username` | sim | — | Usuário para login no GHCR |
| `ghcr_password` | sim | — | Token para autenticação no GHCR |
| `docker_username` | sim | — | Usuário para login no Docker Hub |
| `docker_password` | sim | — | Token para autenticação no Docker Hub |
| `dockerfile` | não | `Dockerfile` | Caminho para o Dockerfile |
| `context` | não | `.` | Contexto do build Docker |
| `tag` | não | `github.sha` | Tag da imagem; usa o SHA do commit se omitido |

## Outputs

| Output | Descrição |
|--------|-----------|
| `image-uri` | URI completa da imagem com tag |

## O que faz

1. Calcula a tag final (input ou SHA do commit)
2. Configura QEMU e Docker Buildx para build multi-plataforma
3. Autentica no Docker Hub e no GHCR
4. Extrai metadados da imagem
5. Realiza build e push com cache GHA (`linux/amd64` + `linux/arm64`)
