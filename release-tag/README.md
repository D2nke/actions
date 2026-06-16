# release-tag

Gera e publica uma git tag semver (`v0.0.{run_number}`) automaticamente ao final do pipeline.

## Uso

```yaml
- uses: D2nke/my_workflows/actions/release-tag@main
```

Não requer inputs nem secrets.

## Permissões necessárias no job

```yaml
permissions:
  contents: write
```

## O que faz

1. Faz checkout com histórico completo (`fetch-depth: 0`)
2. Gera a versão no formato `v0.0.{GITHUB_RUN_NUMBER}`
3. Cria uma tag anotada localmente
4. Faz push da tag para o repositório remoto

## Exemplo de tag gerada

Para o run número 42: `v0.0.42`
