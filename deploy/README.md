# deploy

Deploy de imagem Docker no Fly.io. Cria o app automaticamente se ele ainda não existir.

## Uso

```yaml
- uses: D2nke/my_workflows/actions/deploy@main
  with:
    environment: dev
    image_name: ghcr.io/${{ github.repository_owner }}/meu-app
    image_tag: ${{ github.sha }}
```

## Inputs

| Input | Obrigatório | Padrão | Descrição |
|-------|-------------|--------|-----------|
| `environment` | sim | — | Ambiente alvo: `dev`, `staging` ou `prd` |
| `image_name` | sim | — | Imagem Docker completa (ex: `ghcr.io/user/app`) |
| `fly_app` | não | nome do repositório | Nome do app no Fly.io |
| `image_tag` | não | `latest` | Tag da imagem Docker |

## Secrets necessários no repositório

| Secret | Descrição |
|--------|-----------|
| `FLY_API_TOKEN` | Token de API do Fly.io |

## Vars necessárias no repositório

| Var | Descrição |
|-----|-----------|
| `FLY_ORG` | Nome da organização no Fly.io |

## O que faz

1. Instala o Fly CLI (`flyctl`)
2. Valida a conexão com o Fly.io
3. Monta o nome final do app no padrão `{org}-{app}-{owner}` e cria se não existir
4. Faz deploy da imagem com estratégia `rolling` (zero downtime)
5. Exibe o status final do app
