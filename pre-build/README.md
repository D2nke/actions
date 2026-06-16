# pre-build

Valida variáveis de ambiente e estrutura obrigatória do projeto antes de iniciar o build. Use como primeiro step do pipeline para falhar cedo e evitar builds inválidos.

## Uso

```yaml
- uses: D2nke/my_workflows/actions/pre-build@main
```

Não requer inputs, secrets nem vars.

## O que valida

| Verificação | Comportamento em falha |
|-------------|----------------------|
| `DOCKER_REGISTRY` ou `FLY_APP_NAME` definido | Aviso (não bloqueia) |
| `Dockerfile` presente na raiz | Erro — interrompe o pipeline |
| `package.json`, `pom.xml` ou `setup.py` presente | Erro — interrompe o pipeline |

## Vars lidas no repositório (opcionais)

| Var | Descrição |
|-----|-----------|
| `DOCKER_REGISTRY` | Registry de destino (apenas verificada) |
| `FLY_APP_NAME` | Nome do app no Fly.io (apenas verificada) |

## O que faz

1. Exibe contexto do run (trigger, ref, SHA, ambiente)
2. Verifica se ao menos uma estratégia de deploy está configurada
3. Confirma que `Dockerfile` existe na raiz do projeto
4. Confirma que existe ao menos um arquivo de build (`package.json`, `pom.xml` ou `setup.py`)
