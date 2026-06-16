# pre-build

Valida variáveis de ambiente e estrutura obrigatória do projeto antes de iniciar o build. Use como primeiro step do pipeline para falhar cedo e evitar builds inválidos.

## Uso

```yaml
- uses: D2nke/actions/pre-build@main
  with:
    docker_registry: ${{ vars.DOCKER_REGISTRY }}   # opcional
    fly_app_name: ${{ vars.FLY_APP_NAME }}         # opcional
```

## Inputs

| Input | Obrigatório | Padrão | Descrição |
|-------|-------------|--------|-----------|
| `docker_registry` | não | — | Registry de destino (apenas verificada) |
| `fly_app_name` | não | — | Nome do app no Fly.io (apenas verificada) |

## O que valida

| Verificação | Comportamento em falha |
|-------------|----------------------|
| `docker_registry` ou `fly_app_name` definido | Aviso (não bloqueia) |
| `Dockerfile` presente na raiz | Erro — interrompe o pipeline |
| `package.json`, `pom.xml` ou `setup.py` presente | Erro — interrompe o pipeline |

## O que faz

1. Exibe contexto do run (trigger, ref, SHA, ambiente)
2. Verifica se ao menos uma estratégia de deploy está configurada
3. Confirma que `Dockerfile` existe na raiz do projeto
4. Confirma que existe ao menos um arquivo de build (`package.json`, `pom.xml` ou `setup.py`)
