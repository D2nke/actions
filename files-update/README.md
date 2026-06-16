# files-update

Sincroniza arquivos de um repositório fonte para o repositório chamador via commit direto na branch atual.

## Uso

```yaml
jobs:
  sync:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: D2nke/my_workflows/actions/files-update@main
        with:
          source-repo: D2nke/my_workflows
          source-dir: actions/build-docker
          target-dir: .github/actions/build-docker
```

## Inputs

| Input | Obrigatório | Padrão | Descrição |
|-------|-------------|--------|-----------|
| `source-repo` | sim | — | Repositório fonte no formato `org/repo` |
| `source-dir` | sim | — | Diretório dentro do repositório fonte |
| `target-dir` | não | `.github/workflows` | Diretório de destino no repositório chamador |
| `dry-run` | não | `false` | Se `true`, executa sem fazer commit |

## Secrets necessários no repositório

| Secret | Descrição |
|--------|-----------|
| `GH_PAT` | Personal Access Token com permissão de escrita no repositório chamador |

## Permissões necessárias no job

```yaml
permissions:
  contents: write
```

## O que faz

1. Faz checkout do repositório chamador e do repositório fonte em diretórios separados
2. Copia os arquivos do diretório fonte para o diretório destino
3. Detecta se houve mudanças
4. Se houver mudanças e `dry-run` for `false`, faz commit e push direto na branch atual
